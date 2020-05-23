# MavenTest
banner依赖库

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
dependencies {
    ···
     //添加依赖
     implementation 'com.cjf.banner:banner:1.0.0'
     
      //androidx
     implementation 'com.base.banner:banner-androidx:1.0.0'
    ···
}
```

3.使用

```
Banner的xml属性-------------------------
app:banner_default_image=""空数据时的背景图资源
app:banner_delay_time=""轮播延迟时间，默认2000毫秒
app:banner_scroll_time=""轮播图切换滚动时间，默认800毫秒
app:banner_is_auto_play=""是否允许自动切换轮播图
app:banner_is_scroll=""是否允许手动滑动切换轮播图
app:banner_image_scale_type=""图片缩放类型，参考ScaleType类
app:banner_page_limit=""viewPager左右两边预留页数
app:banner_layout=""轮播图布局，默认R.layout.banner，如要自定义，则id名必须一样
app:banner_style=""轮播图样式，默认圆点指示器，有
public @interface BannerStyle {
        int NOT_INDICATOR = 0;//没有指示器
        int CIRCLE_INDICATOR = 1;//圆形指示器
        int NUM_INDICATOR = 2;//数字指示器
        int NUM_INDICATOR_TITLE = 3;//数字指示器+标题
        int CIRCLE_INDICATOR_TITLE = 4;//圆形指示器+标题
        int CIRCLE_INDICATOR_TITLE_INSIDE = 5;//圆形指示器+标题内嵌式
    }

app:banner_indicator_drawable_selected=""指示器选中图片资源
app:banner_indicator_drawable_unselected=""指示器不选中图片资源
app:banner_indicator_gravity=""指示器位置START，CENTER，END
app:banner_indicator_margin=""指示器间距
app:banner_indicator_height=""指示器高
app:banner_indicator_width=""指示器宽

app:banner_title_background=""标题背景颜色
app:banner_title_height=""标题高度
app:banner_title_text_color=""标题文字颜色
app:banner_title_text_size=""标题文字大小

方法讲解
暂无
       
```
