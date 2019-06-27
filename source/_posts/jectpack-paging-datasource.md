---
title: jectpack-paging-datasource
date: 2019-06-06 15:28:29
tags: ['jetpack','paging']
---

## 数据源 DataSource
分页的数据都取之于数据源，我们需要实现对应的获取方法，PagedList会从DataSource中获取数据。

### 数据源的类型
1. ItemKeyedDataSource 每次加载数据需要根据之前加载的数据的ID来进行加载（`select * form video where id > ?? limit ??`）
```kotlin
class VideoDataSource : ItemKeyedDataSource<Int, Video>() {

    override fun getKey(item: Video): Int = item.id

    override fun loadInitial(params: LoadInitialParams<Int>, callback: LoadInitialCallback<Video>) {

        // 初次加载数据

        Api.getVideos(
            // 加载后面的数据
            loadMode = "after",
            // 起始的视频ID（我们要获取这个视频后面的视频），如果没有那么默认为0从头开始
            currentVideoId = params.requestedInitialKey,
            // 需要获取的数据量
            requestSize = params.requestedLoadSize
        ).onSuccess { videos ->
            // 加载成功后使用callback.onResult把加载的数据传递出去
            callback.onResult(videos)
        }
    }

    override fun loadAfter(params: LoadParams<Int>, callback: LoadCallback<Video>) {

        //  向前加载数据（类似上一页）

        Api.getVideos(
            // 加载前面的数据
            loadMode = "before",
            // 当前最前面的视频ID（我们要获取这个视频前面的视频）
            currentVideoId = params.key,
            // 需要获取的数据量
            requestSize = params.requestedLoadSize
        ).onSuccess { videos ->
            // 加载成功后使用callback.onResult把加载的数据传递出去
            callback.onResult(videos)
        }
    }

    override fun loadBefore(params: LoadParams<Int>, callback: LoadCallback<Video>) {

        //  向后加载数据（类似下一页）

        Api.getVideos(
            // 加载前面的数据
            loadMode = "after",
            // 当前最后面的视频ID（我们要获取这个视频后面的视频）
            currentVideoId = params.key,
            // 需要获取的数据量
            requestSize = params.requestedLoadSize
        ).onSuccess { videos ->
            // 加载成功后使用callback.onResult把加载的数据传递出去
            callback.onResult(videos)
        }
    }
}
```

2. PageKeyedDataSource 经典的上一页，下一页分页（类似数据根据页码来获取）
....
....
等待完善