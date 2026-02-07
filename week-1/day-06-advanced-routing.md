# Day 6: Advanced Routing & Guards (Weekend)

## 🎯 Learning Objectives
- Master all route guard types
- Implement authentication guards
- Handle unsaved changes
- Prefetch data with Resolve guards
- Configure lazy loading
- Custom preloading strategies

## 🌅 Morning Session - Theory

### Route Guards Types

| Guard | Interface | Purpose | Return Type |
|-------|-----------|---------|-------------|
| CanActivate | CanActivateFn | Can route be activated? | boolean/UrlTree |
| CanDeactivate | CanDeactivateFn | Can leave current route? | boolean |
| Resolve | ResolveFn | Prefetch data before navigation | any |
| CanLoad | CanLoadFn | Can lazy-load module? | boolean |
| CanActivateChild | CanActivateChildFn | Can activate child routes? | boolean |

### CanActivate Guard (Authentication)
```typescript
import { inject } from '@angular/core';
import { Router } from '@angular/router';

export const authGuard: CanActivateFn = (route, state) => {
  const router = inject(Router);
  const authService = inject(AuthService);
  
  if (authService.isLoggedIn()) {
    return true;
  }
  
  return router.createUrlTree(['/login']);
};

// In routes
{ path: 'admin', component: AdminComponent, canActivate: [authGuard] }
```

### CanDeactivate Guard (Unsaved Changes)
```typescript
export const unsavedChangesGuard: CanDeactivateFn<any> = (component) => {
  if (component.hasUnsavedChanges && component.hasUnsavedChanges()) {
    return confirm('You have unsaved changes. Do you want to leave?');
  }
  return true;
};
```

### Resolve Guard (Data Prefetching)
```typescript
export const userResolver: ResolveFn<User> = (route) => {
  const userService = inject(UserService);
  const id = route.paramMap.get('id')!;
  return userService.getUserById(id);
};

// In routes
{ path: 'user/:id', component: UserComponent, resolve: { user: userResolver } }

// In component
ngOnInit() {
  this.route.data.subscribe(data => {
    this.user = data['user'];
  });
}
```

### Lazy Loading
```typescript
const routes: Routes = [
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.routes').then(m => m.ADMIN_ROUTES)
  }
];
```

### Preloading Strategies
```typescript
import { PreloadAllModules } from '@angular/router';

// Preload all lazy modules
bootstrapApplication(AppComponent, {
  providers: [
    provideRouter(routes, withPreloading(PreloadAllModules))
  ]
});
```

## 🌙 Evening Session - Practice Tasks

### Task 1: Authentication Guard (60 min)
- Create AuthService with login/logout
- Implement CanActivate guard
- Redirect to login if not authenticated
- Store auth state in localStorage

### Task 2: Unsaved Changes Guard (45 min)
- Create form component
- Implement CanDeactivate guard
- Show confirmation dialog
- Handle browser back button

### Task 3: Resolve Guard (45 min)
- Create resolver for user data
- Implement loading indicator
- Handle errors in resolver
- Cache resolved data

### Task 4: Lazy Loading (60 min)
- Split app into feature modules
- Configure lazy loading
- Test bundle sizes
- Implement preloading strategy

### Task 5: Complete Admin Project (90 min)
Build protected admin section with:
- Login page
- Authentication guard
- Role-based access
- Lazy-loaded admin module
- Unsaved changes protection

## 🎯 Interview Questions

**1. What are route guards?**
- Functions that control navigation
- Return boolean or UrlTree
- Used for authentication, authorization, data loading

**2. Types of guards?**
- CanActivate: Can access route?
- CanDeactivate: Can leave route?
- Resolve: Prefetch data
- CanLoad: Can load lazy module?
- CanActivateChild: Can access child routes?

**3. CanActivate vs CanLoad?**
- CanActivate: Checks after module loads
- CanLoad: Prevents module loading entirely
- CanLoad better for large lazy modules

**4. How to pass data to guards?**
- Route data property
- ActivatedRouteSnapshot
- Service injection

**5. What is Resolve guard?**
- Prefetches data before navigation
- Component receives data via route.data
- Delays navigation until data ready

**6. How to implement role-based routing?**
- Store user roles in AuthService
- Check roles in CanActivate guard
- Return false or redirect if unauthorized

**7. What is lazy loading?**
- Load modules on-demand
- Reduces initial bundle size
- Use loadChildren property
- Improves app performance

**8. Preloading strategies?**
- NoPreloading: Default, no preloading
- PreloadAllModules: Preload all after initial load
- Custom: Preload based on conditions

**9. How guards affect performance?**
- Resolve guards delay navigation
- CanLoad prevents unnecessary downloads
- Multiple guards execute in order
- Heavy logic should be async

**10. Can multiple guards be applied?**
- Yes, array of guards
- Execute in order defined
- All must return true to proceed
- First rejection stops navigation

## ✅ Checklist
- [ ] Implemented all guard types
- [ ] Created authentication system
- [ ] Built unsaved changes protection
- [ ] Configured lazy loading
- [ ] Custom preloading strategy
- [ ] Completed admin project

**Tomorrow:** Forms (Template-driven & Reactive)

[← Day 5](day-05-routing-basics.md) | [README](../README.md) | [Day 7 →](day-07-forms.md)