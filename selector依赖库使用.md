# MavenTest
selector依赖库

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
     //基本功能依赖库
     implementation 'com.github.Ghostbullets:hykjBase:1.5.47'
     //图片选择器，需配套使用hykjBase
     implementation 'com.base.selector:selector:1.0.0'
     
     //如果你升级到androidx，请使用下面依赖
     //基本功能依赖库
     implementation 'com.Ghostbullets.base:base-androidx:0.1.2'
     //图片选择器，需配套base-androidx使用
     implementation 'com.base.selector:selector-androidx1.0.1'
    ···
}
```

3.使用

```
使用示例
MediaAction.from(Activity activity)
                    .choose(new MimeType(MimeType.GetType.IMAGE))//选择媒体文件类型
                    .countable(true)//是否显示自动添加的数字或者复选标记
                    .spanCount(4)//一行显示几张图
                    .imageEngine(new GlideEngine())//使用哪种图片加载器
                    .maxSelectable(8)//最多可选择的数
                    .originalEnable(false)//是否使用原图(暂时没用)
                    .setOnSelectedListener(new OnSelectedListener() {
                        @Override
                        public void onSelected(@NonNull List<Uri> uriList, @NonNull List<String> pathList) {

                        }
                    })
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


MediaAction类方法讲解
//从页面启动选择器
from(Activity activity)
//从碎片启动选择器
from(Fragment fragment)
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
//是否显示自动添加的数字或者复选标记
countable(boolean countable)
//最多可选择的数
maxSelectable(int maxSelectable)
为图像和视频媒体类型选择文件。(暂无用)
maxSelectablePerMediaType(int maxImageSelectable, int maxVideoSelectable)
//是否在媒体网格视图上启用了拍照功能。(功能暂未实现)
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
//媒体文件选中监听
setOnSelectedListener(@Nullable OnSelectedListener listener)
//当用户选中或取消选中原图时，立即将侦听器设置为回调。
setOnCheckedListener(@Nullable OnCheckedListener listener)
//选中文件路径集合
setSelectMediaPaths(@Nullable ArrayList<String> selectMediaPaths)
//选中文件Uri集合
setSelectMediaUris(@Nullable ArrayList<Uri> selectMediaUris)
开始选择媒体并等待结果。
forResult(int requestCode)
       
```
