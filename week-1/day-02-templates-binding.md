# Day 2 - Angular Templates and Data Binding

In Angular, templates are a key part of the framework that allows developers to define the structure of the user interface. Data binding is the mechanism that connects the data from the component to the view. In this document, we will cover the different types of data binding and provide comprehensive examples.

## Types of Data Binding

There are four types of data binding in Angular:

1. **Interpolation**: This is a one-way data binding from component to view.
   ```html
   <h1>{{ title }}</h1>
   ```
   This binds the `title` property from the component to the `<h1>` element.

2. **Property Binding**: Bind the property of an HTML element to a component property.
   ```html
   <img [src]="imageUrl" />
   ```
3. **Event Binding**: Listen to events such as clicks or key presses.
   ```html
   <button (click)="onClick()">Click me</button>
   ```
4. **Two-way Binding**: Combines property and event binding to keep the data in sync.
   ```html
   <input [(ngModel)]="username" />
   ```

## Structural Directives

Structural directives change the structure of the DOM. The most common examples are `*ngIf`, `*ngFor`, and `*ngSwitch`.

### Example of `*ngIf`
```html
<div *ngIf="isLoggedIn">Welcome, user!</div>
```

### Example of `*ngFor`
```html
<ul>
  <li *ngFor="let user of users">{{ user.name }}</li>
</ul>
```

## Attribute Directives

Attribute directives can change the appearance or behavior of an element. A common built-in directive is `ngClass`.

### Example of `ngClass`
```html
<div [ngClass]="{ 'active': isActive }">Content</div>
```

## Complete Code Examples

### Binding Demo Component
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-binding-demo',
  template: `
    <h1>{{ title }}</h1>
    <img [src]="imageUrl" />
    <button (click)="onClick()">Click me</button>
    <input [(ngModel)]="username" />
  `
})
export class BindingDemoComponent {
  title = 'Angular Data Binding';
  imageUrl = 'https://example.com/image.png';
  username = '';
  onClick() {
    console.log('Button clicked!');
  }
}
```

### Directives Demo Component
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-directive-demo',
  template: `
    <div *ngIf="isVisible">Visible Content</div>
    <ul>
      <li *ngFor="let item of items">{{ item }}</li>
    </ul>
  `
})
export class DirectiveDemoComponent {
  isVisible = true;
  items = ['Item 1', 'Item 2', 'Item 3'];
}
```

## User Management System Project

### Full Implementation
1. **Set Up Project**: Use Angular CLI to generate a new project:
   ```bash
   ng new user-management-system
   cd user-management-system
   ng serve
   ```

2. **Generate Components**: Create necessary components:
   ```bash
   ng generate component user-list
   ng generate component user-detail
   ```

3. **Set Up Routing**: Configure routing in `app-routing.module.ts`.

4. **Implement Services**: Create a service to fetch user data from an API.
   ```typescript
   import { Injectable } from '@angular/core';
   import { HttpClient } from '@angular/common/http';
   import { Observable } from 'rxjs';

   @Injectable({ providedIn: 'root' })
   export class UserService {
     constructor(private http: HttpClient) {}
     getUsers(): Observable<User[]> {
       return this.http.get<User[]>('https://api.example.com/users');
     }
   }
   ```

5. **Build User List**: Use `ngFor` to display the users in the `UserListComponent`.

6. **Detail View**: Route to `UserDetailComponent` for detailed user info.

7. **Test Application**: Use Angular testing tools to verify that binding and directives work as expected.

This comprehensive guide covers Angular Templates and Data Binding, giving you the tools to implement these concepts in your applications.