---
layout:     post
title:      "gradle aliyun代理"
subtitle:   ""
date:       2020-03-12
author:     "Rain"
header-img: "img/post-bg-2020-03-12.jpg"
tags:
    - Android
    - Gradle
    - Gradle配置

---

众所周知，国内的网络环境，Android开发运行又依赖于Gradle，经常无法或者太慢下载google和jcenter的依赖资源，使用国内大厂的仓库比较快速，记录下阿里云的依赖仓库。

将Android项目中的build.gradle文件中的一些部分更换为：

```gradle
buildscript {
  repositories {
      	maven{ url 'https://maven.aliyun.com/repository/google'}
      	maven{ url 'https://maven.aliyun.com/repository/gradle-plugin'}
      	maven{ url 'https://maven.aliyun.com/repository/public'}
      	maven{ url 'https://maven.aliyun.com/repository/jcenter'}
      	google()
        jcenter()
    }
}

allprojects {
    repositories {
      	maven{ url 'https://maven.aliyun.com/repository/google'}
      	maven{ url 'https://maven.aliyun.com/repository/gradle-plugin'}
      	maven{ url 'https://maven.aliyun.com/repository/public'}
      	maven{ url 'https://maven.aliyun.com/repository/jcenter'}
        google()
        jcenter()
        // kotlin仓库
        maven {
            url "http://dl.bintray.com/kotlin/kotlin-eap"
        }
    }
}
```

## 更改android studio项目新建build.gradle模版

找到路径`安装目录/plugins/android/lib/templates/gradle-projects/NewAndroidProject/root`，更改build.gradle.flt文件