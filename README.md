# Angular-notes
## Some code block
### Detect browser 10/6/2021
```javascript
// new Opera outputs true for window.chrome
// but needs to check if window.opr is not undefined
// and new IE Edge outputs to true now for window.chrome
// and if not iOS Chrome check
// so use the below updated condition
const isChromium = window.chrome;
const winNav = window.navigator;
const vendorName = winNav.vendor;
const isOpera = typeof window.opr !== "undefined";
const isIEedge = winNav.userAgent.indexOf("Edge") > -1;
const isIOSChrome = winNav.userAgent.match("CriOS");
      
const isAndroid: /Android/.test(navigator.userAgent),
const isCordova: !!window.cordova,
const isEdge: /Edge/.test(navigator.userAgent),
const isFirefox: /Firefox/.test(navigator.userAgent),
const isChrome: /Google Inc/.test(navigator.vendor),
const isChromeIOS: /CriOS/.test(navigator.userAgent),
const isChromiumBased: !!window.chrome && !/Edge/.test(navigator.userAgent),
const isIE: /Trident/.test(navigator.userAgent),
const isIOS: /(iPhone|iPad|iPod)/.test(navigator.platform),
const isOpera: /OPR/.test(navigator.userAgent),
const isSafari: /Safari/.test(navigator.userAgent) && !/Chrome/.test(navigator.userAgent),
const isTouchScreen: ('ontouchstart' in window) || window.DocumentTouch && document instanceof DocumentTouch,
const isWebComponentsSupported: 'registerElement' in document && 'import' in document.createElement('link') && 'content' in document.createElement('template')

if (isIOSChrome) {
  // is Google Chrome on IOS
} else if(
  isChromium !== null &&
  typeof isChromium !== "undefined" &&
  vendorName === "Google Inc." &&
  isOpera === false &&
  isIEedge === false
) {
  // is Google Chrome
} else { 
  // not Google Chrome 
}
      
```      
Or other
```javascript
const GOOGLE_VENDOR_NAME = 'Google Inc.';

function isOpera(){
  return Boolean(window.opr);
}

function isChromium() {
  return Boolean(window.chrome);
}

function getBrowserName() {
        const userAgent = window.navigator.userAgent;
        const vendor = window.navigator.vendor;
        switch (true) {
          case /Edge|Edg|EdgiOS/.test(userAgent):
            return 'Edge';
          case /OPR|Opera/.test(userAgent) && isOpera():
            return 'Opera';
          case /Chrome|CriOS/.test(userAgent) && vendor === GOOGLE_VENDOR_NAME && isChromium():
            return 'Chrome';
          case /Vivaldi/.test(userAgent):
            return 'Vivaldi';
          case /YaBrowser/.test(userAgent):
            return 'Yandex';
          case /Firefox|FxiOS/.test(userAgent):
            return 'Firefox';
          case /Safari/.test(userAgent):
            return 'Safari';
          case /MSIE|Trident/.test(userAgent):
            return 'Internet Explorer';
          default:
            return 'Unknown';
        }
      }

function isChrome() {
        const name = getBrowserName();
        return name === 'Chrome';
}
```
## Rxjs
Reactive Programming is programming with asynchronous data streams
Angular uses RxJS observables. I can’t understand something until I create it with my own hands

## Angular route
### data setup in routing
Use case for detect user role
```javascript
    const routes: Routes = [
     {
         path: "admin",
         component: AdminOnlyComponent,
         canActivate: [RoleGuardService],
         data: { roles: ['admin']}
     },
    ...
    canActivate(route: ActivatedRouteSnapshot): boolean {
      return route.data.roles.some( ai => this.getUserRoles.getRoles().includes(ai) );
      }
    }
```
## Angular performance
https://www.grapecity.com/blogs/14-ways-to-speed-up-your-angular-app
1. Enabling production mode
2. AOT
3. Minification
4. Avoiding function calls and getters in views
5. Pure pipes
6. Lazy loading modules
7. Code splitting
8. OnPush change detection
9. Async pipe
10. ngDoCheck
11. Track by function
12. Zone.js
13. Unsubscribing observables
14. Web workers
### Speeding Up Your Angular App
As Angular expert Bonnie Brennan says, "to get the most performance out of Angular, you need to think of it as a sports car, not as a pickup truck." That means Angular should be very lean and fast, so let it run freely. For example, it doesn't have to detect changes in every piece of data of your model.

### Build and Deploy Optimizations
#### Production Build
Before deploying your application, make sure you create a production build. This mode performs many significant optimizations not available in a development build, including ahead-of-time compilation, minification, and tree-shaking.

