# NgxMatErrors

[![npm version](https://img.shields.io/npm/v/ngx-mat-errors.svg?style=flat-square)](https://www.npmjs.com/package/ngx-mat-errors)
[![npm downloads total](https://img.shields.io/npm/dt/ngx-mat-errors.svg?style=flat-square)](https://www.npmjs.com/package/ngx-mat-errors)
[![npm downloads monthly](https://img.shields.io/npm/dm/ngx-mat-errors.svg?style=flat-square)](https://www.npmjs.com/package/ngx-mat-errors)

## What does it do?

NgxMatErrors provides an easy, yet flexible solution for displaying error messages in a MatFormField.

## Try it

See it in action on [StackBlitz](https://stackblitz.com/edit/ngx-mat-errors-angular-16?file=src%2Fapp%2Fapp.component.html)

## How to use it?

Install `ngx-mat-errors` in your project:

```
npm install ngx-mat-errors
```

Import `NgxMatErrorsModule` and provide `NGX_MAT_ERROR_CONFIG_EN` (or your custom error messages) in your `app.module.ts`:
You can import only `NgxMatErrors` and `NgxMatErrorDef` as they are marked standalone.

```typescript
import { NgxMatErrorsModule, NGX_MAT_ERROR_CONFIG_EN } from "ngx-mat-errors";

@NgModule({
  imports: [...NgxMatErrorsModule],
  provide: [NGX_MAT_ERROR_CONFIG_EN],
})
export class AppModule {}
```

Add `ngx-mat-errors` to your `mat-error` in your `mat-form-field`.

```html
<mat-form-field>
  <mat-label>Label</mat-label>
  <input type="text" matInput [formControl]="control" />
  <mat-error ngx-mat-errors></mat-error>
</mat-form-field>
```

### Outside a `MatFormField` or override the control

`ngx-mat-errors` can be used as an `@Input()` to override the `MatFormFieldControl`.

```html
<mat-form-field>
  <mat-label>Label</mat-label>
  <input
    type="text"
    matInput
    #input="matInput"
    [formControl]="control1"
    autocomplete="off"
  />
</mat-form-field>
<mat-error [ngx-mat-errors]="input"></mat-error>
```

## Customize

There are 2 ways to customize your error messages.

### Injection token

There is the `NGX_MAT_ERROR_DEFAULT_OPTIONS` injection token, you can provide it in your `app.module.ts` with `useClass`, or `useFactory` and customize your error messages globally.

This example changes only the `min` error message.

```typescript
import {
  errorMessagesEnFactory,
  NGX_MAT_ERROR_DEFAULT_OPTIONS
} from 'ngx-mat-errors';
import { FactoryProvider, LOCALE_ID } from '@angular/core';

export const NGX_MAT_ERROR_DEFAULT_CONFIG: FactoryProvider = {
  useFactory: (locale: string) => {
    return {
      ...errorMessagesEnFactory(locale),
      min: (error: MinError) =>
        `Min value is ${error.min}, actual is ${error.actual}`,
    };
  },
  provide: NGX_MAT_ERROR_DEFAULT_OPTIONS,
  deps: [LOCALE_ID],
};

@NgModule({
  ...
  providers: [NGX_MAT_ERROR_DEFAULT_CONFIG],
})
export class AppModule {}
```

### \*ngxMatErrorDef

You can customize your error messages even more with `*ngxMatErrorDef` directive.

```html
<mat-form-field>
  <mat-label>Label</mat-label>
  <input type="text" matInput [formControl]="control1" />
  <mat-error ngx-mat-errors>
    <span *ngxMatErrorDef="let error; for: 'pattern'">
      Only digits are allowed, up to 12 digits.
    </span>
    <ng-container *ngxMatErrorDef="let error; for: 'min'">
      The minimum value is {{ error.min }}.
    </ng-container>
  </mat-error>
</mat-form-field>
```

## Compatibility

- `@angular/core`: `^16.0.0`,
- `@angular/material`: `^16.0.0`,

### Reactve forms

```html
<mat-form-field>
  <mat-label>Label</mat-label>
  <input type="text" matInput [formControl]="control" />
  <mat-error ngx-mat-errors></mat-error>
</mat-form-field>
```

### Template-driven forms

```html
<mat-form-field>
  <mat-label>Label</mat-label>
  <input type="text" matInput [(ngModel)]="value" />
  <mat-error ngx-mat-errors></mat-error>
</mat-form-field>
```

## Development

### Library Build / NPM Package

Run `npm run develop` to build the library and generate an NPM package.
The build artifacts will be stored in the `dist/ngx-mat-errors` folder.

### Development server

Run `npm start` for a dev server. Navigate to `http://localhost:4202/`. The app will automatically reload if you change any of the source files.
