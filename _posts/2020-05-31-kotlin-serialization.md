---
title: "Kotlin - JSON Serialize 라이브러리(Kotlin Native Serialization Library)"
date: 2020-05-31
categories: kotlin
---

Kotlin-Serialization Library
-


serialize a JSON response
-

1.Kotlin에서 제공하는 Library
 JSON을 serialize 하는 라이브러리 제공
  Gson : Android only, reflection을 사용함으로 신규로 개발.

2.사용법

 build.gradle 
 
```
apply plugin: 'kotlinx-serialization'

//dependencies
classpath "org.jetbrains.kotlin:kotlin-serialization:$kotlin_version"

implementation "org.jetbrains.kotlinx:kotlinx-serialization-runtime:0.20.0"
```

일반 사용

```kotlin
@Serializable
data class Example(val data : String,
                         var optionalData : String = "test")
```

Retrofit2과 같이 사용.
 
```
//dependencies
implementation("com.jakewharton.retrofit:retrofit2-kotlinx-serialization-converter:0.5.0")
```

```kotlin
val contentType = "application/json".toMediaType()
val retrofit = Retrofit.Builder()
    .baseUrl(BuildConfig.BASE_URL)
    .addConverterFactory(serializationConverterFactory(contentType, JSON))
    .build()
```

추가 기능

```kotlin
@Serializable
data class Example(val data : String,
                         var optionalData : String = "test",
                         var complexClass: ComplexClass )
```


출처: https://medium.com/better-programming/why-and-how-to-use-kotlins-native-serialization-library-c88c0f14f93d
 