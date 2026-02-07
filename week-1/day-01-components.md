# Day 1: Angular Architecture & Components

## 🎯 Learning Objectives

By the end of today, you will:
- Understand Angular architecture
- Master component creation and structure
- Implement all lifecycle hooks
- Build parent-child component communication
- Complete a hands-on counter project

---

## 🌅 Morning Session (7:00 AM - 8:00 AM) - Theory

### What is Angular?

Angular is a **TypeScript-based**, open-source web application framework developed and maintained by Google. It's designed for building **single-page applications (SPAs)** with a focus on:

- **Component-based architecture** - Build UIs as reusable components
- **Two-way data binding** - Automatic sync between model and view
- **Dependency Injection** - Manage dependencies efficiently
- **Routing** - Navigate between views
- **RxJS** - Handle asynchronous operations
- **TypeScript** - Type safety and modern JavaScript features

### Angular Architecture

```
Angular Application
├── Components (UI Building Blocks)
├── Services (Business Logic)
├── Modules (Feature Organization)
├── Directives (DOM Manipulation)
├── Pipes (Data Transformation)
└── Routing (Navigation)
```

### Component Structure

```typescript
@Component({
  selector: 'app-example',      // How to use in HTML
  template: `<h1>Hello</h1>`,   // Component HTML
  styles: [`h1 { color: blue }`] // Component CSS
})
export class ExampleComponent {
  // Component logic here
}
```

### Component Lifecycle Hooks

| Hook | When Called | Common Use Cases |
|------|-------------|------------------|
| **ngOnChanges** | Before ngOnInit and when @Input changes | React to input property changes |
| **ngOnInit** | Once after first ngOnChanges | Initialize component, fetch data |
| **ngDoCheck** | During every change detection | Custom change detection |
| **ngAfterContentInit** | After content projection | Access projected content |
| **ngAfterContentChecked** | After every content check | React to content changes |
| **ngAfterViewInit** | After view initialization | Access ViewChild, DOM manipulation |
| **ngAfterViewChecked** | After every view check | React to view changes |
| **ngOnDestroy** | Before component destruction | Cleanup: unsubscribe, clear timers |

### Reading Checklist
- [ ] Read Angular official docs: Introduction
- [ ] Read about Components
- [ ] Watch: "Angular in 100 Seconds" by Fireship
- [ ] Review TypeScript basics if needed

---

## 🌙 Evening Session (8:00 PM - 10:30 PM) - Hands-on Practice

### Task 1: Environment Setup (20 minutes)

```bash
# Install Node.js (if not installed)
# Download from https://nodejs.org/

# Verify Node.js installation
node --version
npm --version

# Install Angular CLI globally
npm install -g @angular/cli

# Verify Angular CLI
ng version

# Create your first Angular project
ng new my-first-app --standalone --routing --style=css

# Navigate to project
cd my-first-app

# Start development server
ng serve

# Open browser: http://localhost:4200
```

### Task 2: Basic Component (30 minutes)

Create a simple counter component:

```typescript
// counter.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-counter',
  standalone: true,
  template: `
    <div class="counter-container">
      <h2>Counter: {{ count }}</h2>
      <button (click)="increment()">+</button>
      <button (click)="decrement()">-</button>
      <button (click)="reset()">Reset</button>
    </div>
  `,
  styles: [`
    .counter-container {
      text-align: center;
      padding: 20px;
      border: 2px solid #333;
      border-radius: 8px;
      max-width: 300px;
      margin: 20px auto;
    }
    button {
      margin: 0 5px;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
      background: #007bff;
      color: white;
      border: none;
      border-radius: 4px;
    }
    button:hover {
      background: #0056b3;
    }
  `]
})
export class CounterComponent {
  count = 0;

  increment() {
    this.count++;
  }

  decrement() {
    this.count--;
  }

  reset() {
    this.count = 0;
  }
}
```

**To use this component:**
```typescript
// app.component.ts
import { Component } from '@angular/core';
import { CounterComponent } from './counter.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [CounterComponent],
  template: `<app-counter></app-counter>`
})
export class AppComponent {}
```

