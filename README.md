# Android SearchView 搜索演示

## 简介

本 Demo 演示 SearchView 的基本用法，展示如何在 Android 中实现搜索功能。

## 基本原理

SearchView 是 Android 提供的搜索组件，通常嵌入在 Toolbar 中使用。它提供了完整的搜索界面，包括输入框、搜索按钮、清除按钮等。

SearchView 的核心功能：
- 文本输入和搜索提交
- 实时搜索建议
- 与 ListView/RecyclerView 配合实现过滤

## 启动和使用

### 环境要求
- Android Studio
- JDK 17
- Gradle 8.x

### 安装和运行

1. 用 Android Studio 打开项目
2. 连接 Android 设备或模拟器
3. 点击 Run 运行

### 使用方法
- 在搜索框中输入文字，列表会实时过滤显示匹配的项

## 教程

### 什么是 SearchView？

SearchView 是 Android 提供的搜索组件，提供了一个可展开的搜索界面。它可以放在 Toolbar 中，也可以单独使用。

SearchView 的特点：
- 内置搜索图标和输入框
- 支持提交搜索和实时搜索
- 支持搜索建议
- 可以与 ListView 配合实现搜索过滤

### 基本用法

1. 在布局中使用 SearchView：

```xml
<SearchView
    android:id="@+id/searchView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

2. 设置搜索监听器：

```kotlin
searchView.setOnQueryTextListener(object : SearchView.OnQueryTextListener {
    // 用户提交搜索时调用
    override fun onQueryTextSubmit(query: String?): Boolean {
        return true
    }

    // 搜索文本变化时调用
    override fun onQueryTextChange(newText: String?): Boolean {
        adapter.filter.filter(newText)
        return true
    }
})
```

### ArrayAdapter 的过滤

ArrayAdapter 自带 Filter 功能，可以直接用于搜索过滤：

```kotlin
val adapter = ArrayAdapter(this, android.R.layout.simple_list_item_1, items)

// 调用 filter 方法进行过滤
adapter.filter.filter(searchText)
```

### 注意事项

1. **filter 回调**：在 onQueryTextChange 中调用 filter
2. **返回 true**：表示已处理事件
3. **性能**：大数据量时考虑使用 SearchViewSuggestionsProvider

## 关键代码详解

### MainActivity.kt

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // 1. 获取 SearchView 和 ListView 组件
        val searchView = findViewById<SearchView>(R.id.searchView)
        val listView = findViewById<ListView>(R.id.listView)

        // 2. 准备数据列表
        val items = listOf("苹果", "香蕉", "橙子", "葡萄", "西瓜")

        // 3. 创建 Adapter
        val adapter = ArrayAdapter(
            this,
            android.R.layout.simple_list_item_1,
            items
        )

        // 4. 将 Adapter 设置到 ListView
        listView.adapter = adapter

        // 5. 设置搜索监听器
        searchView.setOnQueryTextListener(object : SearchView.OnQueryTextListener {
            // 用户点击搜索按钮时调用
            // 返回 false 表示不处理，使用默认行为
            override fun onQueryTextSubmit(query: String?): Boolean = false

            // 搜索文本变化时调用
            // 实现实时搜索过滤
            override fun onQueryTextChange(newText: String?): Boolean {
                // 使用 ArrayAdapter 的 filter 方法过滤数据
                adapter.filter.filter(newText)
                return true  // 返回 true 表示已处理
            }
        })
    }
}
```

### activity_main.xml

```xml
<!-- 根布局：垂直线性布局 -->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <!-- SearchView 搜索组件 -->
    <SearchView
        android:id="@+id/searchView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

    <!-- ListView 显示列表 -->
    <ListView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_marginTop="16dp" />
</LinearLayout>
```
