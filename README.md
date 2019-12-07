# MavenTest
com.Ghostbullets.base:base-androidx依赖库使用：

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
implementation 'com.Ghostbullets.base:base-androidx:0.1.0'
}
