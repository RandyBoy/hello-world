1.用户输入
  a)	input（keyup)=”onKeyup($event)”  通过$event.target.value获取值
  b)	Input  #box  (keyup)=”onkeyup(box.value)” 定义模板变量，模板变量指向元素，我们可以从它的任何兄弟或子节点访问模板变量
  c)	{{box.value}} 模板内引用本地变量值
  d)	<input #box (keyup.enter)="values=box.value">Angular的keyup.enter伪事件，我们可以只监听“Enter”事件(blur)="addHero(newHero.value); newHero.value='' " 绑定事件内可以多行代码
  e)	(blur)="values=box.value" blur事件在元素页面单击时触发

2.表单输入
  a)	[(ngModel)] 双向绑定值，ngModelChange是ngModel的模板事件属性
  b)	如果用户触发了控制器，值发生了变化或者值变得无效了我们的应用能够唤起NgControl=绑定到元素的属性，值”ngForm”(#name=”ngForm”)为模板局部变量(name)设置初始值。Angular能够识别这种语法然后重新设置这个name的模板局部变量来替代这个ngControl指令。换句话说，这个局部变量name会控制变成这个输入框的ngControl对象。
  c)NgControl不仅仅监视状态。它会在这三个影响状态的方面更新控制器。
      State	Class if true	Class if false
      控制器被访问	ng-touched	ng-untouched
      控制器被访问	ng-dirty	ng-pristine
      控制器的值有效	ng-valid	ng-invalid
  d)<form (ngSubmit)=”onSubmit()” #heroForm=”ngForm”>定义模板局部变量—#heroForm，并用”ngForm”来初始化它的值。
3.属性指令
  a)@Directive装饰器表示属性指令,使用ElementReference元素引用）和Renderer（渲染器）符号来作为依赖注入到指令的构造函数，命名格式：selector: '[myHighlight]',，注意中括号
  b)开始事件检测。我们在指令元数据中添加一个host 属性,在属性配置对象声明了两个鼠标事件以及事件发生时所触发的指令方法。
  c)	host 属性指的是作为属性指令宿主的DOM元素
  d)	host: { // 定义在@Directive内部
  	  '(mouseenter)': 'onMouseEnter()',
  	  '(mouseleave)': 'onMouseLeave()'
  	}
h)	@Input() set defaultColor(colorName:string){ //定义输入属性
	  this._defaultColor = colorName || this._defaultColor;
	}

4.结构指令
a)	constructor(
b)	  private _templateRef: TemplateRef,//访问模版
c)	  private _viewContainer: ViewContainerRef //渲染器
d)	  ) { }
e)	@Input() set myUnless(condition: boolean) {
f)	  if (!condition) {
g)	    this._viewContainer.createEmbeddedView(this._templateRef);
h)	  } else {
i)	    this._viewContainer.clear();
j)	  }
k)	}
l)	<p *ngIf="condition">
m)	  Our heroes are true!
n)	</p>
o)	<!-- (B) [ngIf] with template -->
p)	<template [ngIf]="condition">
q)	<p>
r)	    Our heroes are true!
s)	</p>
t)	</template>

u)	<div *ngFor="#hero of heroes">{{ hero }}</div>
v)	<!-- (B) ngFor with template -->
w)	<template ngFor #hero [ngForOf]="heroes">
x)	<div>{{ hero }}</div>
y)	</template>
z)	div *ngIf="hero">{{hero}}</div>
aa)	<div *ngFor="#hero of heroes">{{hero}}</div>
bb)	<div [ngSwitch]="status">
cc)	<template [ngSwitchWhen]="'in-mission'">In Mission</template>
dd)	<template [ngSwitchWhen]="'ready'">Ready</template>
ee)	<template ngSwitchDefault>Unknown</template>
ff)	</div>
gg)	<hr>

