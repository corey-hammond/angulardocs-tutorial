# Angular Docs Tutorial (angular.io)

# Notes

## Template Syntax

Angular's template syntax extends HTML and Javascript

*ngFor 
: <div *ngFor="let product of products"></div>
: The div will repeat itself for each product in the product list.
: *ngFor is a "structural directive". Structual directives shape or reshape the DOM's structure. Any directive with an * is a structural directive 

*ngIf
: <p *ngIf="product.description"> Description: {{ product.description }} </p>
: Structural directive that only creates the element if the product description property is not empty

Interpolation syntax {{ }}
: <h3> {{ product.name }} </h3>
: Interpolation renders a property's value as text

Property binding syntax [ ]
: <app-product-alerts [title]="product.name + ' details'">
: Property binding lets you use the property value in a template expression

Event binding syntax ( )
: <button (click)="share()"> Share </button>
: Wrap the event in a set of parenthesis
: Reaches out to the share() method in the related component.ts file

## Components

Components define areas of responsibility in the user interface that let you reuse sets of UI functionality

A component consists of three things:
* A component class that handles data and functionality. 
* An HTML template that determines the UI. 
* Component-specific styles that define the look and feel. 

An Angular application comprises a tree of components, in which each Angular component has a specific purpose and responsibility

app-root is the application shell, which is the first component to load and is the parent of all other components

## Input

@Component
: The @Component decorator indicates that the following class is a component and it provides the metadata about the component
: Metadata includes the selector, templates, and styles. 
: The selector is the name of the component with a prefix of app-, and it is how you identify the component.

@Input
: @Input() product;
: In the class definition, define a property (product) with an @Input() decorator
: To receive data as input in a component, use the @Input() decorator, which indicates that the property value passes in from the component's parent.

Use property binding to pass data as input to the child component
: <app-product-alerts [product]="product"></app-product-alerts>
: The product-alerts component takes a product as input from the product-list component. 




