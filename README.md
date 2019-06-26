# Angular

> **・Angular Material** - AngularでMaterial Designのcssフレームワークを使うためのコンポーネント。
> __・Flex-layout__ -  レイアウトを調整する手段


### ngSelect reset

~~~typescript
	@ViewChild('#name') ngSelect: NgForm;
	reset(){
		this.ngSelect.reset();
	}
~~~

### call child function

~~~html
<section>
	<header>
		<mat-radio-group>
			<mat-radio-button (change)="arbitraryChildName.func()">
				
			</mat-radiopbutton>
		</mat-radio-group>
	</header>
	<app-child #arbitraryChildName>
	</app-child>
</section>
~~~

### flex css
 ~~~css

	div.u-margin-top-m{
		display:flex;
	}

	matp-form-field{
		flex-basis:1300px;
		flex-shrink:1;
	}
	
~~~

### pass data child to parent
~~~typescript
<app-child>
	@Input() passData:string;
	@Output() eventGlmNo  =  new  EventEmitter<string>();

	eventTrigger(ngForm:NgForm){
		this.passData=ngForm.value.glmNo;
		this.eventGlmNo.emit(ngForm.value.glmNo);
	}
	/*
	[passData]="psGlmNo"
	(eventGlmNo)="onReceiveEventFromChild($event)"
	*/
</app-child>

<app-parent>
	public  psGlmNo;

	onReceiveEventFromChild(eventData:string){
		this.psGlmNo  =  eventData;
	}
</app-parent>
~~~

### pass data parent to child
~~~typescript
<app-child>
	private  _passedGlmNo:string;
	@Input() set  passedGlmNo(value:string){

	this._passedGlmNo=value;
	this.reload();
	
	/*****取得済みのリストにフィルターをかける*****/
	var  ll  :LoadingDataInformation [];
	let  tmplist  =  new  Array;
	//Observableのため
	this.loadingDataInformationList$.subscribe(v  =>
		tmplist.push(...v.list.filter(a=>a.glmNo===value))
	);
	
	ll  =  tmplist  as  LoadingDataInformation[];
	var  lld  :LoadingDataInformationResponse={list:ll,hasNext:false,cursor:''};
	//Observable
	this.loadingDataInformationList$  =  of(lld);
	
	/*
	[passedGlmNo]="psGlmNo"
	*/
	}
</app-child>

<app-parent>
	public  psGlmNo;

	onReceiveEventFromChild(eventData:string){
		this.psGlmNo  =  eventData;
	}
</app-parent>

~~~

### innerHTML
~~~typescript
let  ele  =  document.getElementsByName("glmNo")[0];
console.log('innerHTML : ',ele.innerHTML);
~~~

### Get route-outlet Data

```html:app.html
<app-header #header [menuNum]="menu"></app-header>
<router-outlet #child (activate)="onActivate($event)"></router-outlet>
```

```typescript:app.component.ts
import { Component, ViewChild } from  '@angular/core';
import {HeaderComponent} from  './header/header.component';
import { headersToString } from  'selenium-webdriver/http';

@Component({
	selector:  'app-root',
	templateUrl:  './app.component.html',
	styleUrls: ['./app.component.css']
})

export  class  AppComponent {

	public  menu:string ;

	onActivate(componentRef:  any):  void { // <= 追加
		this.menu  =  componentRef.menu;
	}
}
```

```typescript:header.component.ts
...
@input() menuNum:number;
...
```

