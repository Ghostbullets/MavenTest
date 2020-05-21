# MavenTest
集成了图片选择、滚轮、基本模块等等仓库

## 1、演示（请star支持）

## 2、如何使用
目前支持主流开发工具AndroidStudio的使用，直接配置build.gradle，增加依赖即可.

### 2.1、Android Studio导入方法，添加Gradle依赖

1.先在项目根目录的 build.gradle 的 repositories 添加:
```
allprojects {
    repositories {
        ...
        maven { url "https://jitpack.io" }
        maven { url 'https://raw.githubusercontent.com/Ghostbullets/MavenTest/master' }
    }
}
```

2.在项目的 build.gradle 中增加依赖并引用

```
apply plugin: 'com.cjf.aop' //引用aop插件

dependencies {
    ···
     //添加依赖
     //基本功能依赖库
      implementation 'com.github.Ghostbullets:hykjBase:1.5.47'

     //滚轮，需配套wheelview使用，也可以只写pickerview(内部已依赖wheelview)
     implementation 'com.base.wheelview:wheelview:1.0.1'
     implementation 'com.base.pickerview:pickerview:1.0.1'

     //图片选择器，需配套使用hykjBase
     implementation 'com.base.selector:selector:1.0.0'
     
     //轮播图
     implementation 'com.base.banner:banner:1.0.1'
     //WebView独立进程解决方案
     //参考https://github.com/xudjx/webprogress
     implementation 'com.cjf.ipc:ipc:1.0.0'
     //适合mvvm 框架使用的listview、recyclerview、viewpager适配器
     implementation 'com.cjf.bindingadapter:bindingadapter:1.0.0'
     

     //如果你升级到androidx，请使用下面依赖
      //基本功能依赖库
     implementation 'com.Ghostbullets.base:base-androidx:0.1.2'

     //滚轮，需配套wheelview-androidx使用，也可以只写pickerview(内部已依赖wheelview)
     implementation 'com.base.wheelview:wheelview-androidx:1.0.1'
     implementation 'com.base.pickerview:pickerview-androidx1.0.1'

     //图片选择器，需配套base-androidx使用
     implementation 'com.base.selector:selector-androidx1.0.1'
     
     //轮播图
     implementation 'com.base.banner:banner-androidx:1.0.0'
}
```

3.在Application中进行初始化

```
        //基础功能库
        ContextKeep.init(this);
        SPUtils.init();
        FileUtil.init();
```

## 3、详细使用请参考
    banner依赖库使用.txt  
    selector依赖库使用.txt
    wheelview跟pickerview依赖库使用教程.txt
    ipc依赖库使用.md
    bindingadapter依赖库使用.md
