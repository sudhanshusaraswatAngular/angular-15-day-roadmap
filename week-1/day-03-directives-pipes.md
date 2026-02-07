# Day 3: Custom Directives & Pipes

## 🎯 Learning Objectives

By the end of today, you will:
- Understand directive types (Component, Attribute, Structural)
- Create custom attribute directives
- Use HostListener and HostBinding
- Master built-in pipes
- Create custom pipes
- Understand pure vs impure pipes

---

## 🌅 Morning Session (7:00 AM - 8:00 AM) - Theory

### Types of Directives

Angular has three types of directives:

1. **Component Directives** - Directives with templates (@Component)
2. **Attribute Directives** - Change appearance or behavior (ngClass, ngStyle)
3. **Structural Directives** - Change DOM structure (*ngIf, *ngFor)

### Creating Custom Attribute Directives

```typescript
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]',
  standalone: true
})
export class HighlightDirective {
  @Input() appHighlight = 'yellow';

  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.appHighlight);
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight('');
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

### Built-in Pipes

| Pipe | Purpose | Example |
|------|---------|---------|
| DatePipe | Format dates | `{{ date | date:'short' }}` |
| UpperCasePipe | Uppercase text | `{{ name | uppercase }}` |
| LowerCasePipe | Lowercase text | `{{ name | lowercase }}` |
| CurrencyPipe | Format currency | `{{ price | currency:'USD' }}` |
| DecimalPipe | Format numbers | `{{ num | number:'1.2-2' }}` |
| PercentPipe | Format percentages | `{{ val | percent }}` |
| JsonPipe | Convert to JSON | `{{ obj | json }}` |
| AsyncPipe | Unwrap observables | `{{ obs$ | async }}` |

### Creating Custom Pipes

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'truncate',
  standalone: true
})
export class TruncatePipe implements PipeTransform {
  transform(value: string, limit: number = 50): string {
    if (!value) return '';
    return value.length > limit 
      ? value.substring(0, limit) + '...' 
      : value;
  }
}
```

### Pure vs Impure Pipes

**Pure Pipes (default):**
- Execute only when input reference changes
- Better performance
- Most pipes should be pure

**Impure Pipes:**
- Execute on every change detection cycle
- Set `pure: false` in @Pipe decorator
- Use sparingly - can impact performance

```typescript
@Pipe({
  name: 'filter',
  pure: false // Impure pipe
})
export class FilterPipe implements PipeTransform {
  transform(items: any[], searchText: string): any[] {
    if (!items || !searchText) return items;
    return items.filter(item => 
      item.name.toLowerCase().includes(searchText.toLowerCase())
    );
  }
}
```

---

## 🌙 Evening Session (8:00 PM - 10:30 PM) - Hands-on Practice

### Task 1: Custom Attribute Directives (60 minutes)

Create the following directives:

1. **Highlight Directive** - Changes background color on hover
2. **Tooltip Directive** - Shows tooltip on hover
3. **AutoFocus Directive** - Automatically focuses input
4. **Click Outside Directive** - Detects clicks outside element

### Task 2: Custom Pipes (60 minutes)

Create the following pipes:

1. **Truncate Pipe** - Truncates text to specified length
2. **Filter Pipe** - Filters array based on search term
3. **Time Ago Pipe** - Converts date to "2 hours ago" format
4. **File Size Pipe** - Converts bytes to KB/MB/GB
5. **Phone Number Pipe** - Formats phone numbers

### Task 3: Mini Project - Product List (60 minutes)

Build a product list with:
- Custom highlight directive on product cards
- Tooltip directive showing product details
- Truncate pipe for long descriptions
- File size pipe for product images
- Filter pipe for searching products
- Currency and date pipes for price and dates

---

## 📝 Review Session (10:30 PM - 11:00 PM)

### Today's Achievements
- [ ] Created custom attribute directives
- [ ] Built multiple custom pipes
- [ ] Understood pure vs impure pipes
- [ ] Built product list project

### Study Notes
Document:
- Directive creation steps
- HostListener vs HostBinding
- Pipe transformation patterns
- Performance considerations

---

## 🎯 Interview Questions - Day 3

**1. What are the types of directives in Angular?**
- Component Directives: Have templates
- Attribute Directives: Change appearance/behavior
- Structural Directives: Change DOM structure

**2. How do you create a custom directive?**
- Use @Directive decorator
- Inject ElementRef or Renderer2
- Use @HostListener for events
- Use @HostBinding for properties

**3. Difference between ElementRef and Renderer2?**
- ElementRef: Direct DOM access (not safe for SSR)
- Renderer2: Abstract layer (safer, works with SSR)

**4. What is @HostListener?**
- Decorator to listen to events on host element
- Example: `@HostListener('click')` for clicks

**5. What are pipes in Angular?**
- Transform data in templates
- Built-in: date, currency, uppercase
- Can create custom pipes

**6. Pure vs Impure pipes?**
- Pure: Executes when input reference changes (better performance)
- Impure: Executes every change detection (use sparingly)

**7. When to use impure pipes?**
- When detecting changes inside objects/arrays
- Example: filtering array with modified items

**8. How to create custom pipe?**
- Use @Pipe decorator
- Implement PipeTransform interface
- Define transform method

**9. What is AsyncPipe?**
- Subscribes to observables/promises
- Returns latest emitted value
- Auto-unsubscribes on destroy

**10. Can you chain pipes?**
- Yes: `{{ value | pipe1 | pipe2 | pipe3 }}`
- Each pipe transforms previous output

---

## ✅ Day 3 Checklist

- [ ] Created 4+ custom directives
- [ ] Built 5+ custom pipes
- [ ] Understood pure vs impure
- [ ] Completed product list project
- [ ] Practiced 10 interview questions

---

**Tomorrow:** Services & Dependency Injection

[← Day 2](day-02-templates-binding.md) | [README](../README.md) | [Day 4 →](day-04-services-di.md)