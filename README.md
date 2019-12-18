# MavenTest
com.Ghostbullets.base:base-androidx，
com.base.wheelview:wheelview-androidx，
com.base.pickerview.pickerview-androidx，
com.base.selector.selector-androidx，
依赖库使用：

在项目的build.gradle下加入仓库
allprojects {
    repositories {
        google()
        jcenter()
        maven { url 'https://raw.githubusercontent.com/Ghostbullets/MavenTest/master' }
    }
}
在应用该库的module的dependencies里面引用
dependencies {
implementation 'com.Ghostbullets.base:base-androidx:0.1.0'//单独
----------------------------------------------------------------
implementation com.base.wheelview:wheelview-androidx1.0.1
implementation com.base.pickerview.pickerview-androidx1.0.1//需配套wheelview-androidx使用，也可以只写这一行
----------------------------------------------------------------------
implementation com.base.selector.selector-androidx1.0.1//需配套base-androidx使用
---------------------------------------------------------------------
}
