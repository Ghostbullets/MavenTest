# MavenTest
bindingadapter依赖库，参考 https://github.com/evant/binding-collection-adapter

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
     //适合mvvm 框架使用的listview、recyclerview、viewpager适配器
     implementation 'com.cjf.bindingadapter:bindingadapter:1.1.0'
     
     //如果你升级到androidx，请使用下面依赖
     implementation 'com.cjf.bindingadapter:bindingadapter-androidx:1.1.0'
    ···
}
```

3.使用

```
/**
         * 用于xml设置adapter的相关属性mvvm
         */
        /**
         * @param recyclerView recyclerView控件
         * @param adapter 适配器
         * @param bindingClickListener 点击监听，可为空
         * @param bindingLongClickListener 长按监听，，可为空
         * @param diffItemCallback 用于计算列表中两个非空项之间的差异的回调，可为空 https://blog.csdn.net/zxt0601/article/details/52562770
         * @param itemBinding 提供所需的信息将集合中的项绑定到视图，可为空，为空时recycler.setAdapter=null
         * @param itemIds itemId集合，可为空
         * @param items item 数据集合，可为空
         * @param viewHolder RecyclerView.ViewHolder，可为空
         */
        @BindingAdapter(
            "adapter",
            "bindingClickListener",
            "bindingLongClickListener",
            "diffItemCallback",
            "itemBinding",
            "itemIds",
            "items",
            "viewHolder", requireAll = false
        )
        @JvmStatic
        fun <T : ItemLayoutRes> setAdapter(
            recyclerView: RecyclerView,
            adapter: BindingRecyclerViewAdapter<T>?,
            bindingClickListener: BindingRecyclerViewAdapter.OnBindingItemClickListener<T>?,
            bindingLongClickListener: BindingRecyclerViewAdapter.OnBindingItemLongClickListener<T>?,
            diffItemCallback: DiffUtil.ItemCallback<T>?,
            itemBinding: ItemBinding<in T>?,
            itemIds: ItemIds<in T>?,
            items: MutableList<T>?,
            viewHolder: BindingRecyclerViewAdapter.ViewHolderFactory?
        )


<android.support.v7.widget.RecyclerView
            android:id="@+id/rv_menu"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:overScrollMode="never"
            android:scrollbars="none"
            binding:adapter="@{viewModel.adapter}"
            binding:bindingClickListener="@{viewModel.onBindingItemClickListener}"
            binding:bindingLongClickListener="@{viewModel.onBindingItemLongClickListener}"
            binding:itemBinding="@{viewModel.itemBinding}"
            binding:items="@{viewModel.menuGroup.items}"
            binding:layoutManager="android.support.v7.widget.LinearLayoutManager"
            binding:linearManager="@{LinearManagers.linear(@drawable/divider_bg_v_1dp)}" />
            
            
    val adapter=BindingRecyclerViewAdapter<ItemBottomMenuVM>()

    var itemBinding = ItemBinding.of<ItemBottomMenuVM>(BR.viewModel, R.layout.item_bottom_list_menu)

    val onBindingItemClickListener =
        object : BindingRecyclerViewAdapter.OnBindingItemClickListener<ItemBottomMenuVM> {
            override fun onItemClick(
                adapter: BindingRecyclerViewAdapter<ItemBottomMenuVM>,
                view: View?,
                item: ItemBottomMenuVM,
                position: Int
            ) {

            }
        }
        
      val onBindingItemLongClickListener =
        object : BindingRecyclerViewAdapter.OnBindingItemLongClickListener<ItemBottomMenuVM> {
            override fun onItemLongClick(
                adapter: BindingRecyclerViewAdapter<ItemBottomMenuVM>,
                view: View?,
                item: ItemBottomMenuVM,
                position: Int
            ): Boolean {
                
            }
        }
       
```