#### JIT vs. AOT
Angular provides two compilation models: just-in-time (JIT), which compiles your app at runtime, and ahead-of-time (AOT), where compilation occurs at build time. By default, the development compilation uses the JIT compilation, which requires you to include the Angular compiler.

AOT anticipates compilation at build time, produces only the compiled templates, and removes the Angular compiler from the deployment bundle, which reduces your app payload by around 1MB (roughly the size of the Angular compiler).

You can compile using CLI commands with the -- AOT switch to take advantage of AOT optimizations:

ng build --aot 
ng serve --aot

#### Minification
Many characters in our JavaScript code, including white spaces, newline characters, comments, and block delimiters, are used just for readability and visual purposes. They aren't necessary for the code to run correctly. The minification process removes these characters, simplifies names, and ignores unreachable code. By minifying your code, you speed up page download and execution times.

Now, compare this code, before minification:

var app = angular.module("myApp", []);
    app.controller("myCtrl", function($scope) {
    $scope.title = "Lorem";
    $scope.subtitle = "Ipsum";
    if (false) {
        console.log('Lorem Ipsum Dolor');
    }
});
with this code after minification:

var app=angular.module("myApp",[]);app.controller("myCtrl",function(l){l.title="Lorem",l.subtitle="Ipsum"});
Build Optimizer
Created by the Angular team, Build Optimizer is a tool that further optimizes Angular Webpack builds. It identifies code that can be removed at build time without side effects. For instance, Build Optimizer can remove Angular decorators like @Component from AOT builds. Since the compiler extracts all necessary information from these decorators, they are needed only at compile time.

You can browse the project's GitHub repository main page to understand how Angular Build Optimizer implements these transformations.

Creating a Production Build Using Angular Optimizations
The above optimizations will be applied if you perform a production build. The following CLI instruction allows a straightforward deployment by creating a production build:

ng build --prod
After that, the generated output directory can be copied to a web server. If you use the prod flag, you may start seeing errors you wouldn't get without it. But that's a good thing: now you have the opportunity to catch and solve bugs that would otherwise show up only later, at runtime.

Enabling Production Mode
By default, Angular runs in debug mode, which adds some assertion checks, but also runs ChangeDetection twice each time to ensure there are no unexpected changes to binding values. To call ChangeDetection only once, you need to enable production mode by adding the following code to your Angular app:

import { enableProdMode } from '@angular/core';
import { environment } from './environments/environment';

if (environment.production) {
    enableProdMode();
}
Useful Angular Demo
Alexey Zuev, author and Google Developer Expert, built an online IDE that supports Angular development projects out-of-the-box. You can use it to test Angular AOT and JIT compilation online by going to the site. There, ng-run will instantly perform ahead-of-time compilation on the sample application:

14 Ways to Speed Up Your Angular App

After AOT compilation, the app is deployed, but without the Angular compiler:

14 Ways to Speed Up Your Angular App

Now let's test it with JIT compilation by turning off the Ivy AOT switch:

14 Ways to Speed Up Your Angular App

When we change the compilation mode to JIT, the only difference seems to be the absence of the "Ivy AOT compilation…" message when the application starts. However, when we open Chrome tools to compare the downloaded files, we see that JIT mode requires the application to include the Angular compiler and related bundles, which don't need to be downloaded when the app is precompiled with AOT:

14 Ways to Speed Up Your Angular App

Angular Code Optimizations
Lazy Loading Modules
Angular provides lazy loading, an easy way to load pages on demand. With lazy loading, a module is loaded only when the user navigates to the route of that particular module. The Angular team built this feature into the router, and it's all done under the hood for you. Therefore, lazy loading is straightforward to use.

However, one fact that is overlooked, yet imperative for taking advantage of lazy loading, is this: it requires more modules. A benefit of dividing the application into modules is the ability to load modules on demand. If you've written 30,000 lines of code and they're all in one module, you won't be able to leverage the lazy loading feature, and your app may grow slower and slower.

Here's an example of route configuration without lazy loading:

const routes: Routes = [
  {
    path: 'customers',
    loadChildren: CustomersComponent
  },
  {
    path: 'orders',
    loadChildren: OrdersComponent
  }
];
And the same route configuration after lazy loading was applied:

