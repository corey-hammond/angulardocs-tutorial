# Angular Docs Tutorial (angular.io)

# Fundamentals of Angular

## Template Syntax

Angular's template syntax extends HTML and Javascript

*ngFor
: <div *ngFor="let product of products"></div>
: The div will repeat itself for each product in the product list.
: _ngFor is a "structural directive". Structual directives shape or reshape the DOM's structure. Any directive with an _ is a structural directive
: With the *ngFor directive you can assign the index of each item to a variable - *ngFor="let product of products; index as productId"

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

- A component class that handles data and functionality.
- An HTML template that determines the UI.
- Component-specific styles that define the look and feel.

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

## Output

- A child component emits an event to the parent component; parent component acts on that event
- In child component class, define a property with an @Output decorator and an instance of EventEmitter() - @Output() notify = new EventEmitter();
- In child component, add an event binding call the property's .emit() method - (click)="notify.emit()"
- Parent component acts on event, not child, so you define a method in the parent component class
- Finally, bind the child's output component to the parent's new method - (notify)="onNotify()"

# Routing

## Registering a Route

- A route associates one or more URL paths with a component. Add a route for the new component in the app.module.ts file:

```
@NgModule({
  imports: [
    BrowserModule,
    ReactiveFormsModule,
    RouterModule.forRoot([
      { path: '', component: ProductListComponent },
      { path: 'products/:productId', component: ProductDetailsComponent },
    ])
  ],
```

- Use the RouterLink directive to define a link in the component template.
- In your anchor tag - [routerLink]="['products', productId]"

## Using Route Information

- In the new route's component, inject ActivatedRouter into the constructor:

```
export class ProductDetailsComponent implements OnInit {
  product;

  constructor(
    private route: ActivatedRoute,
  ) { }

}
```

- This ActivatedRoute is specific to each routed component and contains information about the route, its parameters, and additional info
- In the ngInit() method, subscribe to route params and fetch the product based on productId:

```
ntOnInit() {
  this.route.paramMap.subscribe(params => {
    this.product = products[+params.get('productId')]
  })
}
```

- The route parameters correspond to the path variables defined in the route (in this case, in the product-list.component.html)

# Managing Data

## Services

A service is an instance of a class that can be made available to any part of your application using Angular's "dependency injection system." Services are the place where you share data between parts of your application.

- Generate a service - ng g c serviceName (cart)
- Include a place to store data here (array, object, ect) along with methods to manipulate that data
- Import this service into any components that you would like to have access to this data - import { CartService } from '../cart.service'
- Inject the service into the component's constructor - private cartService: CartService
- Define methods in the component that use the service's methods - this.cartService.addToCart(product)

# Forms

Forms in Angular take the standard capabilities of the HTML based forms and add an orchestration layer to help with creating custom form controls, and to supply great validation experiences. There are two parts to an Angular Reactive form, the objects that live in the component to store and manage the form, and the visualization of the form that lives in the template.

## Define the Form Model

The form model is the source of truth for the status of the form and is defined in the component class. Angular's FormBuilder service provides convenient methods for generating controls. Import and inject this service before you can use it.

- In your component class, define a form property to store the form model - checkoutForm;
- Use FormBuilder group() method to set the property (checkoutForm) with a form model containing one or more fields:

```
this.checkoutForm = this.formBuilder.group({
  name: '',
  address: ''
});
```

- We have defined the form model in the component class, now to reflect the model in the view, we need to create a checkout form in the template

## Create a Form in the Template

- In your component template, add an HTML form
- Use a formGroup property binding to bind the checkoutForm to the HTML form tag in the template, and add a standard submit button:

```
<form [formGroup]="checkoutForm">
  <button class="button" type="submit"> Submit </button>
</form>
```

- On the HTML form tag, use an ngSubmit event binding to listen for the form submission and call a method (onSubmit()) with the checkoutForm value:

```
<form [formGroup]="checkoutForm" (ngSubmit)="onSubmit(checkoutForm.value)"></form>
```

- Add your input fields and use the formControlName attribute binding to bind the checkoutForm form controls to their input fields:

```
<form [formGroup]="checkoutForm" (ngSubmit)="onSubmit(checkoutForm.value)">

  <div>
    <label for="name">
      Name
    </label>
    <input id="name" type="text" formControlName="name">
  </div>

  <div>
    <label for="address">
      Address
    </label>
    <input id="address" type="text" formControlName="address">
  </div>

  <button class="button" type="submit">Purchase</button>

</form>
```

# Deployment

To deploy your application, you have to compile it, and then host the JavaScript, CSS, and HTML on a web server. Built Angular applications are very portable and can live in any environment or served by any technology, such as Node, Java, .NET, PHP, and many others.

- To build and produce the files you need to deploy - ng build --prod
- To host the built project:
  - The files in the dist/my-project-name folder are static and can be hosted on any web server capable of serving files(Node.js, Java, .NET) or any backed (Firebase, Google Cloud, App Engine, others).

## Hosting an Angular App on Firebase

One of the easiest ways to get your site live is to host it using Firebase.

1. Sign up for a firebase account on Firebase.
2. Create a new project, giving it any name you like.
3. Install the firebase-tools CLI that will handle your deployment using npm install -g firebase-tools.
4. Connect your CLI to your Firebase account and initialize the connection to your project using firebase login and firebase init.
5. Follow the prompts to select the Firebase project you are creating for hosting.

- Select the HOSTING option on the first prompt.
- Select the project you previously created on Firebase.
- Select dist/my-project-name as teh public directory.

1. Deploy your application with firebase deploy, because the command firebase init has created a firebase.json file that tells Firebase how to serve your app.
2. Once deployed, visit https://your-firebase-project-name.firebaseapp.com to see it live!

# Notes

- Install the Angular CLI globally - npm install -g @angular/cli

  - This will install the 'ng' command into your system, which is the command you use to create new workspaces, new projects, serve your app during development, or produce builds that can be shared or distributed.
  - Create a new Angular CLI workspace using the 'ng new' command - ng new my-project-name

- Angular calls ngOnInit() shortly after creating a component
