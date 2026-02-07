# Day 6: Advanced Routing & Guards (Weekend)

## 🎯 Learning Objectives
- Master route guards (CanActivate, CanDeactivate, Resolve)
- Implement lazy loading
- Create preloading strategies
- Build protected routes

## 🌅 Theory - Route Guards

### Guard Types
```typescript
// CanActivate - Prevent unauthorized access
export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  if (authService.isLoggedIn()) {
    return true;
  }
  return inject(Router).createUrlTree(['/login']);
};

// CanDeactivate - Warn before leaving
export const unsavedChangesGuard: CanDeactivateFn<ComponentType> = (component) => {
  if (component.hasUnsavedChanges()) {
    return confirm('You have unsaved changes. Leave anyway?');
  }
  return true;
};

// Resolve - Prefetch data
export const userResolver: ResolveFn<User> = (route) => {
  return inject(UserService).getUser(route.params['id']);
};
```

### Lazy Loading
```typescript
const routes: Routes = [
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.routes').then(m => m.ADMIN_ROUTES),
    canActivate: [authGuard]
  }
];
```

### Preloading Strategies
```typescript
import { PreloadAllModules } from '@angular/router';

bootstrapApplication(AppComponent, {
  providers: [
    provideRouter(routes, withPreloading(PreloadAllModules))
  ]
});
```

## 🚀 Practice Tasks (8 hours)

### Session 1 (10 AM - 1 PM)
- Create all guard types
- Implement authentication guard
- Build unsaved changes guard
- Create data resolver

### Session 2 (2 PM - 5 PM)
- Set up lazy loading
- Implement custom preloading
- Build admin section with guards
- Complete multi-page protected app

### Interview Practice (5 PM - 6 PM)
- Practice 20 routing questions
- Code guards from memory

## 🎯 Interview Questions

**1. What are route guards?**
- Functions that control navigation
- Can prevent/allow route activation
- Used for authentication, validation

**2. Types of guards?**
- CanActivate: Before entering route
- CanDeactivate: Before leaving route
- Resolve: Prefetch data
- CanLoad: Lazy loading control
- CanActivateChild: Child routes

**3. What is lazy loading?**
- Load modules on-demand
- Reduces initial bundle size
- Uses loadChildren property
- Improves performance

**4. CanActivate vs CanLoad?**
- CanActivate: Module already loaded
- CanLoad: Before loading module
- CanLoad better for security

**5. What is Resolve guard?**
- Prefetches data before navigation
- Component receives data immediately
- Prevents showing loading states

**6. Preloading strategies?**
- NoPreloading: Load only on demand
- PreloadAllModules: Preload all lazy modules
- Custom: Selective preloading

**7. How to create custom preloading?**
- Implement PreloadingStrategy interface
- Define preload method
- Return Observable<any> or null

**8. Route guard return types?**
- boolean: Allow/deny
- UrlTree: Redirect
- Observable/Promise: Async decisions

**9. Multiple guards on one route?**
- Yes, executed in order
- All must return true
- First false/UrlTree stops execution

**10. Guard execution order?**
- CanLoad → CanActivate → CanActivateChild → Resolve → Component

## ✅ Checklist
- [ ] Created all guard types
- [ ] Implemented lazy loading
- [ ] Built preloading strategy
- [ ] Completed protected app
- [ ] Practiced 20 questions

[← Day 5](day-05-routing-basics.md) | [README](../README.md) | [Day 7 →](day-07-forms.md)