const routes: Routes = [
  {
    path: 'customers',
    loadChildren: () => import('./customers/customers.module').then(m => m.CustomersModule)
  },
  {
    path: 'orders',
    loadChildren: () => import('./orders/orders.module').then(m => m.OrdersModule)
  }
];
Code Splitting
One way of decreasing initial load time and speeding up page navigation is through code splitting. As web applications became more complicated, the JavaScript files shipped to users grew in size. Large JavaScript files can delay interaction time in the browser, particularly for mobile users.

Code splitting efficiently reduces JavaScript bundles in your app with no loss of features. This technique lets you break the JavaScript code into multiple parts that can be progressively loaded as the user navigates to different routes, or when the user opens or expands components.

There are two main approaches to it: component-level code-splitting, where individual components can be loaded lazily even without route navigation, and route level code-splitting, where individual routes are loaded lazily.

OnPush Change Detection
By default, Angular will check every component to see if there have been changes and update the view accordingly. While this is a relatively fast process, as your application grows, these frequent update checks become slower.

Unlike the default strategy, which checks a component whenever there's a change in your app, OnPush change detection reacts only to changes in the @input parameters, or when you manually trigger detection.

To enable OnPush change detection, define this strategy in the component decorator:

import { ChangeDetectionStrategy, Component, Input } from '@angular/core';

