---
title: Kotlin - 범위지정함수(Scoping functions)
---

date: 2020-02-29
categories: kotlin
---

Kotlin-scoping functions
-

**run, with, T.run, T.let, T.also, T.apply **

1. 정상 vs 확장
with 와 T.run 매우 비슷하지만 Nice한 방법은 아래와 같음.
상황에 따라 선별적으로 사용하는 조건을 알고 있어야 함.
null check가 필요하다면 run을 활용하자!

```kotlin
with(webview.settings) {
    javaScriptEnabled = true
    databaseEnabled = true
}

// similarly
webview.settings.run {
    javaScriptEnabled = true
    databaseEnabled = true
}
```
null 예외처리를 해야 한다면 아래와 같이 with가 아니라 T.run이 더 적합함을 알 수 있다.

```kotlin
with(webview.settings) {
      this?.javaScriptEnabled = true
      this?.databaseEnabled = true
}

// Nice.
webview.settings?.run {
    javaScriptEnabled = true
    databaseEnabled = true
}
```

2. This vs it argument (run과 let을 사용하는 방법)
T.run 과 T.let 비슷하지만 해당 객체를 직접 명시해서 사용할때는 T.let을 사용하는것이 효율적,
run은 아님.

```kotlin
stringVariable?.run {
      println("The length of this String is $length")
}

// Similarly.
stringVariable?.let {
      println("The length of this String is ${it.length}")
}
```

```kotlin
stringVariable?.let {
      nonNullString ->
      println("The non null string is $nonNullString")
}
```

3. Return this vs other type
T.let의 경우 let이후 it(value)가 변경됨을 알 수 있음.
T.alos는 also이후 it(value)가 처음과 동일하다. 동일 it을 가지고 이후 value참조
T.let 과 T.also

```kotlin
stringVariable?.let {
      println("The length of this String is ${it.length}")
}

// Exactly the same as below
stringVariable?.also {
      println("The length of this String is ${it.length}")
}
```

```kotlin
val original = "abc"
// Evolve the value and send to the next chain
original.let {
    println("The original String is $it") // "abc"
    it.reversed() // evolve it as parameter to send to next let
}.let {
    println("The reverse String is $it") // "cba"
    it.length  // can be evolve to other type
}.let {
    println("The length of the String is $it") // 3
}

// Wrong
// Same value is sent in the chain (printed answer is wrong)
original.also {
    println("The original String is $it") // "abc"
    it.reversed() // even if we evolve it, it is useless
}.also {
    println("The reverse String is ${it}") // "abc"
    it.length  // even if we evolve it, it is useless
}.also {
    println("The length of the String is ${it}") // "abc"
}

// Corrected for also (i.e. manipulate as original string
// Same value is sent in the chain 
original.also {
    println("The original String is $it") // "abc"
}.also {
    println("The reverse String is ${it.reversed()}") // "cba"
}.also {
    println("The length of the String is ${it.length}") // 3
}
```

T.also의 자점은 chain으로 묶을때 그 자체 감ㅅ을 유지하며 명시적으로 보여준다는 것에 있다. T.also와 T.let을 상황에 맞게 잘 쓰자!

```kotlin
// Normal approach
fun makeDir(path: String): File  {
    val result = File(path)
    result.mkdirs()
    return result
}

// Improved approach
fun makeDir(path: String) = path.let{ File(it) }.also{ it.mkdirs() }
```

4. T.apply 함수 : 확장함수, this사용(argument, return)

```kotlin
// Normal approach
fun createInstance(args: Bundle) : MyFragment {
    val fragment = MyFragment()
    fragment.arguments = args
    return fragment
}

// Improved approach
fun createInstance(args: Bundle) 
              = MyFragment().apply { arguments = args }
```         

unchaind object를 chain-able하게 만들 수 있다.

```kotlin
// Normal approach
fun createIntent(intentData: String, intentAction: String): Intent {
    val intent = Intent()
    intent.action = intentAction
    intent.data=Uri.parse(intentData)
    return intent
}

// Improved approach, chaining
fun createIntent(intentData: String, intentAction: String) =
        Intent().apply { action = intentAction }
                .apply { data = Uri.parse(intentData) }
```

출처: https://medium.com/@elye.project/mastering-kotlin-standard-functions-run-with-let-also-and-apply-9cd334b0ef84

 