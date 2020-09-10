---

layout:     post
title:      "Android项目的gradle依赖路径记录"
subtitle:   "好记性不如烂笔头！"
date:       2020-03-14
author:     "Rain"
header-img: "img/post-bg-2020-03-14.jpg"
tags:
    - Android
    - Gradle
    - Gradle配置
    - Gradle依赖

---

# Contents

{:.no_toc}

* A markdown unordered list which will be replaced with the ToC, excluding the "Contents" from above
{:toc}


为了在添加依赖的时候，不再到处查找，所以用一个文档来记录下依赖的配置路径。持续更新～

## RecyclerView

```groovy
def recyclerViewVersion = "1.1.0"
implementation "androidx.recyclerview:recyclerview:$rootProject.recyclerViewVersion"
```

## LifeCycle-ViewModel-LiveData

```groovy
def lifecycleVersion = "2.2.0"
implementation "androidx.lifecycle:lifecycle-extensions:$rootProject.lifecycleVersion"
implementation "androidx.lifecycle:lifecycle-livedata-ktx:$rootProject.lifecycleVersion"
implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$rootProject.lifecycleVersion"
```

## Room

// 需要在build.gradle文件中加入`apply plugin: 'kotlin-kapt'`

```groovy
def roomVersion = "2.2.4"
// Room
implementation "androidx.room:room-runtime:$rootProject.room_version"
implementation "androidx.room:room-ktx:$rootProject.room_version"
kapt "androidx.room:room-compiler:$rootProject.room_version" // Java用annotationProcessor
```

## WorkManager

```groovy
def workVersion = "2.1.0"
implementation "androidx.work:work-runtime-ktx:$rootProject.workVersion"
```



## 示例

以下提供[sunflower](https://github.com/android/sunflower)截止到2020-03-14日的gradle的配置信息示例。

### project build.gradle

```groovy

/*
 * Copyright 2018 Google LLC
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     https://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

buildscript {
    // Define versions in a single place
    ext {
        // Sdk and tools
        compileSdkVersion = 28
        minSdkVersion = 21
        targetSdkVersion = 28

        // App dependencies
        appCompatVersion = '1.1.0'
        constraintLayoutVersion = '2.0.0-beta3'
        coreTestingVersion = '2.0.0'
        coroutinesVersion = "1.3.0-M2"
        espressoVersion = '3.1.1'
        fragmentVersion = '1.1.0-alpha09'
        glideVersion = '4.10.0'
        gradleVersion = '3.6.1'
        gsonVersion = '2.8.2'
        junitVersion = '4.12'
        kotlinVersion = '1.3.70'
        ktlintVersion = '0.33.0'
        ktxVersion = '1.0.2'
        lifecycleVersion = '2.2.0-alpha01'
        materialVersion = '1.1.0-alpha09'
        navigationVersion = '2.2.0'
        recyclerViewVersion = '1.1.0-alpha05'
        roomVersion = '2.1.0'
        runnerVersion = '1.0.1'
        truthVersion = '0.42'
        testExtJunit = '1.1.0'
        uiAutomatorVersion = '2.2.0'
        viewPagerVersion = '1.0.0'
        workVersion = '2.1.0'
    }

    repositories {
        maven{ url 'https://maven.aliyun.com/repository/google'}
        maven{ url 'https://maven.aliyun.com/repository/gradle-plugin'}
        maven{ url 'https://maven.aliyun.com/repository/public'}
        maven{ url 'https://maven.aliyun.com/repository/jcenter'}
        google()
        jcenter()
    }

    dependencies {
        classpath "com.android.tools.build:gradle:$gradleVersion"
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlinVersion"
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$navigationVersion"
    }
}

plugins {
    id "com.diffplug.gradle.spotless" version "3.24.0"
}

allprojects {
    repositories {
        maven{ url 'https://maven.aliyun.com/repository/google'}
        maven{ url 'https://maven.aliyun.com/repository/gradle-plugin'}
        maven{ url 'https://maven.aliyun.com/repository/public'}
        maven{ url 'https://maven.aliyun.com/repository/jcenter'}
        google()
        jcenter()
    }
}

spotless {
    kotlin {
        target "**/*.kt"
        ktlint(ktlintVersion).userData(['max_line_length' : '100'])
    }
}

```
### app build.gradle

```groovy
/*
 * Copyright 2018 Google LLC
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     https://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'
apply plugin: 'kotlin-kapt'
apply plugin: 'androidx.navigation.safeargs.kotlin'

android {
    compileSdkVersion rootProject.compileSdkVersion
    dataBinding {
        enabled = true
    }
    defaultConfig {
        applicationId "com.google.samples.apps.sunflower"
        minSdkVersion rootProject.minSdkVersion
        targetSdkVersion rootProject.targetSdkVersion
        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
        versionCode 1
        versionName "0.1.6"
        vectorDrawables.useSupportLibrary true
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    // work-runtime-ktx 2.1.0 and above now requires Java 8
    kotlinOptions {
        jvmTarget = "1.8"
    }
}

dependencies {
    kapt "androidx.room:room-compiler:$rootProject.roomVersion"
    kapt "com.github.bumptech.glide:compiler:$rootProject.glideVersion"
    implementation "androidx.appcompat:appcompat:$rootProject.appCompatVersion"
    implementation "androidx.constraintlayout:constraintlayout:$rootProject.constraintLayoutVersion"
    implementation "androidx.core:core-ktx:$rootProject.ktxVersion"
    implementation "androidx.fragment:fragment-ktx:$rootProject.fragmentVersion"
    implementation "androidx.lifecycle:lifecycle-extensions:$rootProject.lifecycleVersion"
    implementation "androidx.lifecycle:lifecycle-livedata-ktx:$rootProject.lifecycleVersion"
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$rootProject.lifecycleVersion"
    implementation "androidx.navigation:navigation-fragment-ktx:$rootProject.navigationVersion"
    implementation "androidx.navigation:navigation-ui-ktx:$rootProject.navigationVersion"
    implementation "androidx.recyclerview:recyclerview:$rootProject.recyclerViewVersion"
    implementation "androidx.room:room-runtime:$rootProject.roomVersion"
    implementation "androidx.room:room-ktx:$rootProject.roomVersion"
    implementation "androidx.viewpager2:viewpager2:$rootProject.viewPagerVersion"
    implementation "androidx.work:work-runtime-ktx:$rootProject.workVersion"
    implementation "com.github.bumptech.glide:glide:$rootProject.glideVersion"
    implementation "com.google.android.material:material:$rootProject.materialVersion"
    implementation "com.google.code.gson:gson:$rootProject.gsonVersion"
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk8:$rootProject.kotlinVersion"
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:$rootProject.coroutinesVersion"
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:$rootProject.coroutinesVersion"

    // Testing dependencies
    androidTestImplementation "androidx.arch.core:core-testing:$rootProject.coreTestingVersion"
    androidTestImplementation "androidx.test.espresso:espresso-contrib:$rootProject.espressoVersion"
    androidTestImplementation "androidx.test.espresso:espresso-core:$rootProject.espressoVersion"
    androidTestImplementation "androidx.test.espresso:espresso-intents:$rootProject.espressoVersion"
    androidTestImplementation "androidx.test.ext:junit:$rootProject.testExtJunit"
    androidTestImplementation "androidx.test.uiautomator:uiautomator:$rootProject.uiAutomatorVersion"
    androidTestImplementation "androidx.work:work-testing:$rootProject.workVersion"
    androidTestImplementation "com.google.truth:truth:$rootProject.truthVersion"
    testImplementation "junit:junit:$rootProject.junitVersion"
}

```

