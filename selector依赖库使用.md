# MavenTest
selector依赖库

## 1、版本描述（请star支持）

### 后续开发优化计划
    1.预览界面可以拖动已选列表项，更换图片选中位置(已实现)
    2.预览界面长按弹出详情弹窗(已实现)
    3.当只选择一张图片的时候，提供裁剪功能(图片放大、旋转，裁剪框比例)
    4.(bug)标题设置了ellipsize,文字从 多-->少-->多 会出现省略号(已解决)
    5.视频选择可限制大小、时长
    6.支持同时选择图片、视频
    
  ### 1.0.0-beta4 特性
 * attr自定义主题属性增加selector_select_text、selector_media_cancel_visibility、selector_media_cancel_text、selector_media_cancel_text
 、selector_media_details_info_visibility等属性，具体用法请看后面
 * 界面UI布局优化，清除并退出按钮可根据attr值显示隐藏、更换文本，预览界面底部增加详细信息按钮，可用于查看媒体详情，可根据attr值隐藏
 * 预览界面已选列表项支持长按拖拽更换位置、支持侧滑删除选中项(PS:可设置.previewSelectMediaSwipeDelete(true)方法开启、关闭侧滑删除功能)
 * (bug解决)标题设置了ellipsize,文字从 多-->少-->多 会出现省略号
 * (bug解决)预览界面控件显示隐藏功能优化
 * (bug解决)媒体选择页面的文件夹选择弹窗位置、高度问题优化
    
 ### 1.0.0-beta3 特性
 * theme主题的selector_title_bottom_background属性拆分为selector_title_background、selector_bottom_background，增加selector_title_height
 、selector_bottom_height属性
 * 界面UI布局优化，增加清除并退出功能(清除选择并退出)
 * 支持传入application的context来打开图片选择器，可通过setOnSelectedListener方法获取选中媒体文件

### 1.0.0-beta 特性
 * 基本功能实现
 * 除了图片加载库、jiaozivideoplayer视频播放库以及谷歌support库，移除其他三方库

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
     //基本功能依赖库
     implementation 'com.github.Ghostbullets:hykjBase:1.5.47'
     //图片选择器，需配套使用hykjBase
     implementation 'com.base.selector:selector:1.0.0'
     
     //新的图片选择器依赖库,只需要依赖谷歌support库，无需依赖其他库，另外根据你选择的图片加载方式引用图片加载库(默认glide)
     //内部引用了jiaozivideoplayer用于视频播放
     implementation 'com.android.support:appcompat-v7:28.0.0'
     implementation 'com.android.support:recyclerview-v7:28.0.0'
     implementation 'com.android.support.constraint:constraint-layout:1.1.3'
     implementation 'com.cjf.selector:selector:1.0.0-beta4'
     
     //可选图片加载库
     implementation 'com.github.bumptech.glide:glide:4.10.0'
     implementation 'com.squareup.picasso:picasso:2.71828'

     
     //如果你升级到androidx，请使用下面依赖
     //基本功能依赖库
     implementation 'com.Ghostbullets.base:base-androidx:0.1.2'
     //图片选择器，需配套base-androidx使用
     implementation 'com.base.selector:selector-androidx1.0.1'
     
      //新的图片选择器依赖库,只需要依赖谷歌androidx库，无需依赖其他库
      (待转换为androidx发布)
    ···
}
```

### 2.2、代码中使用

```
使用示例
MediaAction.from(Activity activity)
                    .choose(new MimeType(MimeType.GetType.IMAGE))//选择媒体文件类型
                    .capture(true) //是否显示拍照功能,默认false
                    .themeId(R.style.MySelectorTheme) //允许设置主题，默认主题R.style.selectorTheme 具体属性请看下面代码
                    .providerName(".FileProvider") //7.0以上版本拍照用到provider，默认.FileProvider
                    .previewSelectMediaSwipeDelete(true) //预览界面，选中项是否支持上下侧滑删除，默认false
                    .countable(true)//是否显示自动添加的数字或者复选标记,即是否多选，默认false
                    .spanCount(4)//一行显示几张图,默认4
                    .imageEngine(new GlideEngine())//使用哪种图片加载器,并在gradle中implementation对应库，默认Glide
                    .maxSelectable(8)//最多可选择的数,默认1
                    .originalEnable(false)//是否显示原图单选框，默认false
                    .setOnSelectChangeListener() //媒体文件选中状态改变监听
                    .setOnSelectedListener(new OnSelectedListener() { //媒体文件选中监听,也可以在onActivityResult中接收
                        @Override
                        public void onSelected(@NonNull List<Uri> uriList, @NonNull List<String> pathList) {
                            list.clear();
                            list.addAll(pathList);
                        }
                    })
                    .setOnOriginalCheckedListener()//原图选中状态改变监听
                    .setSelectMediaPaths(list)//传入选中文件路径集合
                    // .setSelectMediaUris()//传入选中文件Url集合
                    .forResult(5);//开始选择媒体等待结果
  @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == 5) {
            if (resultCode == Activity.RESULT_OK && data != null) {
                list.clear();
                list.addAll(MediaAction.obtainPathResult(data));
            }
        }
    }
    
    //可传入application的context
    MediaAction.from(Context context)
                    .choose(new MimeType(MimeType.GetType.IMAGE))//选择媒体文件类型
                    .setSelectMediaPaths(list)//传入选中文件路径集合
                    .setOnSelectedListener(new OnSelectedListener() { //媒体文件选中监听
                        @Override
                        public void onSelected(@NonNull List<Uri> uriList, @NonNull List<String> pathList) {
                            list.clear();
                            list.addAll(pathList);
                        }
                    })
                    .start()
