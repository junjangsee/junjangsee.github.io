---
title: intelliJ에서 Gradle을 이용한 Kotlin 프로젝트 생성하기
date: 2020-01-02 00:48:19
tags: [intelliJ, Gradle, Kotlin, 코틀린]
---

![kotlin](/images/kotlin/kotlin.png)<br/>

# 프로젝트 생성

## Gradle로 프로젝트 생성하기
![kotlin](/images/kotlin/gradle/gra1.png) IntelliJ 메인 화면에서 `Create New Project`를 클릭합니다.<br/>
![kotlin](/images/kotlin/gradle/gra2.png) 좌측에 `Gradle`을 클릭하고 `Kotlin/JVM`을 선택 후 Next를 눌러 다음으로 이동합니다.<br/>
![kotlin](/images/kotlin/gradle/gra3.png) GroupId는 조직의 식별자이고 ArtifactId가 프로젝트 이름입니다.<br/>
![kotlin](/images/kotlin/gradle/gra4.png) Finish를 클릭해서 마무리를 해줍니다.<br/>
<br/>

## Kotlin 파일 생성하기

![kotlin](/images/kotlin/gradle/gra5.png) src 폴더 하위에 `kotlin`이라는 폴더가 생성된 것을 볼 수 있습니다.<br/>
![kotlin](/images/kotlin/gradle/gra7.png) 코틀린은 꼭 클래스로 생성하지 않고 파일로도 실행이 가능하기 때문에 `파일`로 생성해보겠습니다.<br/>
<br/>

## Hello World 출력하기
파일을 생성 하였다면 Hello World를 출력하는 코드를 작성하면 됩니다.
```
fun main(args: Array<String>) {
    println("Hello World");
}
```
![kotlin](/images/kotlin/gradle/gra8.png) 컴파일은 파일을 우클릭하거나 코드 좌측의 실행 버튼을 눌러 할 수 있습니다.<br/>
![kotlin](/images/kotlin/gradle/gra9.png) 컴파일이 성공적으로 완료되면 우상단에 실행된 코틀린 파일이 추가되며 Hello World가 출력된 것을 확인할 수 있습니다.<br/>
<br/>

## 마무리
이번 주제는 IntelliJ와 Gradle을 활용하여 Hello World를 출력해보았습니다.
간단하지만 처음 코틀린을 입문할 때 어떻게 생성해야할지 모를 때 참고하면 좋을 것 같습니다.