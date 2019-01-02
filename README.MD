# Original project : sabyasachibiswal https://github.com/sabyasachibiswal/angular5-social-login


Social login api for Angular 6. Includes Facebook, Google login and Linkedin. 

![Generic badge](https://img.shields.io/badge/build-passing-green.svg)
![Generic badge](https://img.shields.io/badge/Angular-6-green.svg)
[AOT](https://angular.io/guide/aot-compiler) Compatible.


## Getting started


### Install via npm 

```sh
npm install --save angular-6-social-login
```

### Import the module

In `app.module.ts`,

```javascript
...

import {
    SocialLoginModule,
    AuthServiceConfig,
    GoogleLoginProvider,
    FacebookLoginProvider,
    LinkedinLoginProvider,
} from "angular-6-social-login";


// Configs 
export function getAuthServiceConfigs() {
  let config = new AuthServiceConfig(
      [
        {
          id: FacebookLoginProvider.PROVIDER_ID,
	      provider: new FacebookLoginProvider("Your-Facebook-app-id")
        },
        {
          id: GoogleLoginProvider.PROVIDER_ID,
	      provider: new GoogleLoginProvider("Your-Google-Client-Id")
        },
          {
            id: LinkedinLoginProvider.PROVIDER_ID,
            provider: new LinkedinLoginProvider("1098828800522-m2ig6bieilc3tpqvmlcpdvrpvn86q4ks.apps.googleusercontent.com")
          },
      ];
  );
  return config;
}

@NgModule({
  imports: [
    ...
    SocialLoginModule
  ],
  providers: [
    ...
    {
      provide: AuthServiceConfig,
      useFactory: getAuthServiceConfigs
    }
  ],
  bootstrap: [...]
})

export class AppModule { }

```

### Usage : 

In `signin.component.ts`,

```javascript
import {Component, OnInit} from '@angular/core';
import {
    AuthService,
    FacebookLoginProvider,
    GoogleLoginProvider
} from 'angular-6-social-login';

@Component({
  selector: 'app-signin',
  templateUrl: './signin.component.html',
  styleUrls: ['./signin.component.css']
})


export class SigninComponent implements OnInit {

  constructor( private socialAuthService: AuthService ) {}
  
  public socialSignIn(socialPlatform : string) {
    let socialPlatformProvider;
    if(socialPlatform == "facebook"){
      socialPlatformProvider = FacebookLoginProvider.PROVIDER_ID;
    }else if(socialPlatform == "google"){
      socialPlatformProvider = GoogleLoginProvider.PROVIDER_ID;
    } else if (socialPlatform == "linkedin") {
      socialPlatformProvider = LinkedinLoginProvider.PROVIDER_ID;
    }
    
    this.socialAuthService.signIn(socialPlatformProvider).then(
      (userData) => {
        console.log(socialPlatform+" sign in data : " , userData);
        // Now sign-in with userData
        // ...
            
      }
    );
  }
  
}
```
In `the profile component,the page After login you give your user the access profile.component.ts`,
```javascript
public unlinkaccount(socialPlatform : string) {
    let socialPlatformProvider;
    if(socialPlatform === "facebook"){
      socialPlatformProvider = FacebookLoginProvider.PROVIDER_ID;
      socialPlatformProvider = GoogleLoginProvider.PROVIDER_ID;
      this.socialAuthService.revokeAccess(this.userid).then(
        (revokeres) => {
                console.log(revokeres);
                //the url to redirect after revocation
                this.router.navigate(['/login',{unlinkmessage:'You have successfully unlinked your account'}]);
        }
        );
    }else if(socialPlatform === "google"){
      socialPlatformProvider = GoogleLoginProvider.PROVIDER_ID;
      this.socialAuthService.revokeAccess(this.userid).then(
        (revokeres) => {
                console.log(revokeres);
                //the url to redirect after revocation
                this.router.navigate(['/login',{unlinkmessage:revokeres}]);
        }
        );
    }
  }
```
In `profile.component.html`
```html
<h1>
Unlink {{provider}} Account
</h1>

 <button (click)="unlinkaccount(provider)" *ngIf="provider" color="warn">Unlink {{provider}} account</button>
 
```
In `signin.component.html`,

```html
<h1>
     Sign in
</h1>

<button (click)="socialSignIn('facebook')">Sign in with Facebook</button>
<button (click)="socialSignIn('google')">Sign in with Google</button>              
```



### Facebook App Id : 

You need to create your own app by going to [Facebook Developers](https://developers.facebook.com/) page.
Add `Facebook login` under products and configure `Valid OAuth redirect URIs`.

### Google Client Id : 

Follow this official documentation on how to [
Create a Google API Console project and client ID](https://developers.google.com/identity/sign-in/web/devconsole-project).

