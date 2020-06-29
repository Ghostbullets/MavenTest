# MavenTest
ipc依赖库演示

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
     //WebView独立进程解决方案
     //参考https://github.com/xudjx/webprogress
     implementation 'com.cjf.ipc:ipc:1.0.0'     
     
     //如果你升级到androidx，请使用下面依赖
     implementation 'com.cjf.ipc:ipc-androidx:1.0.0'
}
```

3.执行发送、处理命令

```
 /**
     * 执行命令
     *
     * @param context            上下文
     * @param actionCategory     要执行的主进程行动命令所属类型
     * @param actionName         要执行的主进程行动命令名称
     * @param params             参数
     * @param dispatcherCallBack 命令执行回调
     */
    public void execute(final Context context, final String actionCategory, final String actionName, final String params, final DispatcherCallBack dispatcherCallBack)
     //发送 获取用户手机号命令 
    CommandDispatcher.getInstance()
            .execute(getApplication(), ActionCategoryName.DATA_PROVIDER, MethodCallType.ACCESS_USER_PHONE, "", this)
            
    在要处理进程命令的地方，例如MainActivity，根据所需，实现Command接口，处理webView进程发送的命令
     //处理数据获取
        dataProviderCommand = object : Command {
            override fun name(): String {
                return ActionCategoryName.DATA_PROVIDER //获取数据
            }

            override fun execute(
                context: Context,
                actionName: String,
                params: Map<*, *>?,
                resultCallBack: ResultCallBack
            ) {
                try {
                    when (actionName) {
                        MethodCallType.ACCESS_USER_PHONE -> {//获取用户手机号
                            resultCallBack.onResult(0, "12345678911")
                            return
                        }
                        else -> {
                        }
                    }
                    aidlError(resultCallBack)
                } catch (e: Exception) {
                    e.printStackTrace()
                    aidlError(resultCallBack)
                }
            }
        }
        
        /**
     * aidl调用失败
     *
     * @param resultCallBack
     */
    private fun aidlError(resultCallBack: ResultCallBack) {
        val aidlError =
            AidlError(-1003,“参数错误”)
        resultCallBack.onResult(-1, aidlError)
    }
```
