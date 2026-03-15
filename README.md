# Android SearchView 搜索演示

## 简介

本 Demo 演示 SearchView 的基本用法。

## 教程

```kotlin
searchView.setOnQueryTextListener(object : SearchView.OnQueryTextListener {
    override fun onQueryTextChange(newText: String?): Boolean {
        adapter.filter.filter(newText)
        return true
    }
})
```
