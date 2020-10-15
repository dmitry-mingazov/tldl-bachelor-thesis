# :closed_book: Multiuser Configurable DataView Web Dashboard - Backend

## Installation
### Register Loccioni.Aulos packages repository
Just add on nuget sources:

https://nuget.loccioni.com/repository/nuget-aulos-csharp-next/

See: https://docs.microsoft.com/it-it/nuget/consume-packages/install-use-packages-visual-studio#package-sources

## Widget Setup
- Implement the data access layer
- Add a `{sourcecode}.json` file in `DiscoveryConfiguration/parameters`
  - Example:
    ```json
    {
        "metadata": {
            "meta": {
                "fieldName": "label"
            },
        "properties": {
            "color": [
                {
                    "field": "Passed",
                    "value": "green"
                },
                {
                    "field": "Refused",
                    "value": "red"
                }
            ],
            "field": [
                {
                    "field": "Passed",
                    "value": "ok"
                },
                {
                    "field": "Refused",
                    "value": "ko"
                }
            ]
        }
        },
        "filters": [
            {
                "Code": "time-span",
                "ShortText": "Time span",
                "LongText": "T i m e   s p a n",
                "Type": "number",
                "DefaultValue": "7",
                "Editor": {
                    "Type": "EnumEditor",
                    "Path": "@aulos/parameter-editors",
                    "Options": {
                        "comboValues": [
                            {
                                "label": "1 week",
                                "value": 7
                            },
                            {
                                "label": "2 weeks",
                                "value": 14
                            },
                            {
                                "label": "1 month",
                                "value": 28
                            },
                            {
                                "label": "2 months",
                                "value": 56
                            },
                            {
                                "label": "Forever",
                                "value": 9999
                            }
                        ]
                    }
                }
            }   
        ]
    }
    ```
    Where filters is an array of AULOS JSON parameters and metadata defines how the dto is modified for better frontend interpretation: `fieldName` defines the name of the dto unique identifier while the properties field defines parameters that will be changed in the frontend such as Kendo chart field colors and field names.
- Define a `DataSource` class to mediate between the controller and data access class/es
- Extend the `DiscoveryConfiguration` class of the desired type (extend base class if non-existant)
  - Call `AddSource` method in constructor
    - `TDataSource` should be the type of your datasource
    - `SourceCode` must be unique and the same as the previously created filename of the .json
    ```csharp
    public class TupleDiscoveryConfiguration : DiscoveryConfiguration
    {
        public TupleDiscoveryConfiguration()
        {
            AddSource(new SourceInfo
            {
                TDataSource = typeof(AllRefusedDataSource),
                SourceCode = "allcode",
                LongText = "All refused",
                ShortText = "All refused"
            });

            AddSource(new SourceInfo
            {
                TDataSource = typeof(RefusedTypeDataSource),
                SourceCode = "refusedtype",
                LongText = "Refused type",
                ShortText = "Refused type"
            });
        }
    }
    ```
- Define a controller if handling a new data type or otherwise use an already existant one 
  - Single POST method taking `GenericRequestDTO` argument
  - Call `SourceManagementService` `getDataSource<dataSource>` where `dataSource` is the `DataSource` class/interface you need
    ```csharp
    [HttpPost]
        public IActionResult Search([FromBody] GenericRequestDTO request)
        {
            var dataSource = _sourceManagementService.GetDataSource<ITupleDataSource>(request.SourceCode);
            return new ObjectResult(dataSource.GetData(request.Filters));
        }
    ```