```

### 2.3、方法解析
```
MediaAction类方法讲解
//从页面启动选择器
from(Activity activity)
//从碎片启动选择器
from(Fragment fragment)
//从上下文启动选择器
from(Context context)
//在启动的活动或片段中获取用户选择的媒体的{@link Uri}列表
List<Uri> obtainResult(Intent data)
//获取启动的活动或片段中用户选择的媒体路径列表
List<String> obtainPathResult(Intent data)
//获取用户是否决定使用原图
boolean obtainOriginalState(Intent data)
//选择媒体文件类型
SelectionCreator choose(MimeType mimeType)


SelectionCreator类方法讲解
//如果只选择图像或视频作为媒体，是否只显示一种媒体类型。(暂无用)
showSingleMediaType(boolean showSingleMediaType)
设置selector 选择器的主题
themeId(@StyleRes int themeStyleId)
7.0以上版本可能要用到FileProvider
providerName(String providerName)
预览界面，选中项是否支持上下侧滑删除
previewSelectMediaSwipeDelete(boolean previewSelectMediaSwipeDelete)
//是否显示自动添加的数字或者复选标记
countable(boolean countable)
//最多可选择的数
maxSelectable(int maxSelectable)
为图像和视频媒体类型选择文件。(暂无用)
maxSelectablePerMediaType(int maxImageSelectable, int maxVideoSelectable)
//是否在媒体网格视图上启用了拍照功能。
capture(boolean enable)
//是否使用原图
originalEnable(boolean enable)
//最大原始大小，单位为MB，不可选择超过该大小的多媒体文件，使用原图情况下生效
maxOriginalSize(int size)
//设置此活动的预期方向(暂无用)
restrictOrientation(@ScreenOrientation int orientation)
//一行显示几张图
spanCount(int spanCount)
//设置item大小，控制一行显示个数
gridExpectedSize(int size)
//将照片缩略图的比例与视图的大小进行比较。它应该是一个浮点值(0.0，1.0)
thumbnailScale(@FloatRange(from = 0.0f, to = 1.0f) float scale)
//设置图片加载引擎
imageEngine(ImageEngine imageEngine)
//设置占位图
placeholder(@DrawableRes int placeholder)
//媒体文件选中状态改变监听，当用户选择或取消选择某些内容时，立即为回调设置侦听器。
setOnSelectChangeListener(@Nullable OnSelectChangeListener listener)
//媒体文件选中监听
setOnSelectedListener(@Nullable OnSelectedListener listener)
//当用户选中或取消选中原图时，立即将侦听器设置为回调。
setOnOriginalCheckedListener(@Nullable OnOriginalCheckedListener listener)
//选中文件路径集合
setSelectMediaPaths(@Nullable ArrayList<String> selectMediaPaths)
//选中文件Uri集合
setSelectMediaUris(@Nullable ArrayList<Uri> selectMediaUris)
开始选择媒体并等待结果。
forResult(int requestCode)
//通过上下文方式开始选择媒体
start()
       
