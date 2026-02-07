# Day 4 - Services and Dependency Injection in Angular

## Understanding Services
In Angular, a service is a class that provides a specific functionality. Services are used to organize and share business logic, models, or data across components. They are typically singleton, which means that a single instance is shared across the application.

### Creating a Service
You can create a service using the Angular CLI. Run the following command:

```bash
ng generate service serviceName
```

This creates a service file and its corresponding spec file for testing.

### Injecting Services into Components
Services are injected into components using Angular's Dependency Injection (DI) system. This allows you to use the methods and properties of a service in your component.

#### Example:
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class ExampleService {
  getData() {
    return ['data1', 'data2'];
  }
}
```

To use this service in a component:
```typescript
import { Component } from '@angular/core';
import { ExampleService } from './example.service';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html'
})
export class ExampleComponent {
  constructor(private exampleService: ExampleService) {
    const data = this.exampleService.getData();
    console.log(data);
  }
}
```

## Dependency Injection (DI)
DI is a design pattern used in Angular to manage how dependencies are supplied to classes. Instead of instantiating dependencies directly, Angular injects them, making the code easier to manage and test.

### Hierarchical DI
Angular's DI system is hierarchical, meaning that services can be provided at various levels of the application, allowing for a flexible organization of services.

### Summary  
- Services are reusable components that encapsulate logic and data in Angular applications.  
- Use the Angular CLI to generate services.  
- DI allows you to manage dependencies efficiently, improving testability and maintainability.