# :blue_book: Multiuser Configurable DataView Web Dashboard - Frontend

## Create a new widget
To create a new widget you must extend the `BaseWidget` and `BaseWidgetComponent` classes respectively for the `Widget` and `WidgetComponent` classes.

Then you must decorate your `Widget` with the `WidgetDecorator` which takes 5 fields to set the configuration.

**Pie chart widget** example:
```javascript
@WidgetDecorator({
    endpoint: 'tuple',
    type: WidgetTypeEnum.MOBILITY,
    sourceable: true,
    refreshableInfo: {
        refreshable: true,
        defaultRefreshRate: RefreshRateEnum.sec_5
    },
    adapter: PieChartAdapter,
})
export class PieChartWidget extends BaseWidget {

...

}
```


And this is the **structure** of the options passed to the decorator:

```javascript
export interface WidgetOptions {
    endpoint: string;
    type: WidgetTypeEnum;
    sourceable?: boolean;
    refreshableInfo?: RefreshableInfo; 
    adapter?: any;
}

export interface RefreshableInfo {
    refreshable: boolean;
    defaultRefreshRate?: RefreshRateEnum;
}
```

- `endpoint`: indicates which endpoint the widget has to use to take data from  
- `type`: indicates the widget type (currently IT/MOBILITY)
- `sourceable` (optional): indicates whether or not the widget has a selectable source parameter
- `refreshableInfo` (optional): indicates whether or not the widget has a refresh parameter and can be used to set the default refresh rate
- `adapter` (optional): indicates the adapter class used to transform received data into a more useful form

You can create an Adapter class implementing the `Adapter` functional interface and the `adapt` method.

```javascript
export interface Adapter {
    adapt(dto: any): any;
}
```

Lastly inside the `WidgetComponent` you must implement the `getDataCallback` method where `value` is the data received from the backend.  This method will be called everytime the widget receives data.

**Note**: `value` will be adapted data if an `Adapter` for that class exists.

**Pie chart widget component** example:

```javascript
@Component({
    selector: 'app-pie-chart-widget',
    templateUrl: './pie-chart.component.html',
    encapsulation: ViewEncapsulation.None,
    changeDetection: ChangeDetectionStrategy.OnPush
})
export class PieChartComponent extends BaseWidgetComponent<PieChartWidget> {

    public data: TupleData[] = [];
  
    ...

    protected getDataCallback(value: any): void {
        this.data = value;
    }
}
```


<br />

# Loccioni Aulos Web GetStarted

This project was generated with [Angular CLI](https://github.com/angular/angular-cli).

## Register @aulos packages repository
To install @aulos packages from Loccioni npm repository it is necessary to add the user credentials using the following command:

npm login --scope=@aulos --registry=https://nuget.loccioni.com/repository/npm-aulos-web-next/

See official npm documentation: https://docs.npmjs.com/logging-in-to-an-npm-enterprise-registry-from-the-command-line

## Using Mock Server
Run `npm run server:start` from a terminal.

Check `config.json` file and see baseUrl value, to work with expressjs mock server has to be: `http://@localHostName:9000/webservice`

## Using AspNet Core 3.1 Backend
Start project: https://git.loccioni.com/-/ide/project/aulos/web/loccioni-aulos-getstarted/backend.

Check `config.json` file and see baseUrl value, to work with expressjs mock server has to be: `https://@localHostName:5001/api`

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).

# 