```
### 2.4、主题设置
```
1.默认主题属性
 <!--selector 属性-->
    <!--头部标题左侧返回箭头 图标-->
    <attr name="selector_title_back_icon" format="reference" />
    <!--头部标题下拉上拉文件夹右侧标记 图标-->
    <attr name="selector_arrows_down_up_icon" format="reference" />
    <!--选择框 图标-->
    <attr name="selector_choice_box_icon" format="reference" />
    <!--单选框 图标-->
    <attr name="selector_radio_box_icon" format="reference" />
    <!--媒体文件封面阴影颜色,根据select不同显示 图像或颜色-->
    <attr name="selector_choice_shade_color" format="reference|color" />
    <!--媒体文件封面视频标记 图标-->
    <attr name="selector_video_icon" format="reference" />
    <!--媒体文件封面gif标记 图标-->
    <attr name="selector_gif_icon" format="reference" />
    <!--拍照图标-->
    <attr name="selector_camera_icon" format="reference" />
    <!--拍照图标背景色-->
    <attr name="selector_camera_background" format="reference|color" />

    <!--选中字体颜色 (PS:这里指右上角确定按钮)  引用或颜色-->
    <attr name="selector_select_text_color" format="reference|color" />
    <!--选中字体文本 (PS:这里指右上角确定按钮)-->
    <attr name="selector_select_text" format="string" />
    <!--所有字体颜色 引用或颜色-->
    <attr name="selector_text_color" format="reference|color" />
    <!--标题字体大小 引用或尺寸-->
    <attr name="selector_title_text_size" format="reference|dimension" />
    <!--所有字体大小 引用或尺寸-->
    <attr name="selector_text_size" format="reference|dimension" />
    <!--状态栏  背景颜色；图标颜色类型(白色或黑色) 仅在5.0以上生效-->
    <attr name="selector_status_background" format="reference|color" />
    <attr name="selector_status_icon_color_type" format="enum">
        <enum name="white" value="0" />
        <enum name="black" value="1" />
    </attr>
    <!--是否显示媒体文件选择页面的清除并退出按钮-->
    <attr name="selector_media_cancel_visibility" format="enum">
        <enum name="VISIBLE" value="0" />
        <enum name="GONE" value="8" />
    </attr>
    <!--清除并退出文本-->
    <attr name="selector_media_cancel_text" format="string" />
    <!--是否显示预览界面的详细信息按钮-->
    <attr name="selector_media_details_info_visibility" format="enum">
        <enum name="VISIBLE" value="0" />
        <enum name="GONE" value="8" />
    </attr>
    <!--页面背景色  引用或颜色-->
    <attr name="selector_background" format="reference|color" />
    <!--标题栏 背景色  引用或颜色-->
    <attr name="selector_title_background" format="reference|color" />
    <!--底部栏 背景色  引用或颜色-->
    <attr name="selector_bottom_background" format="reference|color" />
    <!--标题栏高度  尺寸-->
    <attr name="selector_title_height" format="dimension">
        <enum name="match_parent" value="-1" />
        <enum name="wrap_content" value="-2" />
    </attr>
    <!--底部栏高度  尺寸-->
    <attr name="selector_bottom_height" format="dimension">
        <enum name="match_parent" value="-1" />
        <enum name="wrap_content" value="-2" />
    </attr>
    <!--dialog 样式  引用-->
    <attr name="selector_dialog_style" format="reference" />
    <!--授权存储权限弹窗布局,里面必须有4个TextView id分别 为tv_title、tv_content、tv_confirm、tv_cancel-->
    <attr name="selector_permission_dialog_layout" format="reference" />
    <!--进度弹窗自定义布局-->
    <attr name="selector_progress_dialog_layout" format="reference" />

    <!--设备无资源时，显示图片、提示文字文本、颜色、字体大小-->
    <attr name="selector_empty_icon" format="reference" />
    <attr name="selector_empty_text" format="reference|string" />
    <attr name="selector_empty_color" format="reference|color" />
    <attr name="selector_empty_text_size" format="reference|dimension" />

