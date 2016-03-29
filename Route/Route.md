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
    Link Parameters Array：链接参数数组。router会将这个数组解释成路由指令。我们可以将这个数组绑定到RouterLink，或者作为参数传递给Router.navigate方法。
    Routing Component。配置了路由的组件。
6.子路由配置
    @RouteConfig([
    { // Crisis Center child route
        path: '/crisis-center/...', //表示该路由不完整,要配置子路由配置完成路由
        name: 'CrisisCenter',
        component: CrisisCenterComponent,
        useAsDefault: true },
        {path: '/heroes',   name: 'Heroes',     component: HeroListComponent},
        {path: '/hero/:id', name: 'HeroDetail', component: HeroDetailComponent},
    ])
