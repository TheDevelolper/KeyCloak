# NgFrontend

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 14.2.9.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

--- 

## Installing Keycloak 

Install keycloak angular and keycloak-js:

``` terminal 
$ npm install keycloak-angular keycloak-js
```
Keycloak needs to be initialised before it can be used. This is achieved using an app initializer in the angular app module:


``` typescript
import { APP_INITIALIZER, NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { KeycloakAngularModule, KeycloakService } from 'keycloak-angular';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { WelcomeComponent } from './member/welcome/welcome.component';


function initializeKeycloak(keycloak: KeycloakService): () => Promise<boolean> {
  return (): Promise<boolean> =>
    keycloak.init({
      config: {
        url: 'http://localhost:8080/auth',
        realm: 'momentum',
        clientId: 'ng-frontend'
      },
      initOptions: {
        // onLoad: 'check-sso',
        onLoad: 'login-required',
        flow: 'standard',
        // silentCheckSsoRedirectUri:
        //   window.location.origin + '/assets/silent-check-sso.html'
      }
    });
}

@NgModule({
  declarations: [
    AppComponent,
    WelcomeComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule, 
    KeycloakAngularModule
  ],
  providers: [
    {
      provide: APP_INITIALIZER,
      useFactory: initializeKeycloak,
      multi: true,
      deps: [KeycloakService]
    }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
Create a Realm and client in keycloak. In my example I created a realm called "momentum" and a client called "ng-frontend".

Be sure to setup:

A home URL e.g.:
```
/realms/master/signin/
```

Valid redirect uris e.g.:
```
http://localhost:4200/
http://127.0.0.1:4200/
```

Web origins e.g.:
```
http://localhost:4200
http://127.0.0.1:4200
```
