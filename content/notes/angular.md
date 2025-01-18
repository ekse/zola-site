+++
title = "Angular"
date = "2025-01-01"
slug = "angular"

[taxonomies]
categories = ["Notes"]
tags = ["angular", "frontend"]

+++

# Angular

Angular uses the Single-Page Application (SPA) pattern. SPAs replace the default navigation of links on pages with javascript functions that handle the routing.
https://blog.pshrmn.com/entry/how-single-page-applications-work/

Quick start:
```
ng new project-name
ng start
```
Concepts in Angular: Components, modules, services, routing, pipes.
https://angular.io/guide/architecture

*Components* map to html tags that can be used in the views. Components are provided inside *modules*, an Angular app is composed of a root module and usually other functional modules. *Services* provide functionality that is not directly related to views. *Routes* are used to map views to urls.

Components are specified with the [@Component](https://angular.io/api/core/Component) decorator.


## Directives

Angular has 3 types of directives: _Components_, _structural directives_ and _attribute directives_. _ngIf_ and _ngFor_ are examples of structural directives. Attribute directives can be used to add custom attributes can be added to elements to change their appearance or behavior.

### Attribute directives

Attribute directives can receive input by adding the @Input decorator to a variable.

```javascript
@Input() highlightColor: string;
```

https://angular.dev/guide/directives/attribute-directives
