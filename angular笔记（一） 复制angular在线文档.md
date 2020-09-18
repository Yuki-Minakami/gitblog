主要任务是自己实现一个类似angular.io/docs的在线文档，由于网速的原因，这里选择的是文档的中文翻译版angular.cn

API： 直接拿网站现成的，反正也不需要登录，统一格式为"https://angular.cn/generated/docs/"+url+".json"

文档的目录信息，其实也可以用在线API的形式，但JSON的内容比较少，所以就直接放本地加载了
这里要稍微吐槽一下angular的设计，在开发中加载本地资源文件也要用http的方式来获得，有些繁琐。


```
 this.http.get('assets/navigate.json')
    .subscribe((res:Response) => {
      this.navigateData = res.json()
      console.log(this.navigateData);
    });
```


稍微总结一下，这个项目的主要难点有两个
- 递归式的组件调用
- 组件间的信息传递

先来看第一点

```
"SideNav": [
      {
        "url": "docs",
        "title": "简介",
        "tooltip": "Angular 文档简介",
        "hidden": false
      },
      {
        "title": "快速上手",
        "tooltip": "建立开发环境，并学习基本概念",
        "children": [
          {
            "url": "guide/setup-local",
            "title": "搭建环境",
            "tooltip": "使用 Angular CLI 搭建本地开发环境。"
          },
          {
            "title": "Angular 的基本概念",
            "tooltip": "介绍 Angular 应用中的一些基本概念。",
            "children": [
              {
                "url": "guide/architecture",
                "title": "基本概念简介",
                "tooltip": "Angular 应用中的基本构造块。"
              },
              {
                "url": "guide/architecture-next-steps",
                "title": "后续步骤",
                "tooltip": "超越基础阶段。"
              }
            ]
          }

```