5.模板语法
数据流向	语法	绑定类型
单向：数据源到目标视图	{{expression}}
[target]=”expression”
bind-target=”expression”	插值（Interpolation)
DOM属性(property）
HTML属性（Attribute）
类（Class）
样式（Style）
单向：目标视图到数据源	（target)="expression"
on-target="expression"	事件（Event）
双向	[(target)]=”expression”
bindon-target=”expression”	
绑定类型	目标	举例
属性（Property）	DOM属性
组件属性
指令属性	<img [src]="heroImageUrl">
<hero-detail [hero]="currentHero">
</hero-detail>
事件	DOM事件
组件事件
指令事件	<button (click)="onSave()">Save</button>
<hero-detail (deleted)="onHeroDeleted()">
<hero-detail>
<div myClick (myClick)="clicked=$event">
click me</div>
双向	指令事件
DOM属性	<input [(ngModel)]=“heroName”>
HTML属性	HTML属性（特例）	<button [attr.aria-label]="help">
help</button>
类	类属性	<div [class.special]="isSpecial">
Special</div>
样式	样式属性	<button [style.color]="isSpecial?'red':'green'">
a)	
6.属性（Attribute）绑定, 类绑定和样式绑定
a)	Atrribute绑定在属性名称之前加attr关键字
b)	类绑定在类名之前加上class关键字[class.class-name]<div [class.special] = "isSpecial"> 表达式为真应用样式，否则移除。
c)	样式绑定在样式属性之前加上style关键字[style.style-property]。
d)	<button [style.color]="isSpecial?'red':'green'">Red</button>
e)	<button [style.backgroundColor]="canSave?'cyan':'grey'">Save</button>
f)	<button [style.fontSize.em]="isSpecial?3:1">Big</button> //样式单位设置
g)	<button [style.fontSize.%]="isSpecial?150:50">Small</button> //样式单位设置
7.事件绑定
a)	deleted = new EventEmitter<Hero>(); //自定义事件
b)	onDeleted(){ 触发事件
c)	    this.deleted.emit(this.hero); 发送值通过$event传递给父组件
d)	} 
e)	<hero-detail (deleted)="onHeroDeleted($event)" [hero]="currentHero"></hero-detail>
当绑定的表达式的返回结果为假值（包括没有返回值）时，事件传播会停止，当表达式的返回结果为真值时，事件传播会继续。按钮和其外层div都会监听到click事件<div (click)="onSave()">
<button (click)="onSave || true">Save will propagation</button>
</div>
8.NgModel实现双向绑定
a)	<input [(ngModel)]="currentHero.firstName">
b)	<input bindon-ngModel="currentHero.firstName">

9.内置指令
a)	NgClass方法给它绑定一个key:value对象，其中key对应于一个类名，当它对应的value为真时，类被添加，反之，类被移除。
b)	div [ngClass]="setClasses()">This div is saveable and special</div>setClasses(){
c)	    return {
d)	        saveable:this.canSave,      //true
e)	        modified:!this.isUnchanged, //false
f)	        special:this.isSpecial      //true
g)	    }
h)	}


10.NgStyle
a)	使用NgStyle的方法就是给它绑定一个key:value对象，其中key对应于一个样式名，vaule对应于该样式的具体设置。setStyles(){
b)	    return {
c)	        //CSS property names
d)	        fontStyle:this.canSave ? 'italic':'normal', //italic
e)	        fontWeight:!this.isUnchanged ? 'bold':'normal', //normal
f)	        fontSize:this.isSpecial ? 'x-large':'smaller' //x-large
g)	    }
h)	}
i)	
11.依赖注入
a)	bootstrap(AppComponent, [HeroService]); bootstrap的provider选项的目的是配置和覆盖Angular自己预注册的服务,路由服务和HTTP服务,创建单例服务
b)	组件中注入服务providers:[HeroService] 
c)	创建服务类加@Injectable()装饰器
d)	可选服务注册constructor(@Optional() private _logger:Logger) {  }
e)	Injector provider依赖提供者: providers: [Logger]等价于 [new Provider(Logger, {useClass: Logger})]
f)	useClass:[UserService,provide(Logger,{useClass: EvenBetterLogger})]
g)	useExisting:[NewLogger, provide(OldLogger,{useExisting: NewLogger})]//别名提供者
h)	useValue:[provide(Logger, {useValue: silentLogger})] //值提供者
i)	factory provider: let heroServiceFactory = (logger: Logger, userService: UserService) => {
j)	          return new HeroService(logger, userService.user.isAuthorized);}
export let heroServiceProvider =
k)	  provide(HeroService, {
l)	    useFactory: heroServiceFactory //provider是工厂函数
m)	    deps: [Logger, UserService] //provider token数组
n)	  });
o)	字符串token:[provide('app.config', {useValue: CONFIG})]
p)	// @Inject(token) to inject the dependency
q)	constructor(@Inject('app.config') private _config: Config){ }
r)	
s)	OpaqueToken:import {OpaqueToken} from 'angular2/core';
t)	
u)	export let APP_CONFIG = new OpaqueToken('app.config');
v)	providers: [
w)	  Logger,
x)	  UserService,
y)	  provide(APP_CONFIG, {useValue: CONFIG}) //注册
z)	constructor(
aa)	  @Inject(APP_CONFIG) config:Config, //使用
bb)	  private _userService: UserService) {this.title = config.title;
cc)	}]
dd)	直接使用依赖:
ee)	1.providers: [Car, Engine, Tires,heroServiceProvider, Logger] //导入相关类
ff)	2.constructor(private _injector: Injector) { } \\注入依赖类
gg)	  car:Car = this._injector.get(Car); //使用服务类
hh)	  heroService:HeroService = this._injector.get(HeroService);