@Component({
    selector: 'app-user-list',
    templateUrl: './user-list.component.html',
    changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserListComponent {
    @Input() users$: any[];
    trackByFn(index: number, item: any): any {
        return item.id;
    }
}
ngDoCheck Lifecycle Hook
Angular invokes ngDoCheck each time the change detection runs. Therefore, ngDoCheck is the perfect place in your components to add custom logic that's computationally intensive or slow or to detect changes that would otherwise be ignored by Angular:

    ngDoCheck() {
        const cartChanges = this.differ.diff(this.shoppingCart);

        if (cartChanges) {
            console.log(cartChanges);
            cartChanges.forEachChangedItem(r => ...);
            cartChanges.forEachAddedItem(r => ...);
            cartChanges.forEachRemovedItem(r => ...);
        }

    }
Here, ngDoCheck is used to apply the differ.diff method to the current value of the collection. The differ will then compare the collection with its previous value and return the list of changes.

You can move complex calculations into the ngDoCheck lifecycle hook and refer to the calculated value in your view. Remember, caching complex calculation results will yield better performance.

Async Pipe
When using observables, calling the subscribe method, but forgetting to invoke unsubscribe, subsequently causes memory leaks. Memory leaks are why the async pipe is your friend: it handles all the cleanup for you. It not only subscribes for you, but also takes care of unsubscribing when you close a component, and it calls markForCheck for each update. Thus, the async pipe is the perfect match for the OnPush change detection strategy.

The async pipe enables you to use RxJS observables directly in a view. As you can see in the code below, whenever the async pipe updates a value, it automatically calls markForCheck for you. As a result, you inject only your data service, and you don't place the actual value in a field in the component. Instead, you add a reference to the observable. Now, your components will also detect changes when there's an update sent to this observable:

<span>Wait for it... {{ greeting | async }}</span>
Avoid Function Calls and Getters in Views
When binding to an object, Angular performs change verification in the model attributes very quickly, as it doesn't need to execute any functions. However, when you bind to a function or a JavaScript getter, Angular must run your function to check whether the value has changed. In some cases, Angular performs change detection frequently, causing severe performance issues in your app.

To avoid such issues, never bind to functions or getters in Angular template expressions if it can be avoided. Instead, use pure pipes to let Angular efficiently skip pipe execution if the value doesn't change. You can also manually compute the values in your component's controller and recalculate them when required:

export class MovieComponent {
    header: string = 'Movie details';
    title: string;
    synopsis: string;
    duration: number;

    constructor(http: HttpClient) {
        http.get('https://my-movie-database.com/api/movie/'
        , (result) => {
            this.title = result.title;
            this.synopsis = result.synopsis;
            this.duration = result.duration;
        });
    }
}
Pure Pipes
Sometimes, you need to call a function in the view, yet in many cases, you can use a pipe instead. Notice how the pure pipe function must implement PipeTransform:

import {Pipe, PipeTransform} from '@angular/core';

@Pipe({name: 'repeat'})
export class RepeatPipe implements PipeTransform {
  transform(value: any, times: number) {
    return value.repeat(times);
  }
}
This can be implemented in a template like so:

<span> {{ 'Bla' | repeat:5 }}</span>
And that template will be rendered as:

<span>BlaBlaBlaBlaBla</span>
Zone.js
Sometimes, your views aren't updated, and later you find out that Angular doesn't detect changes when the model is updated by asynchronous functions, such as setInterval, setTimeout, mouse events, or a promise rejection. To fix this, people resorted to AngularJS alternatives to these async functions, but they still had to update the view programmatically.

Years ago, Angular 2 introduced Zone.js as a means to patch these asynchronous browser functions with automatic change detection. With Zone.js, your async functions were able to update the view automatically. It was a welcome enhancement, yet it also meant that every single async execution triggered a change detection.

The triggered change detection is where ngZone comes in. After you identify which async functions don't affect the model and don't require view updates, you can tell ngZone to run these functions outside the Angular context:

export class AppComponent {
  constructor (ngZone: NgZone) {
     ngZone.runOutsideAngular(() => {
      // runs outside Angular zone, for performance-critical code

      ngZone.run(() => {
      // runs inside Angular zone, for updating view afterwards
     });
   });
  }
}
In this Stackblitz demo, you can see how running a loop outside of the Angular zone doesn't cause the UI to refresh after each setTimeout cycle:

14 Ways to Speed Up Your Angular App

Unsubscribing Observables
Unsubscribing is easy: you store the subscription, then use the ngOnDestroy lifecycle hook function to call unsubscribe on the subscription object.

Luckily, the ngOnDestroy lifecycle hook creates a good pattern, so you can appropriately handle memory during subscriptions. For each component or directive, use the ngOnDestroy callback method, where you call unsubscribe for your subscriptions:

import { OnDestroy } from '@angular/core';

export class MyCleanupComponent implements OnDestroy {
    private _subs: Subscription;

    constructor(router: Router) {
        this._subs = router.events.subscribe(event => {
            //Event must be handled here...
        });
    }

    ngOnDestroy(): void {
        this._subs.unsubscribe();
    }
}
Track by Function
Manipulating the DOM is an expensive task. By default, ngFor performs a simple equality check to see if items have changed. The ngFor directive provides the trackBy function, which determines how Angular will track changes in objects inside a collection so that ngFor can perform efficient updates.

When objects change inside the collection, the directive must redraw the right DOM elements. Since not all DOM nodes are affected, only the elements that have changed are rerendered.

In HTML:

<li *ngFor="let item of strategyItem; trackBy: trackByFn">{{ item }}</li>
In TypeScript:

public trackByFn(index, item) {
    if (!item) return null;
    return item.id;
}
Web Workers
While you don't see many applications out there implementing web workers, they can be a useful asset for running CPU-intensive tasks in a background thread without blocking your application's main thread or freezing the UI.

However, consider two limitations before implementing web workers in your Angular apps:

Some environments or platforms (such as @angular/platform-server) don't support web workers.

The Angular CLI doesn't support running Angular itself in a web worker via @angular/platform-webworker.

Operational Tips for Finding Optimizations
Profiling
Modern browsers provide performance profiling tools to help identify slow-running code. You can further expand your toolbelt with a module called Webpack Bundle Analyzer, which allows you to visualize files generated by Webpack. Each file is represented by a rectangle whose size is relative to the file size.

Webpack Bundle Analyzer will help you identify the modules included in the bundle, the ones that take up more space, and the ones added by mistake. Bundle Analyzer also gets into minified bundles to discover their real sizes.

14 Ways to Speed Up Your Angular App

Lighthouse works as a built-in Chrome dev tool to identify and fix common problems that affect your site's performance, accessibility, and user experience.

14 Ways to Speed Up Your Angular App

Lighthouse allows you to visualize different categories of issues concerning performance, progressive web apps, best practices, SEO, and multi-platform web apps. When you run Lighthouse, it executes your web application and starts collecting metrics. The resulting report provides detailed performance scores by category and proposes solutions for each detected issue:

14 Ways to Speed Up Your Angular App

Angular CLI Budgets Tool
Large JavaScript files can slow application download and delay user interaction. These files are why some Angular optimization techniques—including minification, AOT compilation, and tree-shaking—focus on reducing the final size of the application bundles.

During development, you'll most likely add libraries to expand your application features, but this results in larger production files. You want to keep the size of your bundles under control, so they don't exceed some reasonable, predefined limits. Angular CLI Budgets is a feature that enables you to define the maximum desired sizes for your application bundles and to warn you whenever these thresholds exceed their limit.

Optimization Checklist for Your Angular App
As we've seen, the optimization techniques discussed here deal with different problems with diverse solutions, and not all tips will have the same impact on the performance of your application.

This list of Angular optimization tips will likely to produce gains to fine-tune your app, taking advantage of these features will help your Angular app to be smaller, faster, and more responsive.
