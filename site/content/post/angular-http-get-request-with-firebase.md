---
title: Angular HTTP GET Request with Firebase
date: 2017-08-01T12:11:56+05:30
description: Angular HTTP GET Request with Firebase
---
Hi Folks, In Previous Post we have seen how to send http post request to Firebase.

Today we will see how to send HTTP GET request to firebase.

First of all open http.service.ts and replace it with the code below:

```
import { Injectable } from '@angular/core';
```

```
import { Http, Response, Headers } from '@angular/http';
```

```
import { Observable } from "rxjs/Observable";
```

```
import 'rxjs/Rx';
```

```
@Injectable()
```

```
export class HttpService {
```

```
  constructor(public http: Http) { }
```

```
  postFunc(postData:any){
```

```
  		const body = JSON.stringify(postData);
```

```
  		const headers = new Headers();
```

```
  		headers.append("Content-Type", "application/json");
```

```
  	return this.http.post("https://your-project-ID.firebaseio.com/data.json", body, {headers: headers}).map((response:Response) => response.json());
```

```
  }
```

```
  getFunc(){
```

```
  	return this.http.get("https://your-project-ID.firebaseio.com/data.json").map((res: Response)=> res.json());
```

```
  }
```

```
}
```

Component

Now we are going to create component that will display the data.

open cmd and run

```
ng g c Components/allData
```

Now open all-data.component.ts and replace it with following code

```
import { Component, OnInit } from '@angular/core';
```

```
import { HttpService } from '../../Services/http.service';
```

```
import { Response, Http } from '@angular/http';
```

```
@Component({
```

```
  selector: 'app-all-data',
```

```
  templateUrl: './all-data.component.html',
```

```
  styleUrls: ['./all-data.component.css']
```

```
})
```

```
export class AllDataComponent implements OnInit {
```

```
items:any = [];
```

```
  constructor(public httpService:HttpService) { }
```

```
  ngOnInit() {
```

```
  		this.httpService.getFunc().subscribe(
```

```
  				data => {
```

```
  			const myArr = [];
```

```
  			const itemKey = [];
```

```
  			for(let key in data){
```

```
  				myArr.push(data[key]);
```

```
  				itemKey.push(key);
```

```
  			}
```

```
  			this.items = myArr;
```

```
  		}
```

```
  			)
```

```
  }
```

```
}
```

Open all-data.component.html and replace it with the following

```
<div class="list-group">
```

```
 <h3 class="text-center">Angular Http GET Request</h3>
```

```
 <a *ngFor="let item of items" class="list-group-item list-group-item-action flex-column align-items-start">
```

```
 <div class="d-flex w-100 justify-content-between">
```

```
 <h5 class="mb-1">{{item.title}}</h5>
```

```
 <small class="text-muted">Latest</small>
```

```
 </div>
```

```
 <p class="mb-1">{{item.content}}</p>
```

```
 <small class="text-muted">{{item.content}}.</small>
```

```
 </a>
```

```
 <a routerLink='/sendData' class="btn btn-primary">Back</a>
```

```
</div>
```

Guys, In previous post we did not make routes because we have only one view but now we have to create routes because we have multiple views.

One view will display the form that will use to send post request and the another one will  display data.

so now open app.module.ts and replace it with the following code

```
import { BrowserModule } from '@angular/platform-browser';
```

```
import { NgModule } from '@angular/core';
```

```
import { FormsModule } from '@angular/forms';
```

```
import { HttpModule } from '@angular/http';
```

```
import { AppComponent } from './app.component';
```

```
import { FormComponent } from './Components/form/form.component';
```

```
import { AllDataComponent } from './Components/all-data/all-data.component';
```

```
import { HttpService } from './Services/http.service';
```

```
import { Routes, RouterModule } from '@angular/router';
```

```
const appRouter:Routes=[
```

```
 { path:"allData", component:AllDataComponent },
```

```
 { path:"sendData", component:FormComponent},
```

```
 { path: '', redirectTo: '/sendData', pathMatch: 'full' }
```

```
]
```

```
@NgModule({
```

```
  declarations: [
```

```
\    AppComponent,
```

```
\    FormComponent,
```

```
\    AllDataComponent
```

```
  ],
```

```
  imports: [
```

```
\    BrowserModule,
```

```
\    FormsModule,
```

```
\    HttpModule,
```

```
\    RouterModule.forRoot(appRouter)
```

```
  ],
```

```
  providers: [HttpService],
```

```
  bootstrap: [AppComponent]
```

```
})
```

```
export class AppModule { }
```

So app is ready now. :)

Run command

ng serve

open url http://localhost:4200

After submit your data open http://localhost:4200/allData

and see your submitted data

You can download source from my GitHub Repository

```
https://github.com/gurmandeep/angular2-http
```

Thanks.