2.默认主题(仿微信偏黑色风格)
  <style name="selectorTheme" parent="Theme.AppCompat.NoActionBar">
        <!--头部标题左侧返回箭头 图标-->
        <item name="selector_title_back_icon">@drawable/ic_selector_white_back</item>
        <!--头部标题下拉上拉文件夹右侧标记 图标-->
        <item name="selector_arrows_down_up_icon">@drawable/selector_arrows_down_up</item>
        <!--选择框 图标-->
        <item name="selector_choice_box_icon">@drawable/selector_media_icon</item>
        <!--单选框 图标-->
        <item name="selector_radio_box_icon">@drawable/selector_original_icon</item>
        <!--媒体文件封面阴影颜色,根据select不同显示 图像或颜色-->
        <item name="selector_choice_shade_color">@drawable/selector_choice_shade_color</item>>
        <!-- 媒体文件封面视频标记 图标-->
        <item name="selector_video_icon">@drawable/ic_selector_video</item>
        <!--媒体文件封面gif标记 图标-->
        <item name="selector_gif_icon">@drawable/ic_selector_gif</item>
        <!--拍照图标-->
        <item name="selector_camera_icon">@drawable/ic_selector_camera</item>
        <!--拍照图标背景色-->
        <item name="selector_camera_background">@color/selector_camera_background</item>
        <!--所有字体颜色 引用或颜色-->
        <item name="selector_text_color">@color/selector_text_color</item>
        <!--标题字体大小 引用或尺寸-->
        <item name="selector_title_text_size">@dimen/selector_title_text_size</item>
        <!--所有字体大小 引用或尺寸-->
        <item name="selector_text_size">@dimen/selector_text_size</item>
        <!--状态栏  背景颜色；图标颜色类型(白色或黑色) 仅在5.0以上生效-->
        <item name="selector_status_background">@color/selector_status_background</item>
        <item name="selector_status_icon_color_type">white</item>
        <!--是否显示清除并退出按钮，默认显示-->
        <item name="selector_media_cancel_visibility">VISIBLE</item>
        <!--清除并退出文本-->
        <item name="selector_media_cancel_text">@string/button_clear_default</item>
        <!--是否显示详细信息按钮，默认显示-->
        <item name="selector_media_details_info_visibility">VISIBLE</item>
        <!--页面背景色  引用或颜色-->
        <item name="selector_background">@color/selector_background</item>
        <!--标题栏 背景色  引用或颜色-->
        <item name="selector_title_background">@color/selector_title_background</item>
        <!--底部栏 背景色  引用或颜色-->
        <item name="selector_bottom_background">@color/selector_bottom_background</item>
        <!--标题栏高度  默认wrap_content 尺寸-->
        <item name="selector_title_height">wrap_content</item>
        <!--底部栏高度  默认wrap_content 尺寸-->
        <item name="selector_bottom_height">wrap_content</item>
        <!--选中字体颜色  引用或颜色-->
        <item name="selector_select_text_color">@color/selector_select_text_color</item>
        <!--选中字体文本-->
        <item name="selector_select_text">@string/button_sure_default</item>
        <!--dialog 样式  引用-->
        <item name="selector_dialog_style">@style/custom_dialog</item>
        <!--授权存储权限弹窗布局,里面必须有4个TextView id分别 为tv_title、tv_content、tv_confirm、tv_cancel-->
        <item name="selector_permission_dialog_layout">@layout/dialog_selector_confirm</item>
        <!--进度弹窗自定义布局-->
        <item name="selector_progress_dialog_layout">@layout/dialog_progress_bar_circle</item>

        <!--设备无资源时，显示图片、提示文字文本、颜色、字体大小-->
        <item name="selector_empty_icon">@drawable/ic_empty_icon</item>
        <item name="selector_empty_text">@string/selector_empty_text</item>
        <item name="selector_empty_color">@color/selector_empty_color</item>
        <item name="selector_empty_text_size">@dimen/selector_empty_text_size</item>
    </style>
    
    selector_media_icon.xml
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/ic_selector_choice_box_check" android:state_selected="true" />
    <item android:drawable="@drawable/ic_selector_choice_box_un_check" />
    </selector>
    
    selector_original_icon.xml
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/ic_selector_radio_box_check" android:state_selected="true" />
    <item android:drawable="@drawable/ic_selector_radio_box_un_check" />
    </selector>
    
    selector_choice_shade_color.xml
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@color/selector_choice_check_shade_color" android:state_selected="true" />
    <item android:drawable="@color/selector_choice_un_check_shade_color" />
    </selector>
    
    selector_arrows_down_up.xml
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/ic_selector_arrows_up" android:state_selected="true" />
    <item android:drawable="@drawable/ic_selector_arrows_down" />
    </selector>


```
