---
title: Angular HTTP POST Request with Firebase
date: 2017-02-27T11:27:33.000Z
description: Angular HTTP POST Request with Firebase
image: /img/an2-1.jpg
---
Hi Folks, We are going to create a simple application that used to send HTTP POST request to Firebase.

First of all open cmd and install angular-cli using command

```
npm install angular-cli
```

After installation of angular-cli, create your angular2 project using following Command

```
ng new your-app-name
```

Now open cmd into your project directory and run command

```
ng serve
```

Now open following url.

```
http://localhost:4200
```

and you will see the front page of your Application

Now open app folder and create Components folder.

![App works](/img/2017-01-30_21-40-53.png)

Run Following command to create component

 ng g c Components/form
 Here g is generate , c is component, Components is folder name and form is component name

Now copy selector from form.component.ts and paste it into app.components.html

![](/img/2017-02-26_00-04-25-1.png)

![](/img/2017-02-05_14-23-42.png)

Open form.component.html and replace with following

```
	<div class="offset-md-3 col-6">
		<h3>Post Request with Angular 2</h3>
		<fieldset>	
			<form action="">
				<input type="text" name="title" class="form-control" [(ngModel)]="title"> <br>
				<input type="text" name="content" class="form-control" [(ngModel)]="content"><br>
				<input type="submit" class="btn btn-success" (click) = "getFormData()">
			</form>
		</fieldset>	
	</div>
```

so now I am going to include Twitter Bootstrap.

Install bootstrap by following command

```
npm install bootstrap@4.0.0-alpha-6
```

If you wanna include bootstrap 3 then run following command

```
npm install bootstrap@3
```

Now open angular-cli.json and inlcude bootstrap path into styles.

![](/img/2017-01-30_22-35-24.png)

Restart your app
Now form is ready :)

SETUP Firebase

Open firebase.google.com then go to console and create project

![](/img/2017-02-01_21-09-42.png)

![](/img/2017-02-04_12-50-33.png)

After creating project click on it and then click on database.
we have to change rules to write and read the data.

Service
Create Services folder within app folder
open cmd and run following command

```
ng g s Services/http
```

now open app.module.ts and import service

![](/img/2017-02-04_13-11-26.png)

After importing service open http.service.ts

and update it with following code.

```
import { Injectable } from '@angular/core';
import { Http, Response, Headers } from '@angular/http';
import { Observable } from "rxjs/Observable";
import 'rxjs/Rx';
@Injectable()
export class HttpService {

  constructor(public http: Http) { }

  postFunc(postData:any){
  		const body = JSON.stringify(postData);
  		const headers = new Headers();
  		headers.append("Content-Type", "application/json");
  	
  	return this.http.post("https://your-project-ID.firebaseio.com/data.json", body, {headers: headers}).map((response:Response) => response.json());
  }
}
```

Now open Firebase and click on Database and copy endpoint and replace it into http.service.ts

![](/img/2017-02-04_23-33-13-copy.png)

Now open form.component.ts and replace with the following

```
import { Component, OnInit } from '@angular/core';
import { HttpService } from '../../Services/http.service';
import { Http, Response, Headers } from '@angular/http';

@Component({
  selector: 'app-form',
  templateUrl: './form.component.html',
  styleUrls: ['./form.component.css']
})
export class FormComponent implements OnInit {
title:any;
content:any;
  constructor(public httpService: HttpService) { }

  ngOnInit() {
  	
  }

  getFormData(){
  	this.httpService.postFunc({title: this.title, content:this.content}).subscribe(
  			(res: Response) => console.log(res));
           this.title = this.content = "";
  	
  }

}
```

Your application is ready. Now open http://localhost:4200

Enter title and content and submit it

![](/img/2017-02-05_17-06-44.png)

open firebase to check your submitted data.

Thanks :)

You can download source from my GitHub Repository
https://github.com/gurmandeep/angular2-http
