---
title: 코틀린(Kotlin) - 파일(File)
date: 2020-01-03 00:12:06
tags: [Kotlin, 코틀린, File, Class]
---

![kotlin](/images/kotlin/kotlin.png)<br/>

# 코틀린 파일 정의

## 일반 파일과 클래스 파일
코틀린은 확장자가 kt인 파일을 작성하여 개발합니다. 코틀린 파일을 만들 때 일반 파일과 클래스 파일을 구분해서 만들기는 하지만, 둘의 차이는 없으며 규칙 또한 없습니다.
중요한건 이 둘의 공통점은 **확장자**가 `kt`라는 것입니다.<br/>
![kotlin](/images/kotlin/file/file1.png) 생성된 두 개를 보시면 파일, 클래스로 생성되었지만 같은 kt 확장자를 가지고 있습니다.
현재 저는 IntelliJ를 사용하고 있기 때문에 보기 편하게 구별되어 있을 뿐 확장자는 무조건 `kt`입니다.<br/>
![kotlin](/images/kotlin/file/file2.png) 파일로 생성하더라도 클래스를 선언하게 되면 클래스로 바뀌는 모습입니다.<br/>
<br/>

## 파일의 구성요소
### 클래스
```
import java.util.*

class examClass {
    val name = "junjang"

    fun hello() {
        val date = Date()
        println("date" + date)
        println("hello()" + name)
    }
}
```
![kotlin](/images/kotlin/file/file5.png) 위 코드는 클래스에 변수와 함수가 선언된 모습입니다. 여기서는 `패키지, 임포트, 변수, 함수`가 포함됩니다.<br/>

### 파일
```
var add = 0

fun calAdd() {
    for (i in 1..10) {
        add += i
    }
}
```
![kotlin](/images/kotlin/file/file4.png) 클래스를 따로 선언하지 않고 파일 내에 변수와 함수를 선언할 수도 있습니다.<br/>
```
import java.util.*

var sum = 0

fun calSum() {
    for (i in 1..10) {
        sum += i
    }
}

class exam {
    val name = "junjang"

    fun hello() {
        val date = Date()
        println("date" + date)
        println("hello() " + name)
    }
}

fun main(args: Array<String>) {
    calSum()
    println(sum)
    exam().hello()
}

```
![kotlin](/images/kotlin/file/file6.png) 위 코드처럼 코틀린은 모든 구성요소를 꼭 클래스로 묶지 않아도 되며, 변수나 함수를클래스 외부에 선언할 수 있습니다.