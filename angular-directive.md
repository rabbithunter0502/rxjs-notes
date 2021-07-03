# Angular Directives
### Concepts
* <b>Angular.io: </b>
  Directives are classes that add additional behavior to elements in your Angular applications.
  Technically, a Directive is essentially a typescript class which has been annotated with a TypeScript Decorator.
* <b>Other explain: </b>
  Directives are custom HTML attributes which tell angular to change the style or behavior of the DOM elements.
### Types
  
  ![1_OhepgWassGUMkQ6v8VtK3g](https://user-images.githubusercontent.com/43747716/124205407-5dd07d80-db0b-11eb-9fe2-e3869f09d412.png)

1. Components — directives with a template.
2. Structural directives — change the DOM layout by adding and removing DOM elements.
3. Attribute directives — change the appearance or behavior of an element, component, or another directive.

When we say that components are the building blocks of Angular applications, we are actually saying that directives are the building blocks of Angular applications.

### 1. Structure directive
#### Create custom structure directive from angular.io
* Create UnlessDirective selector which is opposite of NgIf => with "true" condition, NgIf displays the template content and Unless doesn't
```js
  <p *appUnless="condition">Show this sentences with condition is false</p>
```
* Glossary
  * view
  * embedded view
  * view-container   
  * Decorator
    > A Decorator is a special kind of declaration that can be attached to a class declaration, method, accessor, property, or parameter
    
* Directive 
```js
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

/**
 * Add the template content to the DOM unless the condition is true.
 */
@Directive({ selector: '[appUnless]'})
export class UnlessDirective {
  private hasView = false;

  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef) { }

  @Input() set appUnless(condition: boolean) {
    if (!condition && !this.hasView) {
      this.viewContainer.createEmbeddedView(this.templateRef);
      this.hasView = true;
    } else if (condition && this.hasView) {
      this.viewContainer.clear();
      this.hasView = false;
    }
  }
}
```
