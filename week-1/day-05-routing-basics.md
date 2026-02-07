# Day 5: Routing Basics

## 🎯 Learning Objectives
- Understand Angular Router
- Configure routes
- Navigate between components
- Use route parameters and query parameters
- Implement nested/child routes

## 🌅 Morning Session - Theory

### Route Configuration
```typescript
import { Routes } from '@angular/router';

export const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'user/:id', component: UserDetailComponent },
  { path: '**', component: NotFoundComponent }
];
```

### Navigation Methods
```typescript
// Template navigation
<a routerLink="/home">Home</a>
<a [routerLink]="['/user', userId]">User</a>

// Programmatic navigation
constructor(private router: Router) { }

navigate() {
  this.router.navigate(['/products']);
  // With query params
  this.router.navigate(['/search'], { queryParams: { q: 'angular' } });
}
```

### Getting Route Parameters
```typescript
import { ActivatedRoute } from '@angular/router';

constructor(private route: ActivatedRoute) { }

ngOnInit() {
  // Route params
  this.route.params.subscribe(params => {
    const id = params['id'];
  });

  // Query params
  this.route.queryParams.subscribe(params => {
    const search = params['q'];
  });
}
```

## 🌙 Evening Session - Practice Tasks

### Task 1: Basic Routing (30 min)
- Set up routing for 5+ pages
- Create navigation menu
- Add active link styling

### Task 2: Route Parameters (30 min)
- Dynamic routes with :id
- Get params in component
- Navigate with parameters

### Task 3: Query Parameters (30 min)
- Add search functionality
- Read query params
- Update URL with query params

### Task 4: Nested Routes (30 min)
- Create parent-child route structure
- Use router-outlet in child
- Navigate between nested routes

### Task 5: Blog App Project (60 min)
Build a blog with:
- Posts list page
- Post detail page with route params
- Categories with nested routes
- Search with query params

## 🎯 Interview Questions

**1. What is Angular Router?**
- Service for navigation between views
- Interprets browser URL as navigation instruction
- Can pass data through routes

**2. What is RouterModule?**
- Module that provides routing functionality
- Configured with routes array
- Provides RouterLink, RouterOutlet directives

**3. What is router-outlet?**
- Placeholder directive for routed components
- Displays component for active route
- Can have multiple named outlets

**4. Difference between routerLink and href?**
- routerLink: Angular navigation, no page reload
- href: Traditional navigation, page reloads

**5. How to pass data through routes?**
- Route parameters: /user/:id
- Query parameters: /search?q=angular
- Route data property
- State object

**6. What is ActivatedRoute?**
- Service containing route information
- Access params, queryParams, data
- Subscribe to route changes

**7. What is pathMatch?**
- 'full': Match entire URL path
- 'prefix': Match URL prefix (default)
- Important for redirect routes

**8. Wildcard route (**)?**
- Catches all unmatched routes
- Usually for 404 page
- Must be last in routes array

**9. Child/Nested routes?**
- Routes within routes
- Use children property
- Requires router-outlet in parent

**10. How to navigate programmatically?**
- Inject Router service
- Use router.navigate() or router.navigateByUrl()
- Can pass extras like queryParams

## ✅ Checklist
- [ ] Configured basic routing
- [ ] Implemented navigation
- [ ] Used route parameters
- [ ] Added query parameters
- [ ] Created nested routes
- [ ] Built blog app project
- [ ] Practiced interview questions

**Tomorrow:** Advanced Routing & Guards (Weekend Deep Dive!)

[← Day 4](day-04-services-di.md) | [README](../README.md) | [Day 6 →](day-06-advanced-routing.md)