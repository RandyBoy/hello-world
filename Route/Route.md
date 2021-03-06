1.路由是可选的,引用路由库:
    <script src="node_modules/angular2/bundles/router.dev.js"></script>
    
2.使用history.pushState导航时必须在 index.html中添加<base href>来启用它.
        <head> <base href="/"></head>
    
3.路由配置当浏览器的URL变化时，router会寻找相应的RouteDefinition并显示其组件。创建路由并将路由添加到主组件的@RouteConfig
    @Component({ ... })
    @RouteConfig([
        {path:'/crisis-center', name: 'CrisisCenter', component: CrisisListComponent},
        {path:'/heroes',        name: 'Heroes',       component: HeroListComponent},
        {path:'/hero/:id',      name: 'HeroDetail',   component: HeroDetailComponent}
    ])
    export class AppComponent { }
路由的名称name，它必须是PascalCase拼写风格。 as 别名
路由参数:id，URL是/hero/42，参数id的值=42。
导航的组件:component 
useAsDefault: true 默认路由,子路由也要设置一个默认值.

4.总结:关于Router的概念有：
        Router：根据URL显示组件，管理组件之间的跳转。
        @RouteConfig：使用RouteDefinitions配置Router，每一个映射一个URL路径到组件。
        RouteDefinition：定义router如何根据URL进行导航。
        Route：最常见的RouteDefinition形式，包括URL路径、路由名称和组件。
        RouterOutlet：标记router在哪里显示视图的指令（<router-outlet>）。
        RouterLink：用于绑定一个可点击的HTML元素到route的指令。
        Link Parameters Array：链接参数数组。router会将这个数组解释成路由指令。我们可以将这个数组绑定到RouterLink，或者作为参数传递给
        Router.navigate方法。
        Routing Component。配置了路由的组件。
    
6.子路由配置
    在父亲组件上定义不完整的子路由
    @RouteConfig([
        { // Crisis Center child route
            path: '/crisis-center/...', //表示该路由不完整,要配置子路由配置完成路由
            name: 'CrisisCenter',
            component: CrisisCenterComponent,
            useAsDefault: true },
            {path: '/heroes',   name: 'Heroes',     component: HeroListComponent},
            {path: '/hero/:id', name: 'HeroDetail', component: HeroDetailComponent},
        ])
    在子组件上定义路由:
    @RouteConfig([
         {path:'/',    name: 'CrisisList',   component: CrisisListComponent, useAsDefault: true},
        {path:'/:id', name: 'CrisisDetail', component: CrisisDetailComponent}
    ])
    
7.路由方法
        import {ROUTER_PROVIDERS} from 'angular2/router';
        bootstrap(AppComponent, [ROUTER_PROVIDERS]);
        1)导入路由模块
            import {Router} from 'angular2/router';
        2)导航到路由
            // Like <a [routerLink]="['Heroes']">Heroes</a>
            this._router.navigate(['Heroes']);  //不带参数
            let link = ['HeroDetail', { id: hero.id }]; //直接定义成对象传递
            this._router.navigate(link);
            this._router.navigate(['CrisisDetail', { id: crisis.id }]  ); //参数
            this._router.navigate(['Heroes',  {id: heroId, foo: 'foo'} ]); //参数对象 
            子路由的URL:localhost:3000/crisis-center/;id=3;foo=foo
            正常路由的URL:localhost:3000/crisis-center/?id=3&foo=foo
            //组件类中获取路由参数
            import {Router, RouteParams} from 'angular2/router';
            constructor( private _service: HeroService,private _router: Router,routeParams: RouteParams) {
                this._selectedId = +routeParams.get('id');//获取路由参数
            }
            3)路由数据
                import {Router, RouteData} from 'angular2/router';
                @RouteConfig([
                    {path: '/user/:id', component: UserCmp,name:'UserCmp',data:{isAdmin:true}}, //数据传递,对象格式:{(key:string):value}
                ])
                constructor(data: RouteData) { //data.get(string) 获取KEY的值; data.data 返回原传递对象
                    this.isAdmin = data.get('isAdmin'); //获取传递的数据
                }
            4)路由参数
                import {RouteConfig,RouteParams} from 'angular2/router';
                @RouteConfig([
                        {path: '/user/:id', component: UserCmp, name: 'UserCmp'}, //:id参数
                    ])
                class AppCmp {}
                
                @Component({ template: 'user: {{id}}' }) //调用获取的参数
                class UserCmp {
                    id: string;
                     constructor(params: RouteParams) {  //params : {[key: string]: string} 获取参数串 get([key:string]) 获取路由传递参数
                     this.id = params.get('id');  //获取路由传递的参数
                }
            5)RouterLink
                <a [routerLink]="['/User']">link to user component</a>  
                ['/Team', {teamId: 1}/*Team的参数*/, 'User', {userId: 2}/*User的参数*/] 表示基于路由命名NAME的路径: /Team/1/User/2
            6)RouteDefinition
                path or aux (requires exactly one of these)
                component, loader, redirectTo (requires exactly one of these)
                name or as (optional) (requires exactly one of these)
                data (optional)
            7)Route
                path is a string that uses the route matcher DSL.
                component a component type.
                name is an optional CamelCase string representing the name of the route.
                data is an optional property of any type representing arbitrary route metadata for the given route. It is injectable via RouteData.
                useAsDefault is a boolean value. If true, the child route will be navigated to if no child route is specified during the navigation.
            8)Redirect
                import {RouteConfig, Route, Redirect} from 'angular2/router';
                @RouteConfig([
                    new Redirect({path: '/', redirectTo: ['/Home'] }), //指定路径和重定向的路径名称
                    new Route({path: '/home', component: HomeCmp, name: 'Home'})
                ])
            9)Router
                navigate(linkParams: any[]) : Promise<any>  //导航到指定路由Name链接
                ['./MyCmp', {param: 3}]
                navigateByUrl(url: string, _skipLocationChange?: boolean) : Promise<any>
                //导航到指定URL的路由,URL指定/开头就是绝对路径,否则就是相对路径
        
        8.router生命周期钩子
            可以改变路由的行为。router的钩子函数可以组织导航到一个路径，如果返回值是false，就取消导航，保持在当前视图。还可以让路由导航到里你跟一个组件。
        CanActivate
        OnActivate
        CanDeactivate:
            用户修改数据后通常需要验证，我们需要记录修改后的数据状态，直到确认保存或者取消修改。还有，如果在编辑过程中点击了链接导航，我         们不能直接丢弃用户所作的修改直接跳转到新视图，我们应该暂停跳转，让用户自己选择，如果用户取消那就继续保留在当前视图，如果用户决定         继续跳转，那就保存当前的修改。同时我们需要延迟跳转直到保存成功。但是我们不能在等待保存时阻塞浏览器响应，我们需要停止跳转，等待服         务器异步响应。因此我们需要使用CanDeactivate。
            import {CanDeactivate, ComponentInstruction} from 'angular2/router';
            import {DialogService} from '../dialog.service';
            export class CrisisDetailComponent implements OnInit, CanDeactivate {
                crisis: Crisis;
                editName: string;
                routerCanDeactivate(next: ComponentInstruction, prev: ComponentInstruction) : any {
                    // Allow synchronous navigation (`true`) if no crisis or the crisis is unchanged.
                    if (!this.crisis || this.crisis.name === this.editName) {
                        return true;
                }
                    // Otherwise ask the user with the dialog service and return its
                     // promise which resolves to true or false when the user decides
                return this._dialog.confirm('Discard changes?');
                }
            }
        9.路由后退
            window.history.back()
            