### Task 3: Component with All Lifecycle Hooks (45 minutes)

```typescript
// lifecycle-demo.component.ts
import { 
  Component, 
  OnInit, 
  OnChanges, 
  DoCheck, 
  AfterContentInit,
  AfterContentChecked, 
  AfterViewInit, 
  AfterViewChecked, 
  OnDestroy,
  SimpleChanges,
  Input
} from '@angular/core';

@Component({
  selector: 'app-lifecycle-demo',
  standalone: true,
  template: `
    <div class="lifecycle">
      <h3>{{ title }}</h3>
      <p>Count: {{ count }}</p>
      <p>Check the console for lifecycle hook execution order</p>
    </div>
  `,
  styles: [`
    .lifecycle {
      padding: 20px;
      border: 2px solid #28a745;
      border-radius: 8px;
      margin: 20px;
    }
  `]
})
export class LifecycleDemoComponent implements 
  OnInit, OnChanges, DoCheck, AfterContentInit, 
  AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy {
  
  @Input() title: string = 'Lifecycle Demo';
  count = 0;

  constructor() {
    console.log('1. Constructor called');
  }

  ngOnChanges(changes: SimpleChanges) {
    console.log('2. ngOnChanges called', changes);
  }

  ngOnInit() {
    console.log('3. ngOnInit called');
    // Simulate API call
    setTimeout(() => {
      this.count = 10;
      console.log('Data loaded in ngOnInit');
    }, 1000);
  }

  ngDoCheck() {
    console.log('4. ngDoCheck called');
  }

  ngAfterContentInit() {
    console.log('5. ngAfterContentInit called');
  }

  ngAfterContentChecked() {
    console.log('6. ngAfterContentChecked called');
  }

  ngAfterViewInit() {
    console.log('7. ngAfterViewInit called');
  }

  ngAfterViewChecked() {
    console.log('8. ngAfterViewChecked called');
  }

  ngOnDestroy() {
    console.log('9. ngOnDestroy called - Cleanup here!');
  }
}
```

### Task 4: Parent-Child Communication (45 minutes)

