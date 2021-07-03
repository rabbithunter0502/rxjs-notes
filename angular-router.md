# Angular router

## Glossary:
 * router module
 * PRIMARY_OUTLET : this is default outlet which is known as the PRIMARY_OUTLET, Component are rendered using <router-outlet> directives and is <router-outlet> sibling.
 * secondary outlets:
    App can have named <router-outlet> directives which are known as secondary outlets:
    * How to define:
    ```js
      <router-outlet name="sidebar"></router-outlet>
      <ng-component>Chat components go here</ng-component>

      const ROUTES = [
        { path: 'home', component: HomeComponent },
        { path: 'chat', component: ChatComponent, outlet: 'sidebar' }
      ]

      localhost:4200/home(sidebar:chat)
    ```
 * route-state
 * navigation
 * lazy-loading
 * routable
 * UrlTree
    * Concept: 
      Represents the parsed URL.
      Since a router state is a tree, and the URL is nothing but a serialized state,
      the URL is a serialized tree.
      UrlTree is a data structure that provides a lot of affordances in dealing with URLs
    * Refer : https://github.com/angular/angular/blob/c9b47f21822343286d056fd70aec882acc0ae788/packages/router/src/url_tree.ts#L189
      
      ```js
         @Component({templateUrl:'template.html'})
         class MyComponent {
            constructor(router: Router) {
              const tree: UrlTree =
                router.parseUrl('/team/33/(user/victor//support:help)?debug=true#fragment');
              const f = tree.fragment; // return 'fragment'
              const q = tree.queryParams; // returns {debug: 'true'}
              const g: UrlSegmentGroup = tree.root.children[PRIMARY_OUTLET];
              const s: UrlSegment[] = g.segments; // returns 2 segments 'team' and '33'
              g.children[PRIMARY_OUTLET].segments; // returns 2 segments 'user' and 'victor'
              g.children['support'].segments; // return 1 segment 'help'
            }
          }
      ```
      * <strong>A UrlSegmentGroup </strong>Represents the parsed URL segment group.
      * <strong>A UrlSegment </strong>is a part of a URL between the two slashes.
        It contains a path and the matrix parameters associated with the segment.
 


## Stage 1: The router gets a new URL
* A ROUTE OBJECT defines a realtionship between some routable state in app (components, redirects...) and a segment of a URL 
* A ROUTES ARRAY is a set of ROUTE OBJECT and declaratively in forRoot method such as RouterModule.forRoot(ROUTES).
* The routable portions == a tree of router states and to be defined by router object,


An URL being consumed and matched against config in the ROUTES array

![image-196](https://user-images.githubusercontent.com/43747716/124342302-233f1180-dbed-11eb-9264-0e6b9f51ea5f.png)
  
OnInit, The router handles URLs internally.
  we might expect the router to store URLs as strings internally.<br>
  But, since URLs are serializations of ROUTER STATE
  
  ![image-197 (1)](https://user-images.githubusercontent.com/43747716/124342445-3d2d2400-dbee-11eb-9a04-c9dfd322ec85.png)
  
  We can break this structure down using the Router service:
  ```js
    const url = '/users/1/notes/42(sidebar:secondary1)?lang=en#line99';
    const tree = this.router.parseUrl(url); // '/users/1/notes/42(sidebar:secondary1)?lang=en#line99'
 
    const fragment = tree.fragment;         // line99
    const queryParams = tree.queryParams;   // lang=en
    const primary: UrlSegmentGroup = tree.root.children[PRIMARY_OUTLET];  // gets the UrlSegmentGroup for the primary router outlet
    const sidebar: UrlSegmentGroup = tree.root.children['sidebar'];       // gets the UrlSegmentGroup for the secondary router outlet (sidebar)
    const primarySegments: UrlSegment[] = primary.segments;   // returns all UrlSegments for the primary outlet. ['users','1','notes','42']
    const sidebarSegments: UrlSegment[] = sidebar.segments;   // returns all UrlSegments for the secondary outlet. ['secondary1']
  ```
  router.parseUrl(url) on line 2 will convert the URL string into the UrlTree:
  
  ![image-198](https://user-images.githubusercontent.com/43747716/124342587-0dcae700-dbef-11eb-87f6-fb2a2fae0986.png)

    <h4>Brief</h4>
    * The entire URL is represented as a UrlTree.
    * Interior nodes of the tree (those which have child nodes of UrlSegments) are represented as UrlSegmentGroups. These are usually associated with a specific router outlet, such as primary and sidebar in the example above.
    * Leaf nodes (those with no children) are represented as UrlSegments. A UrlSegment is any part of a URL occurring between two slashes, for instance /users/1/notes/42 has four segments, users 1 notes and 42. These are what will be matched to path properties in the router configurations in ROUTES. UrlSegments can also contain matrix parameters, which are data specific to a segment. Matrix parameters are separated by semicolons ;, such as name and type in the example/users;name=nate;type=admin/.
    * The root node has a child UrlSegmentGroup for each outlet. In this case, it has two; one for the default outlet (primary), and one for the secondary outlet (sidebar). Internally, the router serializes secondary outlets in the URL within parenthesis, such as (secondary_outlet_name:secondary_path_name), and matches them to configuration objects which have a matching outlet property, such as {path: ‘secondary_path_name’, outlet: ‘secondary_outlet_name'}. We’ll see later that outlets are routed independently of each other.
    * Fragments and query params live as properties on the UrlTree.
    * A new UrlTree is generated each time the URL changes. UrlTree creation happens synchronously, and independently from the task of matching the URL to something in the ROUTES configuration tree. This is an important distinction because matching may be asynchronous. For instance, matching might require a router configuration from a lazily-loaded module to be loaded asynchronously. We’ll see more on this in the next section on redirects.
    
## Stage 2: Applying Redirects
  
