# Angular

### Why
It allows us to develop large applications in a maintainable manner.  
- *Custom components* : Angular allows you to build your own declarative components that can pack
functionality along with its rendering logic into bite-sized, reusable pieces. It also
plays well with web components.  
- *Data Binding*: Angular allows you to seamlessly move your data from your core JavaScript code
to the view, and react to view events without having to write the glue code your‐
self.
-  *Dependency injection*: Angular allows you to write modular services, and have them injected wherever they are needed. This greatly improves the testability and reusability of the same.
-  *Testing*: Tests are first-class citizens, and Angular has been built from the ground up with
testability in mind. You can  test every part of your application.
-  *Comprehensive*: Angular is a full-fledged framework, and provides out-of-the-box solutions for
server communication, routing within your application, and more.

### Commands 

##### Check angular cli version 
`ng --version`

##### Creating a new application 
`ng new stock-market`

##### run in development mode 
`ng serve`

##### generate component 
`ng generate component stock/stock-item`   
(stock/stock-item means stock-item component generated in stock folder, if you don't specify folder it will be generated in the `app` folder itself) 
The `spec.ts` file is the skeleton unit tests for this component

##### generate class 
`ng generate class model/stock`

### Running the application 

all code is packed into `app-root` and scripts are injected while running by `ng-serve` into the index.html

#### Development mode 
Angular CLI compiles the changes  as it happens and refreshes our UI

#### Production mode 
an optimal compiled build, served via static files

#### files made when running application 
- main.js: transpiled code specific to application we are building
-  vendor.js: third-party libraries and frameworks (including Angular) 
-  styles.js: all css styles 
-  polyfills.js: polyfills needed for supporting some capabilities in older browsers (like advanced ECMA‐
 Script features not yet available in all browsers).
-  runtime.js : runtime.js is the webpack loader. This file contains webpack utilities that are needed to load other files

#### core files 
- index.html file is responsible for deciding which files are to be loaded
-  main.ts file identifies which Angular module is to be loaded when the application starts. It can
also change application-level configuration (like turning off framework-level asserts
and verifications using the enableProdMode() flag),
- app.module.ts: where application-specific source code starts from. it can be thought of as the core configuration of application, from loading all the relevant and necessary dependencies, declaring which components will be used within  application, to marking which is the main entry point component of  application:  
    - `ngModule` is a typescript annotation([decorators](https://www.typescriptlang.org/docs/handbook/decorators.html)) to mark this class definition as an Angular module
    - Declarations marking out which components and directives can be used within
        the application. Any component that you create must be declared before it can be used
    -  Importing other modules that provide functionality needed in the application
    -  [bootstrap](https://stackoverflow.com/questions/1254542/what-is-bootstrapping): The entry point component for starting the application
- app.component.ts: main component
    - selector : css selector , identifies the directive in the template and triggers its instantiation 
    - AppComponent: component class with its own members and functions 

##### Component
A component in Angular is nothing but a TypeScript class, decorated with some
attributes and metadata. The class encapsulates all the data and functionality of the
component, while the decorator specifies how it translates into the HTML.

Angular gives us hooks into the lifecycle of a component to let us take certain actions
when a component is initialized, when its view is rendered, when it is destroyed, and
so on.

- OnInit: Angular’s OnInit hook is executed after the component is created by the Angular
framework, after all the data fields are initialized
-  OnInit is an interface, ngOnInit is a method, When you want to hook on the initialization phase of a component, you need to implement the OnInit interface, and then implement the ngOnInit function in the component, which is where you write your initialization logic.
-   constructor is called to initialize the class and not the component(you can bring in the dependencies here but not initialize the values). The constructor is called before ngOnInit , at which point the component hasn't been created yet, so if you place your initialization code in constructor it won't run

###### Interpolation 
double-curly notation ( {{ }} ), which is also known as interpolation. Interpolation evaluates the expression between the curly braces as per the component backing it,

Angular also provides a way to bind not just text, but also DOM element properties. this is called property binding 

##### Property Binding 
binds the value of the expression to the DOM property between the square brackets. The [] is the general syntax that can be used with any property on an element to bind one-way from the compo‐
nent to the UI.

```
<div class="stock-container">
    <div class="name">
        <h3>{{name}}</h3> - <h4>{{code}}</h4>
    </div>
    <div class="price" [class]="positiveChange ? 'positive': 'negative'">$ {{price}}</div>
</div>
```

When you bind to the class property like we did in the example, note that it overrides the existing value of the property.
Angular data binding only works with DOM properties, and not with HTML attributes.
Attributes are defined by HTML, while
properties are defined by the Document Object Model. Though some attributes (like ID and class) directly map to DOM properties, others may exist on one side but not the other. [html attributes vs dom properties](https://stackoverflow.com/questions/6003819/what-is-the-difference-between-properties-and-attributes-in-html)

##### Event binding 

```
<div class="stock-container">
    <div class="name">
        <h3>{{name}}</h3> - <h4>{{code}}</h4>
    </div>
    <div class="price" [class]="positiveChange ? 'positive': 'negative'">$ {{price}}</div>
    <button (click) = "toggleFavorite()" [disabled] = "favorite">Add to Favorite</button>
</div>
```

`(click)="toggleFavorite()"`   
This syntax is called event binding in Angular. The left part of the equals symbol refers
to the event we are binding to. The right part of the equals symbol then refers to the template statement that Angular should execute whenever the event is triggered.

There are times when we might also care about the actual event triggered. In those cases, Angular gives you access to the underlying DOM event by giving access to a special variable `$event` . You can access it or even pass it to your function as follows:

```
<div class="stock-container">
    <div class="name">
        <h3>{{name}}</h3> - <h4>{{code}}</h4>
    </div>
    <div class="price" [class]="positiveChange ? 'positive': 'negative'">$ {{price}}</div>
    <button (click) = "toggleFavorite($event)" [disabled] = "favorite">Add to Favorite</button>
</div>
```

we change the toggleFavorite function like so 

```
toggleFavorite(event: any){
    console.log("We are toggling the favorite state for this stock", event);
    this.favorite = !this.favorite;
  }
  ```

###### class vs interface
A class is a blueprint from which we can create objects that share the same configuration - properties and methods. An interface is a group of related properties and methods that describe an object, but neither provides implementation nor initialisation for them.

##### using models 

```
export class Stock {
    favorite: boolean = false;
    constructor(public name: string, public code: string, public price: number, public previousPrice: number){

    }
    isPositiveChange(): boolean{
        return this.price >= this.previousPrice;
    }
}
```

Note that we didn’t actually define the variables name , code , and so on as properties of the class. This is because we are using TypeScript’s syntactic sugar to automatically create the corresponding properties based on the constructor arguments by using the public keyword

you can use this class by importing it like 
`import { Stock } from '../../model/stock';`  
and then   

```
public stock!: Stock; //inside the class 
...
this.stock = new Stock('Test Stock Company', 'TSC', 85, 80);  //inside some method
```

## Builtin Angular Directives 

A directive in Angular allows you to attach some custom functionality to elements in your HTML. A component in Angular , is a directive that provides both functionality and UI logic.

- Attribute Directives : Attribute directives change the look and feel, or the behavior, of an existing element or component that it is applied on. Example: NgClass, NgStyle
- Structural Directives: Structural directives change the DOM layout by adding or removing elements
from the view. NgIf and NgFor are examples of structural directives
(Note: NgClass is the class name of the directive, ngClass would be the attibute name )

### NgClass

The NgClass directive allows us to apply or remove multiple CSS classes simultaneously from an element in our HTML
Angular provides the NgClass directive, which can take a JavaScript object as input. For each key in the object that has a truthy value, Angular will add that key (the key itself, not the value of the key!) as a class to the element. Similarly, each key in the object that has a falsy value will be removed as a class from that element.

```
ngOnInit(): void {
    this.stock = new Stock('Test Stock Company', 'TSC', 85, 80);
    let diff = (this.stock.price / this.stock.previousPrice) - 1;
    let largeChange = Math.abs(diff) > 0.01;
    this.stockClasses = {
      "positive": this.stock.isPositiveChange(),
      "negative": !this.stock.isPositiveChange(),
      "large-change": largeChange,
      "small-change": !largeChange
    }
  }
  ```

  ```
  <div class="price" [ngClass]="stockClasses">$ {{stock.price}}</div>
  ```

unlike before, where the class binding was overwriting our initial class from the element, the NgClass directive retains the classes on the element.

### NgStyle  
NgStyle directive works at a CSS style/properties level. The keys and values it expects are CSS properties and attributes rather than class names.

```
this.stockStyles = {
    "color": this.stock.isPositiveChange() ? "green" : "red",
    "font-size": largeChange ? "1.2em" : "0.8em"
};
```

```
<div class="price" [ngStyle]="stockStyles">$ {{stock.price}}</div>
```

### third way of adding and removing classes and styles 

We can add or remove individual classes based on evaluating a truthy expression in Angular with the following syntax: `[class.class-name]="expression"` 

```
<div class="price"
    [class.positive]="stock.isPositiveChange()"
    [class.negative]="!stock.isPositiveChange()">$ {{stock.price}}</div>
```

this also keeps the original class intact  

for style 

```
[style.background-color]="stock.isPositiveChange() ? 'green' : 'red'"
```

### BuiltIn Structural Directives 
All structural directives in Angular start with an asterisk (*)
Unlike the data- or event-binding syntaxes, there are no square brackets or parentheses.

#### NgIf 
The NgIf directive allows you to conditionally hide or show elements in your UI.
(removing the element can be beneficial such as if it is resource intensive)

```
<button (click) = "toggleFavorite($event)" *ngIf="!stock.favorite">Add to Favorite</button>
```

#### NgFor 
NgFor directive is used for creating multiple elements, usually one for each instance of some or the other object in an array.
(NgFor uses classes NgForOf so same thingy)

```
public stocks!: Array<Stock>;
...
ngOnInit(): void {
this.stocks = [
      new Stock('Test Stock Company', 'TSC', 85, 80),
      new Stock('Second Stock Company', 'SSC', 5, 15),
      new Stock('Last Stock Company', 'LSC', 20, 19)
    ]
}

toggleFavorite(event: any, index: number){
    console.log("We are toggling the favorite state for this stock", event);
    this.stocks[index].favorite = ! this.stocks[index].favorite;
  }
```

```
<div class="stock-container" *ngFor="let stock of stocks; index as i">
    <div class="name">
        <h3>{{stock.name}}</h3> - <h4>{{stock.code}}</h4>
    </div>
    <div class="price" [ngClass]="stockClasses">$ {{stock.price}}</div>
    <button (click) = "toggleFavorite($event, i)" *ngIf="!stock.favorite">Add to Favorite</button>
</div>
```

The first part of the microsyntax is basically our for loop. We create a template instance variable named stock , which will be available in the HTML-Element that holds the directive or its children. This can be equated to a standard for-each loop, with the variable stock referring to each individual item in the array.

The second part after the semicolon is creating another template instance variable i , which holds the current index value

Similar to index, there are other local values that are available within the context of an NgFor directive that you can assign to a local variable name (like we did with i ) and bind them to different values:

- index: current index 
- even: true when the item has even index 
- odd: true when item has odd index 
- first: true when item is first in the array 
- last: true when item is last in array

You can then use these variables to bind to CSS classes, or display them in the UI, or run any other calculations you might want to.
for example to add a class to each even item 

```
[css.even-item]="isEven"
```

and then in the ngFor 

```
*ngFor="let stock of stocks; even as isEven; index as i"
```

Angular uses [object identity](http://www.perflensburg.se/Privatsida/cp-web/OBIDKHOS.HTM) to track additions, removals, and modifications to an array. That is, as long as the object reference does not change, Angular will not create new elements for it, and reuse the old element reference.
element creations and removals are pretty costly 

There are cases when the element reference might change, but you still want to continue using the same element. For example, when you fetch new data from the server, you don’t want to blow away your list and re-create it unless the data has fundamentally changed. This is where the trackBy capability of the NgFor directive comes into play.

The trackBy option takes a function that has two arguments: an index and the item. If the trackBy function is provided to the NgFor directive, then it will use the return value of the function to decide how to identify individual elements.


```
// in the component file 
trackStockByCode(index: number, stock: Stock){
    return stock.code; // same value means same element 
  }
```

```
<div class="stock-container" *ngFor="let stock of stocks; index as i; trackBy: trackStockByCode">
```

#### [ngSwitch](https://angular.io/api/common/NgSwitch)  

##### (ngFor and ngIf can't be used on same statement, just wrap one around the other)

#### [proper way to use multiple template bindings on an element](https://stackoverflow.com/questions/38585720/how-to-apply-multiple-template-bindings-on-one-element-in-angular)

### [Shadow DOM](https://developers.google.com/web/fundamentals/web-components/shadowdom)

#### Style Encapsulation 

we can tell angular what kind of style encapsulation we want it to do 

```
import { Component, ViewEncapsulation } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  encapsulation: ViewEncapsulation.None
})
export class AppComponent {
  title = 'stock-market';
}
```

the encapsulation attribute can take three values 
- ViewEncapsulation.Emulated : This the default, where Angular creates shimmed CSS to emulate the behavior that shadow DOMs and shadow roots provide.
- ViewEncapsulation.ShadowDom : This is the ideal, where Angular will use shadow roots. This will only work on browsers and platforms that natively support it.
- ViewEncapsulation.None : Uses global CSS, without any encapsulation.

#### [rest of options for component decorator](https://angular.io/api/core/Component)

### Components and modules 
There are three specific attributes on NgModule 

- declarations : The declarations attribute ensures that components and directives are available
to use within the scope of the module.
- imports: The imports attribute allows you to specify modules that you want imported and accessible within your module. This is mostly as a way to pull in third-party modules to make the components and services available within your application.
- exports: The exports attribute is relevant if you either have multiple modules or you need to create a library that will be used by other developers. Unless you export a component, it cannot be accessed or used outside of the direct module where the component is declared. 

### Input and Output 

Input and Output decorators apply at a class member variable levels 

#### Input 
When we add an Input decorator on a member variable, it automatically allows you to pass in values to the component for that particular input via Angular’s data binding syntax.

```
import { Component, OnInit, Input } from '@angular/core'; //importing the input decorator 
```

```
@Input() public stock!: Stock;
```

in the app component

```
import { Component, ViewEncapsulation, OnInit } from '@angular/core';
import { Stock } from './model/stock';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  encapsulation: ViewEncapsulation.Emulated
})
export class AppComponent implements OnInit {
  title = 'stock-market';

  public stockObj!: Stock;
  ngOnInit(): void{
    this.stockObj = new Stock('Test stock company', 'TSC', 35, 30);
  }
}
```

in the app template 

```
<div style="text-align:center">
  <h1>
    Welcome to {{title}}!
  </h1>
</div>
  <app-stock-item [stock]="stockObj" ></app-stock-item>
<router-outlet></router-outlet>
```

the variable name has to be the same as input (with the case also the same)
Angular has its own HTML parser under the covers that parses the templates for Angular-specific syntax, and does not rely on the DOM API for some of these. This is why Angular attributes are and can be case-sensitive.


#### Output 

Just like we can pass data into a component, we can also register and listen for events from a component. We use data binding to pass data in, and we use event binding syntax to register for events

We register an EventEmitter as an output from any component. We can then trigger the event using the EventEmitter object, which will allow any component bound to the event to get the notification and act accordingly

in stock component 

```
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { Stock } from '../../model/stock';

@Component({
  selector: 'app-stock-item',
  templateUrl: './stock-item.component.html',
  styleUrls: ['./stock-item.component.css']
})
export class StockItemComponent {
 
  @Input() public stock!: Stock;
  @Output() private toggleFavorite!: EventEmitter<Stock>;

  constructor() {
    this.toggleFavorite = new EventEmitter<Stock>();
   }

  onToggleFavorite(event: any){
    console.log("We are toggling the favorite state for this stock", event);
    this.toggleFavorite.emit(this.stock);
  }

}
```

now for the app component html 

```
<div style="text-align:center">
  <h1>
    Welcome to {{title}}!
  </h1>
</div>
  <app-stock-item [stock]="stockObj" (toggleFavorite)="toggly($event)" ></app-stock-item>
<router-outlet></router-outlet>
```

as you can see the event now can be listened to just like any other event for this component,   

also you see that we pass the `$event` in the parameter, not the `stock` this is because the eventemitter emits an event which **contains** the stock, so we use `$event` to get the arguments emitted by the EventEmitter   

now in app.component.ts 

```
 toggly(stock: Stock){
    console.log("favorite toggle triggered for \n" , stock);
    this.stockObj.favorite = !this.stockObj.favorite;
  }
  ```

  we can directly take the event as stock, looks like it doesn't put a wrapper around it 

### Change Detection 

changeDetection is an attribute on the Component decorator.

  By default, Angular applies the `ChangeDetectionStrategy.Default` mechanism to the `changeDetection` attribute. This means that every time Angular notices an event (say, a server response or a user interaction), it will go through each component in the component tree, and check each of the bindings individually to see if any of the values have changed and need to be updated in the view.

[Change Detection Example Github](https://github.com/Kstheking/Angular-Experiments)

you can  give a hint to the Angular change detector to check or not check certain components as you see fit.
For any given component, we can accomplish this by changing the ChangeDetectionStrategy from the
default to ChangeDetectionStrategy.OnPush .  
What this tells Angular is that the bindings for this particular component will need to be checked only based on the `Input` to this component.

The changedetection only runs now in below cases  
- The input reference changes [link](https://www.mokkapps.de/blog/the-last-guide-for-angular-change-detection-you-will-ever-need#input-reference-changes)
-  An event originated from component or one of its children (**the rule applies only to DOM events**) [link](https://www.mokkapps.de/blog/the-last-guide-for-angular-change-detection-you-will-ever-need#event-handler-is-triggered)
-   We run change detection explicitly 

#### Triggering change detection explicitly 

Angular provides us with three methods for triggering change detection ourselves when needed.

- The first is `detectChanges()` which tells Angular to run change detection on the component and his children. [ChangeDetectorRef](https://angular.io/api/core/ChangeDetectorRef) (take a variable of type ChangeDetectorRef in the constructor and call detectChanges on it whenever needed)
-  [ApplicationRef.tick()](https://angular.io/api/core/ApplicationRef#tick) which tells Angular to run change detection for the whole application. [Example](https://stackoverflow.com/questions/46796299/angular-how-to-use-app-tick-can-anyone-give-me-an-example-of-applicationre) (second answer)
-  The third is `markForCheck()` which does NOT trigger change detection. Instead, it marks all onPush ancestors as to be checked once, either as part of the current or next change detection cycle.  Use it similarly to detectChanges [link](https://angular.io/api/core/ChangeDetectorRef#markforcheck)

***

#### [Angular async pipe and other leftover stuff for change detection](https://netbasal.com/a-comprehensive-guide-to-angular-onpush-change-detection-strategy-5bac493074a4) 

### Component lifecycle 

[lifecycle hooks](https://angular.io/guide/lifecycle-hooks) are used to react to different stages of lifecycle of a component   
some of the hooks like (OnInit and AfterContentInit, basically anything that ends with Init) are called only once, while everyone else are called when content changes. The OnDestroy hook is called only once.    
Each of these lifecycle steps comes with an interface that should be implemented when a component cares about that particular lifecycle, and each interface provides a function starting with ng that needs to be implemented (OnInit -> ngOnInit)

### ViewChildren and ContentChildren 
ViewChildren is any component whose tags/selectors appear within the template of the component. 

ContentChildren is any child component that gets projected into the view of the component, but it is not directly included in the template of the component.   

| Interface           | Method                              | Applicable to             | Purpose                                                                                                                                                                                                               |
|---------------------|-------------------------------------|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| OnChanges           | ngOnChanges(changes: SimpleChange)  | Components and Directives | called right after constructor and after any change , called before the ngOnInit                                                                                                                                      |
| OnInit              | ngOnInit()                          | Components and Directives | allows to do one time initializations                                                                                                                                                                                 |
| DoCheck             | ngDoCheck()                         | Components and Directives | Angular's way to test if there are bindings or changes that angular should not detect on its own. one of the way to notify angular of a change in component,  when we override the default changeDetection  attribute |
| AfterContentInit    | ngAfterContentInit()                | Components only           | triggered during component projection cases and  only once during initialization of component                                                                                                                         |
| AfterContentChecked | ngAfterContentChecked()             | components only           | triggered each time change detection runs and in case of initialization runs after  AfterContentInit                                                                                                                  |
| AfterViewInit       | ngAfterViewInit()                   | components only           | triggered after all child components that are are directly used in component are finished  initializing and their view are updated with  bindings, triggered only once                                                |
| AfterViewChecked    | ngAfterViewChecked()                | components only           | triggered after each time all child components have been checked and updated. i.e. all children have finished their AfterViewCheck                                                                                    |
| OnDestroy           | ngOnDestroy()                       | components and directives | called when a component is about to be destroyed                                                                                                                                                                      |


### View projection 

Projection is an important idea in Angular as it gives us more flexibility when we develop our components
Projection is useful when we want to build components but set some parts of the UI of the component to not be an innate part of it   
[link](https://angular.io/guide/content-projection)  

basically you can just put an <ng-content></ng-content> in a component and when you call that component in some other component you can throw random stuff inside it like 

```
<div class="stock-container">
<div class="name">{{stock.name + ' (' + stock.code + ')'}}</div>
<div class="price"
[class]="stock.isPositiveChange() ? 'positive' : 'negative'">
$ {{stock.price}}
</div>
<ng-content></ng-content>
</div>
```

```
<h1>
{{title}}
</h1>
<app-stock-item [stock]="stockObj">
<button (click)="testMethod()">With Button 1</button>
</app-stock-item>
<app-stock-item [stock]="stockObj">
No buttons for you!!
</app-stock-item>
```

[ng-content article](https://medium.com/claritydesignsystem/ng-content-the-hidden-docs-96a29d70d11b)

### modules are cool
as you know every declaration in the module file is available through out the module so any component you make will be available in the module to all other components 

### [generating components with inline css and templates ](https://dev.to/sumitkharche/generate-component-with-inline-template-and-style-using-angular-cli-4ck2)

## testing 

### some frameworks 

#### jasmine 
Jasmine is a test framework that is oriented toward writing specifications rather than traditional unit tests.It is what is referred to as a behavior-driven development (BDD) framework.

#### karma 
If Jasmine is the test-writing framework, then Karma is the test-running framework. Karma’s sole task is to take any kind of test, and run it across a suite of real browsers and report the results back.

#### [angular testing utilities](https://angular.io/guide/testing#atu-apis)

#### Protractor 
Protractor is a framework that is built to write and run end-to-end tests (I have heard this is getting old might as well just use Puppeteer or codeceptjs with puppeteer) 

### files 

#### test.ts 
main entry point for our testing, and responsible for loading all our components, the related specifications, and the testing framework and utilities needed to run them:  

The test.ts file is basically responsible for loading a series of files for the testing framework, and then initializing the Angular testing environment. Then, it looks for specifications (files ending with .spec.ts) recursively in all the folders from the current directory (which is the src folder). It then loads all the relevant modules for them and starts executing Karma.

### time to write tests 

an example of app.component.spec.ts

```
import { AppComponent } from "./app.component";
import { Stock } from "./model/stock";

describe('AppComponent', () => { //our main test suite 

  it('should have stock instantiated on ngInit', () => { // the first test 
    const appComponent = new AppComponent(); //instantiating the app component 
    expect(appComponent.stockObj).toBeUndefined();
    appComponent.ngOnInit();
    expect(appComponent.stockObj).toEqual(new Stock('Test stock company', 'TSC', 35, 30));
  });

  it('should toggle stock favorite ', () => {
    const appComponent = new AppComponent();
    appComponent.ngOnInit();
    expect(appComponent.stockObj.favorite).toBeFalsy();
    appComponent.toggly(new Stock('Test', 'TST', 54, 55));
    expect(appComponent.stockObj.favorite).toBeTruthy();
    appComponent.toggly(new Stock('Test', 'TST', 54, 55));
    expect(appComponent.stockObj.favorite).toBeFalsy();
  });

})
```


##### Note that in an isolated unit test, Angular lifecycle methods are not called automatically, which is why we manually trigger ngOnInit ourselves in the test. This gives us the flexibility that we might want to test some other function and avoid the ngOnInit , which might make expensive or complex server calls.

#### to run tests 

```
ng test 
```

### writing angular aware tests 
we test that the template gets rendered with the correct bindings. Furthermore, we want to make sure the input and output bindings are connected and triggered correctly.

```
import { ComponentFixture, TestBed, waitForAsync } from '@angular/core/testing';

import { StockItemComponent } from './stock-item.component';
import { Stock } from 'src/app/model/stock';
import { By } from '@angular/platform-browser';

describe('Stock Item Component', () => {
    let fixture: ComponentFixture<StockItemComponent>, component: StockItemComponent;

    beforeEach(
        waitForAsync(() => {
            TestBed.configureTestingModule({ //configure a module for testing 
                declarations: [
                    StockItemComponent
                ]
            }).compileComponents(); //compiling all declared components 
        })
    );

    beforeEach(() => {
        fixture = TestBed.createComponent(StockItemComponent); //creating an instance of component fixture under test 
        component = fixture.componentInstance; //getting the underlying component instance 
        component.stock = new Stock('Testing Stock', 'TS', 100, 200);
        fixture.detectChanges(); //trigger change detection 
    })

    it('should create stock component and render stock data', () => {
        const nameEl = fixture.debugElement.query(By.css('.name'));
        expect(nameEl.nativeElement.textContent).toEqual('Testing Stock (TS)');
        const priceEl = fixture.debugElement.query(By.css('.price.negative'));
        expect(priceEl.nativeElement.textContent).toEqual('$ 100');
        const addToFavoriteBtnEl = fixture.debugElement.query(By.css('button'));
        expect(addToFavoriteBtnEl).toBeDefined();
    });

    it('should trigger event emitter on add to favorite', () => {
        let selectedStock!: Stock;
        component.toggleFavorite.subscribe((stock: Stock) => {
            selectedStock = stock;
        });
        const addToFavoriteBtnEl = fixture.debugElement.query(By.css('button'));
        expect(selectedStock).toBeUndefined();
        addToFavoriteBtnEl.triggerEventHandler('click',null);
        expect(selectedStock).toEqual(component.stock);
    });
});
```

- the waitForAsync ensures that we don't start testing until the async functions are complete 
-  compile component loads the components, loads all its related templates and styles, and then creates a compiled component for us to use later. this can happen asynchronously 
-  fixture is an instance of the component along with its template and everything related to it.

### debugging 
- click "debug" on karma runner 
-  on the newly opened page, go to Ctrl+Shift+I (for firefox developer tools) go to debugger and select any source you wanna edit (.ts files .js files whatever), click on left of any line to put the breakpoint there
-  refresh the browser to run tests again 

### Why use waitForAsync not simply mark the function as async and use await statements inside it ?
You can totally do the latter part, no problem, the await statement will pause the execution of the async function until the promise is resolved and then the function continues   
but in cases when you don't know if something is async or not, when to use await or not, you can just wrap the entire thing in waitForAsync which will wait for anything asynchronous and resume once its all done 

## template driven forms 
Template-driven forms, as the name suggests, start with the template, and use data binding to get the data to and from your components. It is template-first, and allows you to drive the logic of your application via your template.

## ngModel  
The ngModel directive and its special syntax abstracts away the internals of each and every input type from us developers, making it easier for us to quickly develop form based applications.  
The ng-model directive binds the value of HTML controls (input, select, textarea) to application data.

- in ngModel it is necessary to put the name field 

```
<h2>create stock form</h2>

<div class="form-group">
    <form >
        <div class="stock-name">
            <input type="text" placeholder="Stock Name" name="stockName" [ngModel]="stock.name" (ngModelChange)="stock.name=$event">
        </div>
    </form>
    <button (click)="stock.name='test'">Reset stock name</button>
</div>

<h4>Stock name is {{stock.name}}</h4>
```

if u want it shorter, we can use banana in a box syntax 

```
[(ngModel)]="stock.name"
```

another example 

```
<h2>create stock form</h2>

<div class="form-group">
    <form (ngSubmit)="createStock()">
        <div class="stock-name">
            <input type="text" placeholder="Stock Name" name="stockName" [ngModel]="stock.name" (ngModelChange)="stock.name=$event">
        </div>
        <div class="stock-code">
            <input type="text" name="stockCode" placeholder="Stock code" [(ngModel)]="stock.code">
        </div>
        <div class="stock-price">
            <input type="number" name="stockPrice" placeholder="Stock price" [ngModel]="stock.price" (ngModelChange)="setStockPrice($event)">
        </div>
        <div class="stock-exchange">
            <div>
                <input type="radio" name="stockExchange" [(ngModel)]="stock.exchange" value="NYSE"> NYSE
            </div>
            <div>
                <input type="radio" name="stockExchange" [(ngModel)]="stock.exchange" value="NASDAQ"> NASDAQ
            </div>
            <div>
                <input type="radio" name="stockExchange" [(ngModel)]="stock.exchange" value="OTHER"> OTHER
            </div>
        </div>
        <div class="stock-confirm">
            <input type="checkbox" name="stockConfirm" [(ngModel)]="confirmed">
            I swear to god that the info above is correct !! 
        </div>
        <button [disabled]="!confirmed" type="submit">Create</button>
    </form>
    <button (click)="stock.name='test'">Reset stock name</button>
</div>

<h4>Stock name is {{stock.name}}</h4>
```

- Each radio button has the same name (which is the standard HTML way of creating a radio group)

you can also use a select   

```
<select name="stockExchange" [(ngModel)]="stock.exchange">
<option value="NYSE">NYSE</option>
<option value="NASDAQ">NASDAQ</option>
<option value="OTHER">OTHER</option>
</select>
```

### control states
In Angular, form controls are classes that can hold both the data values and the validation information of any form element.  
there are two aspects to this : 
- The state, which allows us to peek into the state of the form control, on whether the user has visited it, whether the user has changed it, and finally whether it is in a valid state.
- The validity, which tells us whether a form control is valid or not, and if it is not valid, the underlying reason (or reasons) for which the form element is invalid.

The ngModel directive changes and adds CSS classes to the element it is on, based on the user’s interaction with it. There are three primary modes of interaction that it tracks, and two CSS classes per mode of interation associated with it.    

| Control state | CSS class on true | CSS class if false |
| --- | --- | --- |
| Visited | ng-touched | ng-untouched |
| Changed | ng-dirty | ng-pristine |
| Valid | ng-valid | ng-invalid | 

ng-touched triggers when user interacts with that particular field and leaves it (not while he is interacting)   

### [Template reference variables](https://angular.io/guide/template-reference-variables)
A template reference variable in Angular allows us to get a temporary handle on a DOM element, component, or directive directly in the template. example:

```
<input type="text" #myStockField name="stockName">
```

The #myStockField is a template reference variable that gives us a reference to the input form field. We can then use this as a variable in any Angular expression, or directly access its value through myStockField.value  

By default, when we don’t pass it any value, it will always refer to the HTML DOM element.   

If the variable specifies a name on the right-hand side, such as #var="ngModel", the variable refers to the directive or component on the element with a matching exportAs name.  

```
<form #itemForm="ngForm" (ngSubmit)="onSubmit(itemForm)">
  <label for="name">Name <input class="form-control" name="name" ngModel required />
  </label>
  <button type="submit">Submit</button>
</form>

<div [hidden]="!itemForm.form.valid">
  <p>{{ submitMessage }}</p>
</div>
```

Without the ngForm attribute value, the reference value of itemForm would be the HTMLFormElement  

With NgForm, itemForm is a reference to the NgForm directive with the ability to track the value and validity of every control in the form.Unlike the native <form> element, the NgForm directive has a form property.

### Form groups 

here is another method of working with template-driven forms and the ngModel directive. So far, we have been
declaring a member variable in our component, and then using ngModel to bind to it. We can instead let the form model drive the entire form, and copy over or use the values from it once the form has been submitted.

```
<div class="form-group">
    <form (ngSubmit)="createStock(stockForm)" #stockForm="ngForm">
        <div ngModelGroup="stock">
            <div class="stock-name">
                <input type="text" placeholder="Stock Name" required name="stockName" ngModel>
            </div>
            <div *ngIf="stockName.errors && stockName.errors.required"> Stock name is mandatory</div>
...
```

- We have removed the banana-in-a-box syntax from all the ngModel bindings, and just kept it as an attribute. When we use ngModel like this, Angular uses the namefield on the form element as the model name and creates a model object corresponding to it on the form.
- We have surrounded the form fields with another div, and used an Angular directive called ngModelGroup on it, providing it a name (stock in this case).What this does is group the form elements, thus creating the name, price, code,and exchange fields as models under the common name stock. This is visible in the component when we access this entire set of values through form.value.stock.

## Reactive forms 

Reactive forms provide direct, explicit access to the underlying forms object model. Compared to template-driven forms, they are more robust: they're more scalable, reusable, and testable.  

Unlike template-driven forms in Angular, with reactive forms, you define the entire tree of Angular form control objects in your component code, and then bind them to native form control elements in your template

### Form controls 

The core of any reactive form is the FormControl, which directly represents an individual form element in the template.

```
<h2>create stock form </h2>

<div class="form-group">
    <div class="stock-name">
        <input type="text" placeholder="Stock name here bish" name="stockName" [formControl]="nameControl">
    </div>
    <button (click)="onSubmit()">Submit</button>
</div>

<p>Form control value : {{ nameControl.value | json }}</p>
<p>Form control value : {{nameControl.status | json }} </p>
```

in component 

```
 public nameControl = new FormControl();
  constructor() {
   }

  ngOnInit(): void {
  }

  onSubmit(){
    console.log(this.nameControl.value);
  }
```

We have used the default FormControl constructor, but it can also take the initial value along with a list of validators (both sync and async) as arguments

### Form groups 

the FormGroup is useful as a way to group relevant form fields under one group

```
<h2>Create Stock Form</h2>
<div class="form-group">
    <form [formGroup]="stockForm" (ngSubmit)="onSubmit()">
        <div class="stock-name">
            <input type="text" placeholder="Stock Name" name="stockName" formControlName="name">
        </div>
        <div class="stock-code">
            <input type="text" placeholder="Stock Code" formControlName="code">
        </div>
        <div class="stock-price">
            <input type="number" placeholder="Stock Price" formControlName="price">
        </div>
        <button type="submit">Submit</button>
    </form>
</div>
<p>Form Control value: {{ stockForm.value | json }}</p>
<p>Form Control status: {{ stockForm.status | json }}</p>
```

```
import { Component, OnInit } from '@angular/core';
import { FormControl, FormGroup, NgForm, Validators } from '@angular/forms';
import { Stock } from 'src/app/model/stock';

@Component({
  selector: 'app-create-stock',
  templateUrl: './create-stock.component.html',
  styleUrls: ['./create-stock.component.css']
})
export class CreateStockComponent implements OnInit {
  public stockForm: FormGroup = new FormGroup({
    name: new FormControl(null, Validators.required),
    code: new FormControl(null, [Validators.required, Validators.minLength(2)]),
    price: new FormControl(0, [Validators.required, Validators.min(0)])
  })
  constructor() {
   }

  ngOnInit(): void {
  }

  onSubmit(){
    console.log(this.stockForm.value);
  }
  
}
```


###  Form Builders 
while the FormGroup gives us the flexibility to build complex, nested forms, its syntax is slightly verbose. And that’s why, to replace it, we have a FormBuilder in Angular, which makes it slightly nicer to build these rich forms in a cleaner manner.  

```
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormControl, FormGroup, NgForm, Validators } from '@angular/forms';
import { Stock } from 'src/app/model/stock';

@Component({
  selector: 'app-create-stock',
  templateUrl: './create-stock.component.html',
  styleUrls: ['./create-stock.component.css']
})
export class CreateStockComponent implements OnInit {
  public stockForm!: FormGroup;
  constructor(private fb: FormBuilder) {
    this.createForm();
   }

   createForm(){
     this.stockForm = this.fb.group({
       name: [null, Validators.required],
       code: [null, [Validators.required, Validators.minLength(2)]],
       price: [0, [Validators.required, Validators.min(0)]]
     })
   }

  ngOnInit(): void {
  }

  onSubmit(){
    console.log(this.stockForm.value);
  }
  
}
```

### control state validity and error messages 

```
<h2>Create Stock Form</h2>
<div class="form-group">
  <form [formGroup]="stockForm" (ngSubmit)="onSubmit()">
    <div class="stock-name">
      <input
        type="text"
        placeholder="Stock Name"
        name="stockName"
        formControlName="name"
      />
      <div *ngIf="name!.invalid && (name!.dirty || name!.touched)">
        Name is required
      </div>
    </div>
    <div class="stock-code">
      <input type="text" placeholder="Stock Code" formControlName="code" />
      <div
        *ngIf="
          stockForm.get('code')?.invalid &&
          (stockForm.get('code')?.dirty || stockForm.get('code')?.touched)
        "
      >
        <div *ngIf="stockForm.get('code')?.errors?.required">
          Stock Code is required
        </div>
        <div *ngIf="stockForm.get('code')?.errors?.minlength">
          Stock Code must be at least 2 characters
        </div>
      </div>
    </div>
    <div class="stock-price">
      <input type="number" placeholder="Stock Price" formControlName="price" />
      <div
        *ngIf="
          stockForm.get('price')?.invalid &&
          (stockForm.get('price')?.dirty || stockForm.get('price')?.touched)
        "
      >
        <div *ngIf="stockForm.get('price')?.errors?.required">
          Stock Price is required
        </div>
        <div *ngIf="stockForm.get('price')?.errors?.min">
          Stock Price must be positive
        </div>
      </div>
    </div>
    <button type="submit">Submit</button>
  </form>
</div>
<p>Form Control value: {{ stockForm.value | json }}</p>
<p>Form Control status: {{ stockForm.status | json }}</p>
```

### Form and Data model 

```
import { Component, OnInit } from '@angular/core';
import { FormArray, FormBuilder, FormControl, FormGroup, NgForm, Validators } from '@angular/forms';
import { Stock } from 'src/app/model/stock';

let counter = 1;

@Component({
  selector: 'app-create-stock',
  templateUrl: './create-stock.component.html',
  styleUrls: ['./create-stock.component.css']
})



export class CreateStockComponent implements OnInit {
  private stock!: Stock;
  public stockForm!: FormGroup;
  constructor(private fb: FormBuilder) {
    this.createForm();
    this.stock = new Stock('Test ' + counter++, 'TST', 20, 10, 'NASDAQ');
  }

  createForm() {
    this.stockForm = this.fb.group({
      name: [null, Validators.required],
      code: [null, [Validators.required, Validators.minLength(2)]],
      price: [0, [Validators.required, Validators.min(0)]],
      notablePeople: this.fb.array([])
    })
  }

  get notablePeople(){
    return this.stockForm.get('notablePeople') as FormArray;
  }

  addNotablePerson(){
    this.notablePeople.push(this.fb.group({
      name: ['', Validators.required],
      title: ['', Validators.required]
    }))
  }

  removeNotablePerson(index: number){
    this.notablePeople.removeAt(index);
  }

  loadStockFromServer() {
    this.stock = new Stock('Test ' + counter++, 'TST', 20, 10, 'BITCH');
    let stockFormModel = Object.assign({}, this.stock);
    delete stockFormModel.previousPrice;
    delete stockFormModel.favorite;
    delete stockFormModel.exchange;
    this.stockForm.setValue(stockFormModel);
  }

  patchStockForm() {
    this.stock = new Stock(`Test ${counter++}`, 'TST', 20, 10);
    this.stockForm.patchValue(this.stock);
  }

  resetForm() {
    this.stockForm.reset();
  }

  ngOnInit(): void {
  }

  onSubmit() {
    this.stock = Object.assign({}, this.stockForm.value);
    console.log('Saving stock', this.stock);
  }

  get name() {
    return this.stockForm.get('name');
  }

}
```

### Form arrays 
A FormArray aggregates the values of each child FormControl into an array. It calculates its status by reducing the status values of its children. For example, if one of the controls in a FormArray is invalid, the entire array becomes invalid.

```
import { Component, OnInit } from '@angular/core';
import { FormArray, FormBuilder, FormControl, FormGroup, NgForm, Validators } from '@angular/forms';
import { Stock } from 'src/app/model/stock';

let counter = 1;

@Component({
  selector: 'app-create-stock',
  templateUrl: './create-stock.component.html',
  styleUrls: ['./create-stock.component.css']
})



export class CreateStockComponent implements OnInit {
  public stock!: Stock;
  public stockForm!: FormGroup;
  constructor(private fb: FormBuilder) {
    this.createForm();
    this.stock = new Stock('Test ' + counter++, 'TST', 20, 10, 'NASDAQ');
  }

  createForm() {
    this.stockForm = this.fb.group({
      name: [null, Validators.required],
      code: [null, [Validators.required, Validators.minLength(2)]],
      price: [0, [Validators.required, Validators.min(0)]],
      notablePeople: this.fb.array([])
    })
  }

  get notablePeople(){
    return this.stockForm.get('notablePeople') as FormArray;
  }

  addNotablePerson(){
    this.notablePeople.push(this.fb.group({
      name: ['', Validators.required],
      title: ['', Validators.required]
    }))
  }

  removeNotablePerson(index: number){
    this.notablePeople.removeAt(index);
  }

  loadStockFromServer() {
    this.stock = new Stock('Test ' + counter++, 'TST', 20, 10, 'BITCH');
    let stockFormModel = Object.assign({}, this.stock);
    delete stockFormModel.previousPrice;
    delete stockFormModel.favorite;
    delete stockFormModel.exchange;
    this.stockForm.setValue(stockFormModel);
  }

  patchStockForm() {
    this.stock = new Stock(`Test ${counter++}`, 'TST', 20, 10);
    this.stockForm.patchValue(this.stock);
  }

  resetForm() {
    this.stockForm.reset();
  }

  ngOnInit(): void {
  }

  onSubmit() {
    this.stock = Object.assign({}, this.stockForm.value);
    console.log('Saving stock', this.stock);
  }

  get name() {
    return this.stockForm.get('name');
  }

}
```

```
<h2>Create Stock Form</h2>
<div class="form-group">
  <form [formGroup]="stockForm" (ngSubmit)="onSubmit()">
    <div class="stock-name">
      <input
        type="text"
        placeholder="Stock Name"
        name="stockName"
        formControlName="name"
      />
      <div *ngIf="name!.invalid && (name!.dirty || name!.touched)">
        Name is required
      </div>
    </div>
    <div class="stock-code">
      <input type="text" placeholder="Stock Code" formControlName="code" />
      <div
        *ngIf="
          stockForm.get('code')?.invalid &&
          (stockForm.get('code')?.dirty || stockForm.get('code')?.touched)
        "
      >
        <div *ngIf="stockForm.get('code')?.errors?.required">
          Stock Code is required
        </div>
        <div *ngIf="stockForm.get('code')?.errors?.minlength">
          Stock Code must be at least 2 characters
        </div>
      </div>
    </div>
    <div class="stock-price">
      <input type="number" placeholder="Stock Price" formControlName="price" />
      <div
        *ngIf="
          stockForm.get('price')?.invalid &&
          (stockForm.get('price')?.dirty || stockForm.get('price')?.touched)
        "
      >
        <div *ngIf="stockForm.get('price')?.errors?.required">
          Stock Price is required
        </div>
        <div *ngIf="stockForm.get('price')?.errors?.min">
          Stock Price must be positive
        </div>
      </div>
    </div>
    <button type="submit">Submit</button>
    <button type="button" (click)="resetForm()">Reset</button>
    <button type="button" (click)="loadStockFromServer()">
      Simulate Stock Load from Server
    </button>
    <button type="button" (click)="patchStockForm()">Patch Stock Form</button>

    <div formArrayName="notablePeople">
      <div *ngFor="let person of notablePeople.controls; let i = index" [formGroupName]="i" class="notable-people">
        <div>
          Person {{i + 1}}
        </div>
        <div>
          <input type="text" placeholder="Person name" formControlName="name">
        </div>
        <div>
          <input type="text" placeholder="Person title" formControlName="title">
        </div>
        <button type="button" (click)="removeNotablePerson(i)">Remove Person</button>
      </div>
    </div>
    <button type="button" (click)="addNotablePerson()"> Add notable person</button>
    <button type="submit"> Submit </button>
    <button type="button" (click)="resetForm()"> Reset</button>
  </form>
</div>
<p>Form Control value: {{ stockForm.value | json }}</p>
<p>Form Control status: {{ stockForm.status | json }}</p>
<p>stock value : {{stock | json}} </p>
```


#### Note: not using [] syntax while binding in angular, leads to evaluation of rhs as a string literal only 

## Services 

Service is a broad category encompassing any value, function, or feature that an application needs. A service is typically a class with a narrow, well-defined purpose. It should do something specific and do it well.
A component can delegate certain tasks to services, such as fetching data from the server, validating user input, or logging directly to the console.

### generating a service 

`ng g service services/stock`

The @Injectable() decorator specifies that Angular can use this class in the DI(Dependency Injection) system. The metadata, providedIn: 'root', means that the service is visible throughout the application.  

The providers array in the Angular module tells Angular to create a singleton instance of the service, and make it available for any class or component that asks for it.  

When we register it at the module level, it means that any component within the module that asks for the service will get the exact same instance injected into it.  

to register it at app module level using cli `ng g service services/stock --module=app`

```
import { Component, OnInit } from '@angular/core';
import { Stock } from 'src/app/model/stock';
import { StockService } from 'src/app/services/stock.service';


@Component({
  selector: 'app-stock-list',
  templateUrl: './stock-list.component.html',
  styleUrls: ['./stock-list.component.css']
})
export class StockListComponent implements OnInit {
  public stocks!: Stock[];
  constructor(private stockService: StockService) { }

  ngOnInit(): void {
    this.stocks = this.stockService.getStocks();
  }

  onToggleFavorite(stock: Stock) {
    console.log('Favorite for stock ', stock, ' was triggered');
    this.stockService.toggleFavorite(stock);
  }

}
```

The name itself doesn’t matter; Angular uses the type definition to figure out what service to inject.  

### Dependency injection 
dependency injection is the idea that any class or function should ask for its dependencies, rather than instantiating it themselves.  
Something else (usually called an injector) would be responsible for figuring out what is needed and how to instantiate it.  
Every service that we create needs to be registered as a provider with an injector. Then any other class can ask for the service and the injector will be responsible for providing it.  
At its root, at the application level, Angular has the root module and the root injector.  

now for create-stock-component 

```
@Component({
  selector: 'app-create-stock',
  templateUrl: './create-stock.component.html',
  styleUrls: ['./create-stock.component.css'],
  providers: [MessageService]
})
```

we used component level injector and now the messageService instance will be separate from the root level instance of MessageService   
Angular will create a chain of injectors all the way down, depending on the need and declarations. And all
child components will inherit that injector, which will take precedence over the root injector.

### RxJs and observables 
Observables are a ReactiveX concept that allows us to deal with streams which emit data. Any interested party can then be an observer on this stream, and perform operations and transformations on the events emitted by the stream.  

- Promises operate on a single asynchronous event, while observables allow us to deal with a stream of zero or more asynchronous events.
- Unlike promises, observables can be canceled. That is, a promise’s success or error handler will eventually be called, while we can cancel a subscription and not process data if we don’t care about it.
  - Observables allow us to compose and create a chain of transformations easily. The operators it provides out of the box allow for some strong and powerful compositions, and operations like retry and replay make handling some common use cases trivial

```
import { Observable } from 'rxjs';
import { ErrorObservable } from 'rxjs/observable/ErrorObservable';
import { of as ObservableOf } from 'rxjs/observable/of';
...
getStocks() : Observable<Stock[]> {
    return ObservableOf(this.stocks) ;
  }
...
if (foundStock) {
      return ErrorObservable.create({
        msg: 'Stock with code ' + stock.code + " already exists"
      })
    }
```

in stock list 

```
ngOnInit() {
    this.stockService.getStocks().subscribe(
      stocks => {
        this.stocks = stocks;
      }
    )
  }
```

create stock component 

```
if (stockForm.valid) {
      this.stockService.createStock(this.stock).subscribe(
        (result : any) => {
          this.messageService.message = result.msg;
          this.stock = new Stock('','',0,0,'NASDAQ');
        }, (err) => {
          this.messageService.message = err.msg;
        }
      );
      
    }
```

the subscribe method allows us to take two functions as arguments. The first argument is called in the case of a successful call, while the second is the error handler callback.  

one shortcut you can use is to async pipe and have the observable itself as a member variable (this can be used in case you want to directly display the data thrown by an observable)   
(below stocks is an Observable<Stock[]> type )

```
<app-stock-item *ngFor="let stock of stocks | async" [stock]="stock"
                (toggleFavorite)="onToggleFavorite($event)">
</app-stock-item>
```

## making http calls in angular

in app module  
`import { HttpClientModule } from '@angular/common/http'`  

then use HttpClient service   

```
import { HttpClient } from '@angular/common/http';
...
constructor(private http: HttpClient) {
...

  getStocks() : Observable<Stock[]> {
    return this.http.get<Stock[]>('/api/stock');
    
  }

  createStock(stock: Stock): Observable<any> {
    return this.http.post('api/stock', stock);
    
  }

  toggleFavorite(stock: Stock): Observable<Stock> {
    return this.http.patch<Stock>('/api/stock/' + stock.code, 
    {
      favorite: !stock.favorite
    })
  }
```  
 
we also need a proxy.conf.json   

```
{
    "/api": {
        "target": "http://localhost:3000",
        "secure": false
    }
}
```  

you'll also need a different command   
`ng serve --proxy-config proxy.conf.json`

### advanced http 

#### note: 
When we type the data returned from an api using HttpClient we use interfaces because [here](https://stackoverflow.com/questions/22875636/how-do-i-cast-a-json-object-to-a-typescript-class) you can use [custom stuff](https://github.com/typestack/class-transformer) but no simple casting available in angular   
so you can just use interfaces instead of class   

#### Options - Headers - params
so let's try passing options alongside our url in a get request 

```
  getStocks() : Observable<Stock[]> {
    return this.http.get<Stock[]>('/api/stock', {
      headers: new HttpHeaders()
      .set('Authorization', 'MyAuthorizationHeaderValue')
      .set('X-EXAMPLE-HEADER', 'TestValue'),
      params: {
        q: 'test',
        test: 'value'
      }
    });
  }
```

#### options - observe/response type 
the observe parameter takes one of three values   
- body: default value, ensures that the observable’s subscribe gets called with the body of the response. This body is auto-casted to the return type specified when calling the API.
- response : This changes the response type of the HTTP APIs to returning the entire HttpResponse instead of just the body.  
Note that instead of a raw HttpResponse, what you really get is an HttpResponse<T>, where T is the type of the body. So in the case of getStocks, what you would end up returning is actually an observable of HttpResponse<Stock[]>.
- events: This is similar to response, but gets triggered on all HttpEvents.  It works at a much lower level. It exposes events that capture progress of both the request and response.This would include the initialization event, as well as the request finished event.  returns HttpEvents of generic type object (HttpEvents<object>).

the responseType parameter takes one of four values   
- json: default value, ensures response body is parsed as a json object 
- text: response body as a string with no parsing. in this case response would be an Observable <string> instead of a typed response 
- blob: used to deal with binary responses. blob gives you a file-like object containing the raw immutable data, which you can then process as you see fit.
- arraybuffer: The last option gives us the underlying raw binary data directly.   


#### [Interceptors](https://angular.io/api/common/http/HttpInterceptor) 
One other common use case is to be able to hook into all incoming and outgoing requests to be able to either modify them in flight, or listen in on all responses to accomplish things like logging, handling authentication failures in a common manner, etc.  
With the HttpClient API, Angular allows easily defining interceptors, which can conceptually sit between the HttpClient and the server, allowing it to transform all outgoing requests, and listen in and transform if necessary all incoming responses before passing them on.    

##### [a nice blog on it](https://ultimatecourses.com/blog/intro-to-angular-http-interceptors)  

- we need to implement HttpInterceptor interface and then implement the method intercept to make an interceptor 
- HttpInterceptor is a chain. Each interceptor is called with the request, and it is up to the interceptor to decide whether it continues in the chain or not. Within this context, each interceptor can decide to modify the request as well. It can continue the chain by calling the handler provided to the intercept method with a request object.  
- If there is only one interceptor, then the handler would simply call our backend with the request object. If there are more, it would proceed to the next interceptor in the chain.
- Once our Interceptor is created, we need to register it as a [multi-provider](https://stackoverflow.com/questions/38144641/what-is-multi-provider-in-angular2) since there can be multiple interceptors running within an application. Important note, you must register the provider to the app.module for it to properly apply to all application HTTP requests. Interceptors will only intercept requests that are made using the HttpClient service.

```
providers: [
    StockService,
    AuthService,
    {
      provide: HTTP_INTERCEPTORS,
      useClass: StockAppInterceptor,
      multi: true
    }
  ],
```  

#### [rxjs operators](https://rxjs-dev.firebaseapp.com/guide/operators)  

```
import { Observable } from "rxjs";
import 'rxjs/add/operator/do';
...
intercept(req: HttpRequest<any>, next: HttpHandler): Observable <HttpEvent<any>> {
        if(this.authService.authToken){
            const authReq = req.clone({
                headers: req.headers.set(
                    'Authorization',
                    this.authService.authToken
                )
            });
            console.log("making an authorized request");
            req = authReq;
        }
        return next.handle(req)
        .do(event => this.handleResponse(req, event),
        error => this.handleError(req, error));
    }

    handleResponse(req: HttpRequest<any>, event){
        console.log("Handling response for ", req.url, event);
        if( event instanceof HttpResponse){
            console.log("request for ", req.url, " response status ", event.status, " with body ", event.body);
        }
    }

    handleError(req: HttpRequest<any>, event){
        console.error('Request for ', req.url, ' Response status ', event.status, ' with error ', event.error);
    }
```

- [do operator](https://stackoverflow.com/questions/40957381/use-case-of-observable-do-operator-rxjs) on an observable allows us to hook onto the results of an observable in a pass-through manner, but still make changes or have side effects. It is useful for cases like these where we want to see the response and possibly even make changes to it or log it.

#### why clone the request ?   
HttpRequest (and HttpResponse) instances are immutable, to make sure they are distinct if same request is repeated or a different one is made.   

#### note: 
HttpClient underneath requires all the interceptors, and when we add a service as a dependency to an inceptor that requires HttpClient, we end up with a circular ask that breaks our application. be careful

### Advanced Observables 

- angular observables are **cold** by default which means every time someone subscribes to it the observable is triggered.   

```
<h2>
We have found {{(stocks$ | async)?.length}} stocks!
</h2>
<app-stock-item *ngFor="let stock of stocks$ | async"
[stock]="stock">
</app-stock-item>
```

- In this case, we have two subscribers, which are the two Async pipes, one for the ngFor and one for the length. Thus, instead of using the same observable, we end up with two different calls being made.  
- if an observable is something which connects a producer to a consumer, then a cold observable is responsible for creating the producer as well while a hot observable shares the producer [hot and cold obsrvables](https://benlesh.medium.com/hot-vs-cold-observables-f8094ed53339)  

so we can either kinda use hot observable concept or just not use two subscribers or use promises  or you could pipe the observable using share() which multicasts it and ensures that no matter the subscribers only one underlying trigger to server is done   

```
import { share } from 'rxjs/operators';
...
this.stocks$ = this.stockService.getStocks().pipe(share());
```

ok so let's make a searchbox and try to make it efficient so that it doesn't do duplicate calls   
[subject](https://rxjs.dev/guide/subject)  

```
  public searchString: string = '';
  private searchTerms: Subject<string> = new Subject();
  ...
ngOnInit() {
    this.stocks$ = this.searchTerms.pipe(
      startWith(this.searchString),
      debounceTime(500),
      distinctUntilChanged(),
      switchMap((query) => 
        this.stockService.getStocks(query)
      ),
      share()
    )
  }

  search(){
    this.searchTerms.next(this.searchString);
  }
```

- A Subject is a special type in RxJS that acts as both an observer as well as an observable. That is, it is capable of both emitting events as well as subscribing to one. we can see in ngOnInit we are using it as an observable and then using it as an observer to which we are feeding searchString in search function   
- [startWith](https://www.learnrxjs.io/learn-rxjs/operators/combination/startwith) emit given value first, it is used for the first render so that when we load the page it doesn't show nothing. We start it with an empty string, which ensures that when the page is loaded, we see our original list of stocks.
- [debounceTime](https://indepth.dev/reference/rxjs/operators/debounce-time) make stuff go slow, debounceTime delays the values emitted by a source for the given due time. If within this time a new value arrives, the previous pending value is dropped and the timer is **reset**. In this way debounceTime keeps track of most recent value and emits that most recent value when the given due time is passed. This ensures that we only send a call once the user stops typing for half a second.
- [distinctUntilChanged](https://indepth.dev/reference/rxjs/operators/distinct-until-changed) only emits event when the new value is different from previous value 
- map is used to switch type of observables such as here we are switching from a string observable to `Stock[]`. we use [switchMap](https://www.learnrxjs.io/learn-rxjs/operators/transformation/switchmap) over map here because it can cancel in-flight subscriptions, it does not necessarily cancels the underlying http request but simply drops the subscription [official doc for switchMap](https://rxjs.dev/api/operators/switchMap)


#### note: [don't use function calls in angular templates](https://medium.com/showpad-engineering/why-you-should-never-use-function-calls-in-angular-template-expressions-e1a50f9c0496)

#### [how do i access another component's function](https://stackoverflow.com/questions/37587732/how-to-call-another-components-function-in-angular2)

#### [ngOnChanges vs ngDoCheck](https://stackoverflow.com/questions/38629828/what-is-the-difference-between-onchanges-and-docheck-in-angular-2)

- for EventEmitter next and emit both work although next is deprecated 
- if a component emits an event then in parent template that component is where you put `(eventName)="whatever()"` you can't put event anywhere you want 
- you can also merge observables so if you have some long chain where ( a -> b -> c -> d) and you want to use the c -> d chain for some subject so you just merge in between, this is when let's say for a particular case you don't want to go through a and b and the end result is same type observable 

## Unit testing services 

- Configure the Angular TestBed with a provider for the service we want to test.
- Inject an instance of the service we want to test either into our test or as a common instance in the beforeEach.

### initial setup 

```
import { TestBed, inject } from '@angular/core/testing';

import { StockService } from './stock.service';

describe('StockService', () => {
  beforeEach(() => {
    TestBed.configureTestingModule({
      providers: [StockService]
    });
  });

  it('should be created', inject([StockService], (service: StockService) => {
    expect(service).toBeTruthy();
  }));
});
```

- in the beforeEach we configure the testing module with the provider 
- in the `it` we are calling the inject, we pass an array as the first argument which tells the services that need to be injected , and the second argument is a function which gets arguments in the same order that we passed to the array 


```
beforeEach(inject([StockService], (service: StockService) => {
    stockService = service;
  }))
```

note that this does not mean the same instance of StockService is going to be used in all tests but only that it gets intialized every time before a test   

so you can just provide the service in the testing module and test the component as you did previously no big deal  

### testing components with a mock service   
Let’s see how we could use the real service with just some calls mocked out in our test.

```
beforeEach(async(() => {
        TestBed.configureTestingModule({
            declarations: [StockListComponent, StockItemComponent],
            providers: [StockService]
        })
            .compileComponents();
    }));
beforeEach(() => {
        fixture = TestBed.createComponent(StockListComponent);
        component = fixture.componentInstance;
        stockService = fixture.debugElement.injector.get(StockService);
        let spy = spyOn(stockService, 'getStocks')
        .and.returnValue([
            new Stock('mock stock', 'MS', 800, 900, 'NYSE')
        ])
        fixture.detectChanges();
    });
    it('should load stocks from real service on init', () => {
        expect(component).toBeTruthy();
        expect(component.stocks.length).toEqual(3);
        expect(component.stocks[0].code).toEqual('MS');
    });
```

- we use injector of the component in the test fixture to get a handle on the StockService 
- we are using different jasmine spies to spy on different methods on the service 
- A [spy](https://jasmine.github.io/api/edge/Spy.html) (whether it is from Jasmine or any other framework) allows us to stub any function or method, and track any calls to it along with its arguments and also define our own return values.

## testing components with a fake service 

[ a cool question on differences between fake mock and stubs](https://stackoverflow.com/questions/346372/whats-the-difference-between-faking-mocking-and-stubbing)  

```
beforeEach(async(() => {
        let stockServiceFake = {
            getStocks: () => {
                return [new Stock('Fake stock', 'FS', 800, 900, 'NYSE')];
            }
        }
        TestBed.configureTestingModule({
            declarations: [StockListComponent, StockItemComponent],
            providers: [
                {
                    provide: StockService,
                    useValue: stockServiceFake
                }
            ]
        })
            .compileComponents();
    }));
```

- you see we just made a service and changed the providers to reflect it accordingly 
- now possibly the inject function gives a new instance of service every time it is used, compared to that when you use debugElement.injector.get(Service) you get a reference to the service instance for that component instance 

### unit testing async 

```
it('should create stock through service', async(() => {
        expect(component).toBeTruthy();
        component.stock = new Stock(
            'My New Test Stock', 'MNTS', 100, 120, 'NYSE');
        component.createStock({ valid: true });
        fixture.whenStable().then(() => {
            fixture.detectChanges();
            expect(component.message)
                .toEqual('Stock with code MNTS successfully created');
            const messageEl = fixture.debugElement.query(
                By.css('.message')).nativeElement;
            expect(messageEl.textContent)
                .toBe('Stock with code MNTS successfully created');
        });
    }));
```

- remember to imports FormsModule for module conf in the create-stock test 
- wrap the callback in async (now waitForAsync) 
- we use fixture.whenStable (which waits for asynchronous parts to finish such as createStock in this case) 

- instead of the async function and all the promises you can also use [fakeAsync](https://angular.io/api/core/testing/fakeAsync) which can be used to write async tests almost as if they are sync [another amazing article fakeAsync tick and flush](https://www.digitalocean.com/community/tutorials/angular-testing-async-fakeasync) 
- There are in fact two methods to simulate a passage of time in a fakeAsync test, namely tick() and flush(). tick simulates the passage of time (and can take the number of milliseconds as an argument). flush, on the other hand, takes the number of turns as an argument, which is basically how many times the queue of tasks is to be drained.

### testing http 

```
import { TestBed } from "@angular/core/testing";
import { async } from "@angular/core/testing";
import { StockListComponent } from "./stock-list.component";
import { StockItemComponent } from "../stock-item/stock-item.component";
import { StockService } from "app/services/stock.service";
import { ComponentFixture } from "@angular/core/testing";
import { inject } from "@angular/core/testing";

import { HttpClientModule } from "@angular/common/http";
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { By } from "@angular/platform-browser";


describe('StockListComponent With Real Service', () => {
    let component: StockListComponent;
    let fixture: ComponentFixture<StockListComponent>;
    let httpBackend: HttpTestingController;
    beforeEach(async(() => {
        TestBed.configureTestingModule({
            declarations: [StockListComponent, StockItemComponent],
            providers: [StockService],
            imports: [
                HttpClientModule,
                HttpClientTestingModule
            ]
        })
            .compileComponents();
    }));
    beforeEach(inject([HttpTestingController],
        (backend: HttpTestingController) => {
            httpBackend = backend;
            fixture = TestBed.createComponent(StockListComponent);
            component = fixture.componentInstance;
            fixture.detectChanges();
            httpBackend.expectOne({
                url: '/api/stock',
                method: 'GET'
            }, 'Get list of stocks')
            .flush([{
                name: 'Test Stock 1',
                code: 'TS1',
                price: 80,
                previousPrice: 90,
                exchange: 'NYSE'
            }, {
                name: 'Test Stock 2',
                code: 'TS2',
                price: 800,
                previousPrice: 900,
                exchange: 'NYSE'
            }]);
        }));
    it('should load stocks from real service on init',
        async(() => {
            expect(component).toBeTruthy();
            expect(component.stocks$).toBeTruthy();
            fixture.whenStable().then(() => {
                fixture.detectChanges();
                const stockItems = fixture.debugElement.queryAll(
                    By.css('app-stock-item'));
                expect(stockItems.length).toEqual(2);
            });
        }));
    afterEach(() => {
        httpBackend.verify();
    });
});
```

- we need to import the HttpClient Module so that our service can initialize with the injected HttpClient.
- we include the HttpClientTestingModule from @angular/common/http/testing. This testing module helps automatically mock out the actual server calls, and replaces it with a HttpTestingController that can intercept and mock all server calls.
- after importing it in the imports section Then in our test (or in this case, the beforeEach), we can inject an instance of the HttpTestingController to set our expectations and verify server calls
- we expect that one GET call to the URL /api/stock. We also pass it a human-readable text to print in the logs in case the test fails or that call is not made.
- In addition to setting expectations on calls made, we can also define the response Angular should send when those calls are made. Here, we return an array of two stocks by calling the flush method. The first argument to flush is the response body. the second argument are the options where you can define headers status codes etc. 
- In the afterEach, we call httpBackend.verify(). This ensures that all the expectations we set on the HttpTestingController were actually satisfied during the run of the test. It is generally good practice to do this in the afterEach, to ensure that our code doesn’t make extra or fewer calls.

now to make post http calls 

```
import { TestBed } from "@angular/core/testing";
import { async } from "@angular/core/testing";

import { CreateStockComponent } from "./create-stock.component";
import { StockService } from "app/services/stock.service";

import { ComponentFixture } from "@angular/core/testing";
import { inject } from "@angular/core/testing";

import { HttpTestingController } from '@angular/common/http/testing';
import { By } from "@angular/platform-browser";

describe('CreateStockComponent With Real Service', () => {
    let component: CreateStockComponent;
    let fixture: ComponentFixture<CreateStockComponent>;
    let httpBackend: HttpTestingController;
    beforeEach(async(() => {
        TestBed.configureTestingModule({
            declarations: [
                CreateStockComponent
            ],
            imports: [
                HttpTestingController
            ],
            providers: [
                StockService
            ]
        }).compileComponents();
    }));
    beforeEach(inject([HttpTestingController],
        (backend: HttpTestingController) => {
            httpBackend = backend;
            fixture = TestBed.createComponent(CreateStockComponent);
            component = fixture.componentInstance;
            fixture.detectChanges();
        }));
    it('should make call to create stock and handle failure',
        async(() => {
            expect(component).toBeTruthy();
            fixture.detectChanges();
            component.stock = {
                name: 'Test Stock',
                price: 200,
                previousPrice: 500,
                code: 'TSS',
                exchange: 'NYSE',
                favorite: false
            };
            component.createStock({ valid: true });
            let httpReq = httpBackend.expectOne({
                url: '/api/stock',
                method: 'POST'
            }, 'Create Stock with Failure');
            expect(httpReq.request.body).toEqual(component.stock);
            httpReq.flush({ msg: 'Stock already exists.' },
                { status: 400, statusText: 'Failed!!' });
            fixture.whenStable().then(() => {
                fixture.detectChanges();
                const messageEl = fixture.debugElement.query(
                    By.css('.message')).nativeElement;
                expect(messageEl.textContent).toEqual('Stock already exists.');
            });
        }));
    afterEach(() => {
        httpBackend.verify();
    });
});
```

#### [what's up with object equality check (toEqual) in jasmine ? how does it work](https://scriptverse.academy/tutorials/jasmine-toequal-vs-tobe.html)
#### [test eventEmitter](https://stackoverflow.com/questions/35319476/any-way-to-test-eventemitter-in-angular2)   
and if you want to the reverse just `.not.toHaveBeenCalled`   
#### when specifying status in flush be sure to specify statusText as well or else it will fail to test

- i used fakeAsync in describe, obviously it errros out 

## Routing   

- if you are serving your application from non root then you might need to change this in your index.html  `<base href="/">`
- let's generate a separate module for our routes, just to keep things modular `ng g module app-routes --flat --module=app` (--flat puts the file in src/app instead of its own folder.)

```
//app-routes.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { CreateStockComponent } from './stock/create-stock/create-stock.component';

import { StockListComponent } from './stock/stock-list/stock-list.component';
import { LoginComponent } from './user/login/login.component';
import { RegisterComponent } from './user/register/register.component';

const appRoutes: Routes = [
  { path: 'login', component: LoginComponent },
  { path: 'register', component: RegisterComponent },
  { path: 'stocks/list', component: StockListComponent },
  { path: 'stocks/create', component: CreateStockComponent },
];
@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes), 
  ],
  exports: [
    RouterModule
  ],
})
export class AppRoutesModule { }
```

- for the imports you see we have two options for RouterModule, RouterModule.forRoot and RouterModule.forChild, we can't have more than one router service active therefore in the forRoot which creates a module that  contains router service and forChild which doesn't have the router service 
- Export the RouterModule so that any module importing AppRoutesModule gets access to router directives
- The router-outlet is a directive and is used by the router to mark where in a template, a matched component should be inserted 

```
<div>
<span><a href="/login">Login</a></span>
<span><a href="/register">Register</a></span>
<span><a href="/stocks/list">Stock List</a></span>
<span><a href="/stocks/create">Create Stock</a></span>
</div>
<router-outlet></router-outlet>
```

well this makes the entire page (because of the href) reload every time the links are clicked and so we are gonna use one more directive 

```
<div class="links">
  <span>
    <a routerLink="/login" routerLinkActive="active"> Login </a>
  </span>
  <span>
    <a routerLink="/register" routerLinkActive="active"> Register </a>
  </span>
  <span>
    <a routerLink="/stocks/list" routerLinkActive="active"> Stock List </a>
  </span>
  <span>
    <a routerLink="/stocks/create" routerLinkActive="active"> Create Stock </a>
  </span>
</div>
<router-outlet></router-outlet>
```

- routerLink ensures that all navigations happens within the page 
- routerLinkActive adds the value passed to it as a css class to the element where it is defined whenever the routerLink (specified with it) matches the current link (which class to apply when current link active) 

### adding defaults and wildcards 

```
const appRoutes: Routes = [
  { path: '', redirectTo: '/login', pathMatch: 'full' },
  { path: 'login', component: LoginComponent },
  { path: 'register', component: RegisterComponent },
  { path: 'stocks/list', component: StockListComponent },
  { path: 'stocks/create', component: CreateStockComponent },
];
```

- the default pathMatch value is 'prefix' and so if you don't specify pathMatch : 'full' then every url will match this afterall the paths are checked in the order they are specified 

### required route params 

```
{ path : 'stock/:code', component: StockDetailsComponent},
```

```
import { Component, OnInit } from '@angular/core';
import { StockService } from '../../services/stock.service';
import { ActivatedRoute } from '@angular/router';
import { Stock } from '../../model/stock';

@Component({
  selector: 'app-stock-details',
  templateUrl: './stock-details.component.html',
  styleUrls: ['./stock-details.component.css']
})
export class StockDetailsComponent implements OnInit {

  public stock: Stock;
  constructor(private stockService: StockService,
              private route: ActivatedRoute) { }

  ngOnInit() {
    const stockCode = this.route.snapshot.paramMap.get('code');
    this.stockService.getStock(stockCode).subscribe(stock => this.stock = stock);
  }

}
```

- ActivatedRoute service which holds info about the current activated route 

### navigation in the application 

```
import { Component } from '@angular/core';
import { UserService } from '../../services/user.service';
import { Router } from '@angular/router';

@Component({
  selector: 'app-register',
  templateUrl: './register.component.html',
  styleUrls: ['./register.component.css']
})
export class RegisterComponent {

  public username: string = '';
  public password: string = '';

  public message: string = '';
  constructor(private userService: UserService, private router: Router) { }

  register() {
    this.userService.register(this.username, this.password)
      .subscribe((resp) => {
        console.log('Successfully registered');
        this.message = resp.msg;
        this.router.navigate(['/login']);
      }, (err) => {
        console.error('Error registering', err);
        this.message = err.error.msg;
      });
  }
}
```

- injected the router and navigated using it, it takes an array of commands which together resolves to a particular route 
- router.navigate(['stocks', 'list']), it would navigate to stocks/list route.
- we can also specify the route it is relative to (for example, the current route, which we can obtain by injecting the current ActivatedRoute in the constructor). So if we wanted to navigate to the parent of the current route, we could execute router.navigate(['../'], {relativeTo: this.route}).
- if no starting point (relativeTo) is provided the navigation is absolute 
- similarly for logged in to stocks list flow we can have `this.router.navigate(['stocks/list']);`

to make navigation to stock details component everytime someone clicks on a stock all you have to do is   

```
<div class="stock-container" routerLink="/stock/{{stock.code}}">
...
```

that's it just add the routerLink directive to the top level of the component 

### optional route params 

```
this.router.navigate(['stocks/list'], {
          queryParams: { page: 1 } //passing query parameters to router navigate 
        });
```        

and then `this.route.snapshot.queryParamMap.get('page'));` 
#### notice the difference betweeen paramMap and queryParamMap  
if you use paramMap instead of queryParamMap to get this query parameters you'll get null

#### note: although someDirective="someString" evaluates as a string but if you insert {{}} inside that it will evaluate it as  a template expression 

#### RouterStateSnapshot and RouterState
- snapshot does change even if you change the component from within (navigate to another query parameter of the same component)

```
nextPage(){
    this.page += 1;
    console.log('navigating');
    console.log(this.page);
    this.router.navigate(['stocks/list'], {
      queryParams: {
        page: this.page
      }
    })
    console.log('snapshot of this paramMap is ', this.route.snapshot.queryParamMap.get('page'));//this will give old snapshot tho because it runs from the old component state
  }
  ```

- but if you have your snapshot logic declared in ngOnInit then obviously it will be called only when the component is loaded (when you come from another component to this) 
- so you might need to move the logic to the thing that triggers change or you have to watch out for all changes and use ngDoCheck to check snapshot and make changes (which can be problematic in places such as keyboard input) 
- or you could use an observable to only watch the RouterState which reduces any unncessary complications 
- also Notice the difference between the class `ActivatedRoute` which we used here to both get the snapshot which is updated and the observable and the other class `ActivatedRouteSnapshot` which is an immutable object representing a particular version of ActivatedRoute, it exposes all the properties as plain values [more details here](https://stackoverflow.com/questions/46050849/what-is-the-difference-between-activatedroute-and-activatedroutesnapshot-in-angu)

```
this.route.queryParamMap.subscribe((params) => {
      console.log(params.get('page'));
      this.stocks$ = this.stockService.getStocks();
    })
```

you can do the following to nav to the same page 

```
this.router.navigate([], {
queryParams: {
page: ++this.page
}
})
```

### Route guards 

alright let's make some guards 

`ng g guard guards/auth`

```
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { UserStoreService } from '../services/user-store.service';
import { Observable } from 'rxjs/Observable';


@Injectable()
export class AuthGuard implements CanActivate {
  constructor(private userStore: UserStoreService,
    private router: Router) { }
  canActivate(): boolean {
    console.log('AuthGuard#canActivate called');
    if (this.userStore.isLoggedIn()) { return true };
    console.log('AuthGuard#canActivate not authorized to access page');
    // Can store current route and redirect back to it
    // Store it in a service, add it to a query param
    this.router.navigate(['login']);
    return false;
  }
}
```

- the canActivate can return either an boolean or an observable<boolean> and the rest is pretty much straightforward, if we can activate  a route 

```
//adding the guard to our routes 
  { path: 'stocks/list', component: StockListComponent, canActivate: [AuthGuard] },
  { path: 'stocks/create', component: CreateStockComponent, canActivate: [AuthGuard] },
  { path : 'stock/:code', component: StockDetailsComponent, canActivate: [AuthGuard]},
```

- the canActivate takes an array of guards 
- also make sure to hook the guard in the app module as a provider 

### preventing unload 
The canDeactivate guard is most commonly used to prevent the user from losing data by navigating away unintentionally from a form page, or to autosave the data when the user navigates away from a page

```
import { Injectable } from '@angular/core';
import {  ActivatedRouteSnapshot, RouterStateSnapshot, CanDeactivate, ActivatedRoute } from '@angular/router';
import { CreateStockComponent } from 'app/stock/create-stock/create-stock.component';
import { Observable } from 'rxjs/Observable';

@Injectable()
export class CreateStockDeactivateGuard implements CanDeactivate<CreateStockComponent>{
  constructor(){

  }
  canDeactivate(component: CreateStockComponent, currentRoute: ActivatedRouteSnapshot, currentState: RouterStateSnapshot, 
    nextState?: RouterStateSnapshot): boolean | Observable<boolean> | Promise <boolean>{
      return window.confirm('hey fucker do you actually wanna get out of this window??');
    }
}
```

- the deactivation is in context of an existing component, and so the state of that component is usually important in deciding whether or not the router can deactivate the component and route.

also do it in the routes    

```
{ path: 'stocks/create', component: CreateStockComponent, canActivate: [AuthGuard], canDeactivate : [CreateStockDeactivateGuard] },
```

- note that if you manually change the url from create stock component obviously it can't detect it but if you access a canActivate guard protected component even by directly changing url it can still detect it 

### preloading data using resolve 
- A [Resolver](https://dzone.com/articles/understanding-angular-route-resolvers-by-example) is a class that implements the Resolve interface of Angular Router. In fact, Resolver is a service that has to be [provided] in the root module. Basically, a Resolver acts like middleware, which can be executed before a component is loaded.
- it can be used to preload data before the component is loaded 

```
import { Injectable } from '@angular/core';
import { StockService } from 'app/services/stock.service';
import { Resolve, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';



import { Stock } from '../model/stock';
import { Observable } from 'rxjs/Observable';
@Injectable()
export class StockLoadResolverService implements Resolve<Stock> {
    constructor(private stockService: StockService) { }
    resolve(route: ActivatedRouteSnapshot,
        state: RouterStateSnapshot):
        Stock | Observable<Stock> | Promise<Stock> {
        const stockCode = route.paramMap.get('code');
        return this.stockService.getStock(stockCode);
    }
}
````

- it simply calls the stockService getStocks method and returns the data 
- also hook it up in app module and we also do a change in the routes 

```
{ path : 'stock/:code', component: StockDetailsComponent, canActivate: [AuthGuard], resolve: {
    stock: StockLoadResolverService
  }},
```

- we define how we are going to resolve (the name 'stock' can be anything this name will be used to access the data) 
- we use the data property of the route which is an observable of the static and resolved data of that route 

```
ngOnInit() {
    this.route.data.subscribe((data  : {
      stock: Stock
    }) => {
      this.stock = data.stock;
    })
  }
```  

#### note:  
-  in case we have an observable and we pipe it to make an api call out of the value and thus making it a new observable but if the api call also returns an observable then it will  become an observable of observable   
- in this case you can use mergeMap instead of map which will merge the observables and emit the value inside 

#### [best article on template driven forms](https://www.tektutorialshub.com/angular/angular-template-driven-forms/)

## Productionizing 

- to build `ng build`
- to build for  production `ng build --prod`

#### the second command does a few things 

- bundling: bundles the files into much fewer ones (happens without the prod flag also) 
- minifaction: self explanatory 
- aot: ahead of time compilation , drops unused paths 
- runs angular in production mode, which turns off some checks by angular 
- dead code elimination: stuff like unused modules 

### AOT 
angular used JIT just in time compliation which means angular is compiled at runtime in the browser before running but in production mode it precompiles as much as possible 

### Build optimizer 
a webpack plugin which further optimizes by removing some decorators which aren't relevant in production 

### base href 
in case your app is not served from the root domain (www.something.com/app instead of www.something.com/) then you need to change the base href in the index.html   
or you could use the angular cli `ng build --base-href /app/`  

### caching 
when we build other than index.html all files have a hash which changes if the contents of the files change, and our index file refers to all the other files as such   
- so we don't cache the index.html 
- we cache everything else because if the content changes the name of the css and js files  will change anyway so you don't need to manually check it 

### api server calls and CORS 

- The browser, for security reasons, prevents your web application from making asynchronous calls outside of its domain (this includes subdomains as well). That is, your web application running on www.mytestpage.com can not make an AJAX call to www.mytestapi.com, or even to api.mytestpage.com.
- your frontend server will be the one getting the initial API calls, and it then has to proxy those requests forward to the actual API server.
- however if you can't do that you need to enable CORS (cross origin resource sharing), the api server should respond with an additional header `Access-Control-Allow-Origin: *` we can also set it to only certain origins 

### environments 

- by default when you use create a project with angular cli it creates an src/environments folder which contains different files to set  up different environments 
- you can set environment like `ng serve --env=prod`
- to use the values in the app we just need to import **main environment file** only and angular will provide the values based on the flag (prod or whatever)   

```
import { environment } from './../environments/environment';
//like so
```

### deep linking 
Deep linking is the usage  of the URL, which will take to specific page (content) directly without traversing application from home page

- on deployed application of angular by default if we try to just access anything other than the home page it won't work 
- because when we load the index file it loads all the css, js and when we click on some llink angular intercepts it and serves the proper component for it, so we aren't actually making a request to the server to get a new page, it is behaving as a single page application should 
- now when we directly open up any link other than homepage we request our frontend server for the url but it doesn't know any url other than the one for the index.html and so it won't work 
- so one way is to have index.html as a fallback and then maybe we'll have to start again from base [docs](https://angular.io/guide/deployment#routed-apps-must-fallback-to-indexhtml) ?? I'll have to check this 
- or we could switch to HashLocationStrategy `RouterModule.forRoot(appRoutes, {useHash: true});`
- [a video on routing strategies](https://codecraft.tv/courses/angular/routing/routing-strategies/) 

### Lazy loading 

- instead of defining all routes up front we break our application in smaller modules, each with their routes defined in self contained units 
- the respective components are registered at these submodule level only and not at the main module level 
- at the application level we change our routing to instead point certain subpaths at the new modules rather than the individual routes 

- generate modules along with their routing module `ng g module stock --routing`

let's see an example for user module   

```
//user-routing.module.ts
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { LoginComponent } from './login/login.component';
import { RegisterComponent } from './register/register.component';

const routes: Routes = [
  {path: 'login', component: LoginComponent},
  {path: 'register', component: RegisterComponent}
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class UserRoutingModule { }
```

then we set up the user module 

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

import { UserRoutingModule } from './user-routing.module';
import { FormsModule } from '@angular/forms';
import { LoginComponent } from './login/login.component';
import { RegisterComponent } from './register/register.component';

@NgModule({
  imports: [
    CommonModule,
    FormsModule,
    UserRoutingModule
  ],
  declarations: [
    LoginComponent,
    RegisterComponent
  ]
})
export class UserModule { }
```

now finally in the app routing   

```
import { NgModule } from '@angular/core';
import { RouterModule, Routes }  from '@angular/router';

const appRoutes: Routes = [
  { path: '', redirectTo: 'user/login', pathMatch: 'full' },
  {path: 'stock', loadChildren: 'app/stock/stock.module#StockModule'},
  {path: 'user', loadChildren: 'app/user/user.module#UserModule'},
  { path: '**', redirectTo: 'user/register' }
];

@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes),
  ],
  exports: [
    RouterModule
  ],
})
export class AppRoutesModule { }
```

### server side rendering 
We accomplish this using something known as [Angular Universal](https://angular.io/guide/universal#server-side-rendering-ssr-with-angular-universal). [nice guide on angular universal](https://blog.angular-university.io/angular-universal/)