```typescript
// parent.component.ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { ChildComponent } from './child.component';

@Component({
  selector: 'app-parent',
  standalone: true,
  imports: [CommonModule, FormsModule, ChildComponent],
  template: `
    <div class="parent">
      <h2>Parent Component</h2>
      <input [(ngModel)]="parentMessage" placeholder="Message to child">
      
      <app-child 
        [messageFromParent]="parentMessage"
        (messageFromChild)="receiveMessage($event)"
      ></app-child>
      
      <p *ngIf="childMessage">Message from child: <strong>{{ childMessage }}</strong></p>
    </div>
  `,
  styles: [`
    .parent {
      padding: 20px;
      border: 2px solid #007bff;
      border-radius: 8px;
      margin: 20px;
    }
    input {
      padding: 8px;
      margin: 10px 0;
      width: 100%;
      max-width: 300px;
    }
  `]
})
export class ParentComponent {
  parentMessage = 'Hello from Parent';
  childMessage = '';

  receiveMessage(msg: string) {
    this.childMessage = msg;
    console.log('Received from child:', msg);
  }
}

// child.component.ts
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  standalone: true,
  template: `
    <div class="child">
      <h3>Child Component</h3>
      <p>From Parent: <em>{{ messageFromParent }}</em></p>
      <button (click)="sendMessage()">Send Message to Parent</button>
    </div>
  `,
  styles: [`
    .child {
      padding: 15px;
      border: 2px solid #28a745;
      border-radius: 8px;
      margin: 15px 0;
      background: #f8f9fa;
    }
    button {
      padding: 8px 16px;
      background: #28a745;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
  `]
})
export class ChildComponent {
  @Input() messageFromParent: string = '';
  @Output() messageFromChild = new EventEmitter<string>();

  sendMessage() {
    this.messageFromChild.emit('Hello from Child! Timestamp: ' + new Date().toLocaleTimeString());
  }
}
```

### Task 5: Mini Project - Enhanced Counter (30 minutes)

Build a counter with:
- Increment/Decrement/Reset buttons
- Step size selector (1, 5, 10)
- Min/Max limits (0-100)
- History of actions
- Undo last action

```typescript
// enhanced-counter.component.ts
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

interface Action {
  type: 'increment' | 'decrement' | 'reset';
  value: number;
  timestamp: Date;
}

@Component({
  selector: 'app-enhanced-counter',
  standalone: true,
  imports: [CommonModule],
  template: `
    <div class="counter-container">
      <h2>Enhanced Counter</h2>
      
      <div class="count-display">{{ count }}</div>
      
      <div class="step-selector">
        <label>Step Size:</label>
        <button 
          *ngFor="let s of steps" 
          [class.active]="step === s"
          (click)="step = s">
          {{ s }}
        </button>
      </div>
      
      <div class="controls">
        <button (click)="decrement()" [disabled]="count <= min">-{{ step }}</button>
        <button (click)="reset()">Reset</button>
        <button (click)="increment()" [disabled]="count >= max">+{{ step }}</button>
      </div>
      
      <button (click)="undo()" [disabled]="history.length === 0">
        Undo Last Action
      </button>
      
      <div class="limits">
        Min: {{ min }} | Max: {{ max }}
      </div>
      
      <div class="history">
        <h4>History</h4>
        <div *ngFor="let action of history" class="history-item">
          {{ action.type | uppercase }} by {{ action.value }} at 
          {{ action.timestamp | date:'short' }}
        </div>
      </div>
    </div>
  `,
  styles: [`
    .counter-container {
      max-width: 400px;
      margin: 20px auto;
      padding: 20px;
      border: 2px solid #333;
      border-radius: 8px;
      text-align: center;
    }
    .count-display {
      font-size: 48px;
      font-weight: bold;
      margin: 20px 0;
      color: #007bff;
    }
    .step-selector {
      margin: 15px 0;
    }
    .step-selector button {
      margin: 0 5px;
      padding: 8px 16px;
      background: #f8f9fa;
      border: 2px solid #ddd;
    }
    .step-selector button.active {
      background: #007bff;
      color: white;
      border-color: #007bff;
    }
    .controls button {
      margin: 10px 5px;
      padding: 12px 24px;
      font-size: 18px;
      cursor: pointer;
      border: none;
      border-radius: 4px;
    }
    .controls button:first-child {
      background: #dc3545;
      color: white;
    }
    .controls button:nth-child(2) {
      background: #6c757d;
      color: white;
    }
    .controls button:last-child {
      background: #28a745;
      color: white;
    }
    button:disabled {
      opacity: 0.5;
      cursor: not-allowed;
    }
    .limits {
      margin: 15px 0;
      color: #666;
    }
    .history {
      margin-top: 20px;
      text-align: left;
      max-height: 200px;
      overflow-y: auto;
    }
    .history-item {
      padding: 5px;
      border-bottom: 1px solid #eee;
      font-size: 14px;
    }
  `]
})
export class EnhancedCounterComponent {
  count = 0;
  step = 1;
  steps = [1, 5, 10];
  min = 0;
  max = 100;
  history: Action[] = [];
  previousCount = 0;

  increment() {
    if (this.count + this.step <= this.max) {
      this.previousCount = this.count;
      this.count += this.step;
      this.addHistory('increment', this.step);
    }
  }

  decrement() {
    if (this.count - this.step >= this.min) {
      this.previousCount = this.count;
      this.count -= this.step;
      this.addHistory('decrement', this.step);
    }
  }

  reset() {
    this.previousCount = this.count;
    this.count = 0;
    this.addHistory('reset', this.previousCount);
  }

  undo() {
    if (this.history.length > 0) {
      this.count = this.previousCount;
      this.history.pop();
    }
  }

  private addHistory(type: Action['type'], value: number) {
    this.history.unshift({
      type,
      value,
      timestamp: new Date()
    });
    
    // Keep only last 10 actions
    if (this.history.length > 10) {
      this.history.pop();
    }
  }
}
```

---

## 📝 Review Session (10:30 PM - 11:00 PM)

### Documentation Tasks

Create a new file: `day-01-notes.md` and document:

1. **Component Structure:**
   - What are the 3 main parts of a component?
   - What does each decorator property do?

2. **Lifecycle Hooks:**
   - Draw the lifecycle hook execution order
   - Write 1-2 use cases for each hook

3. **Parent-Child Communication:**
   - How does @Input work?
   - How does @Output work with EventEmitter?

### Tomorrow's Preparation

- [ ] Review data binding types
- [ ] Read about template syntax
- [ ] Prepare questions about directives

### Self-Assessment

Rate your understanding (1-5):
- [ ] Can you explain Angular architecture? ___/5
- [ ] Can you create a standalone component? ___/5
- [ ] Do you understand all lifecycle hooks? ___/5
- [ ] Can you implement @Input/@Output? ___/5

---

## 🎯 Interview Questions - Day 1

### Practice These 10 Questions:

**1. What is Angular?**
- **Answer:** Angular is a TypeScript-based, open-source web application framework developed by Google for building single-page applications. It provides features like two-way data binding, dependency injection, routing, and a component-based architecture.

**2. What are the key features of Angular?**
- **Answer:** 
  - Component-based architecture
  - Two-way data binding
  - Dependency Injection
  - RxJS for reactive programming
  - Powerful template syntax
  - Built-in routing
  - Forms handling
  - CLI for scaffolding

**3. Explain the Angular component lifecycle.**
- **Answer:** Components go through lifecycle phases from creation to destruction. Hooks include: ngOnChanges, ngOnInit, ngDoCheck, ngAfterContentInit, ngAfterContentChecked, ngAfterViewInit, ngAfterViewChecked, and ngOnDestroy.

**4. What's the difference between Constructor and ngOnInit?**
- **Answer:**
  - Constructor: TypeScript class constructor, called when class is instantiated, used for dependency injection
  - ngOnInit: Angular lifecycle hook, called after first ngOnChanges, used for initialization logic and API calls

**5. What is @Input and @Output?**
- **Answer:**
  - @Input: Decorator to pass data from parent to child component
  - @Output: Decorator with EventEmitter to send data from child to parent

**6. What is a decorator in Angular?**
- **Answer:** Decorators are TypeScript functions that add metadata to classes, properties, methods, or parameters. Common decorators: @Component, @Injectable, @Input, @Output, @ViewChild, @NgModule.

**7. What is the purpose of ngOnDestroy?**
- **Answer:** ngOnDestroy is called just before component destruction. It's used for cleanup: unsubscribing from observables, detaching event handlers, clearing timers to prevent memory leaks.

**8. How do you share data between components?**
- **Answer:** Multiple ways:
  - @Input/@Output for parent-child
  - Services with shared state
  - RxJS Subjects
  - ViewChild/ContentChild
  - Route parameters
  - State management (NgRx)

**9. What is component metadata?**
- **Answer:** Metadata in @Component decorator defines:
  - selector: How to use component in HTML
  - template/templateUrl: Component's HTML
  - styles/styleUrls: Component's CSS
  - standalone: If component is standalone
  - imports: Dependencies

**10. Explain ngOnChanges hook.**
- **Answer:** ngOnChanges is called when @Input properties change. It receives a SimpleChanges object with previous and current values. Called before ngOnInit and whenever input-bound properties change.

---

## ✅ Day 1 Checklist

- [ ] Set up Angular development environment
- [ ] Created first Angular application
- [ ] Built basic counter component
- [ ] Implemented all lifecycle hooks
- [ ] Created parent-child communication
- [ ] Built enhanced counter project
- [ ] Documented all learnings
- [ ] Practiced 10 interview questions

---

## 🎉 Congratulations!

You've completed Day 1! You now understand:
- Angular architecture
- Component structure
- Lifecycle hooks
- Component communication

**Tomorrow:** Templates & Data Binding

---

[← Back to README](../README.md) | [Next: Day 2 →](day-02-templates-binding.md)