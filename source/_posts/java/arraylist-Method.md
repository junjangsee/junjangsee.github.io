---
title: '[JAVA - 자료구조] ArrayList 메소드'
date: 2019-07-25 19:57:39
tags: [java, ArrayList, List, Method]
---

![images](/images/java/ArrayList.png)<br/>

# ArrayList
`ArrayList`란 자료구조의 한 종류로서 Java에서 가장 많이 사용되는 데이터 스트럭쳐입니다.
알고리즘에서 많이 활용되며, 실무에서 데이터를 다룰 때 입출력하는 부분에서 매우 많은 비중을 차지하고 있습니다.
하지만 이런 ArrayList를 다룰 때 지원하는 메소드를 숙지하고 있지 못하면 ArrayList의 성능을 제대로 활용할 수 없겠죠? 😱 <br/>
그래서 심화적인 부분은 아니지만 메소드를 활용할 수 있도록 익혀놓는다면 필요할 때 떠올려 관련 자료를 찾아보며 해결할 수 있는 능력이 생길테고, 나중엔 본인 것으로 자연스럽게 남아 언젠간 ArrayList에 통달할 수 있겠습니다!<br/>
작성자인 저 또한 공부하면서 정리하는 부분이기 때문에 틀린 정보가 있을 수 있으니 혹시 틀린점이 있다면 피드백 부탁드립니다. :)<br/>
참고자료 : [ArrayList (Java 8)](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)
<br/>

## ArrayList 생성
```java
import java.util.ArrayList;
ArrayList<Integer> numbers = new ArrayList<>();
```
ArrayList를 사용하려면 객체를 생성해야합니다. import는 `java.util.ArrayList`로 하면 되겠습니다.
타입은 Integer로 선언하였습니다.<br/>
<br/>

## add(E e)
ArrayList 객체가 생성되어 있다면 값이 있어야겠죠? 만약 값이 없는 채로 출력되면 텅텅 빈 엘리먼트만 나올 것입니다.
```java
    numbers.add(10);
    numbers.add(20);
    numbers.add(30);
    numbers.add(40);
    numbers.add(50);
```
생성한 객체에 `add`로 Integer 타입을 넣어주면 numbers에는 총 5개의 엘리먼트가 들어가게 됩니다. <br/>
그리고 출력을 하면..
<br/>
> [10, 20, 30, 40, 50]

위 결과가 나오겠죠?
여기서 알 수 있는건 바로 .add() 한 `순서대로` 엘리먼트가 들어간 것을 알 수 있습니다.
그러면 저 사이에 값을 넣는건 불가능 할까요? 😭
**NOPE!!** 다행히도 `index`를 기준으로 add가 가능합니다. 🎉<br/>
<br/>

## add(int index, E element)
기존 총 5개의 엘리먼트 중 2번째에 15라는 값을 넣어보겠습니다.
```java
    numbers.add(1, 15);
```
index는 0부터 시작하고 1에 15라는 엘리먼트를 넣겠다는 의미입니다.
그렇다면 결과는..
<br/>
> [10, 15, 20, 30, 40, 50]

위와 같이 나오게 됩니다.
이제 각 하나의 엘리먼트 들을 넣는 방법은 알았습니다.<br/>
그렇다면 하나의 ArrayList에 또 다른 ArrayList의 엘리먼트들을 넣고싶을 땐 어떻게 해야할까요?
바로 `addAll()`을 이용하는 방법입니다.<br/>
<br/>

## addAll(Collection<? extends E> c)
여기서 Collection은 현재 사용중인 ArrayList와 같은 자료구조를 뜻합니다.
이 메소드는 위에 설명한 것과 같이 하나의 자료구조에 또 다른 자료구조의 엘리먼트를 넣는데에 사용됩니다.
```java
ArrayList<Integer> newNumbers = new ArrayList<>();

    newNumbers.add(60);
    newNumbers.add(70);
    newNumbers.add(80);
    newNumbers.add(90);
    newNumbers.add(100);

    numbers.addAll(newNumbers);
```
기존 numbers에 새롭게 만든 ArrayList를 addAll() 합니다.
<br/>
> [10, 15, 20, 30, 40, 50, 60, 70, 80, 90, 100]

그럼 위 엘리먼트들을 얻을 수 있을 것입니다.
여기서 유추할 수 있는 것은 add()도 마찬가지고 `index`를 기준으로 addAll()이 가능합니다.<br/>
<br/>

## addAll(int index, Collection<? extends E> c)
index를 명시하여 해당 공간에 addAll()이 가능합니다.
```java
ArrayList<Integer> newNumbers = new ArrayList<>();

    newNumbers.add(60);
    newNumbers.add(70);
    newNumbers.add(80);
    newNumbers.add(90);
    newNumbers.add(100);

    numbers.addAll(2 , newNumbers);

    numbers.addAll(newNumbers);
```
add()와 다른 점은 메소드 이름의 차이 뿐입니다.
<br/>
> [10, 15, 60, 70, 80, 90, 100, 20, 30, 40, 50]

2번 째 index에 addAll() 된 것을 볼 수 있습니다.
그렇다면 넣은 엘리먼트들을 싹다 빼는 방법에 대해 알아보겠습니다.<br/>
<br/>

## clear()
이 메소드는 이전에 무슨 작업을 하건 말건 상관없이 무조건 **깔.끔.** 하게 `비워`줍니다.
```java
ArrayList<Integer> numbers = new ArrayList<>();

        numbers.add(10);
        numbers.add(20);
        numbers.add(30);
        numbers.add(40);
        numbers.add(50);

        numbers.clear();
```
엘리먼트가 넣어진 numbers에 clear()가 선언된 모습 (곧 게워낼 예정..)
<br/>
> []

그 전까지 부자였던 numbers가 빈털털이가 된 모습... 💀<br/>
<br/>

## clone()
clear()가 시원하게 게워냈다면 이 메소드는 있던걸 **풍.만.** 하게 `복제`합니다.
```java
ArrayList<Integer> numbers = new ArrayList<>();

        numbers.add(10);
        numbers.add(20);
        numbers.add(30);
        numbers.add(40);
        numbers.add(50);

        numbers.add(1, 15);

ArrayList<Integer> newNumbers = (ArrayList<Integer>) numbers.clone();
```
clone() 하기 위해 새롭게 만든 ArrayList에 기존 ArrayList를 clone() 합니다.
<br/>
> [10, 15, 20, 30, 40, 50]

numbers에 있던 엘리먼트가 똑같이 newNumbers로 복제된 모습입니다.<br/>
<br/>

## contains(Object o)
`contains()`는 어떤 구체적인 객체가 있는지 확인하는 메소드입니다.

```java
ArrayList<Integer> newNumbers = new ArrayList<>();

    newNumbers.add(60);
    newNumbers.add(70);
    newNumbers.add(80);
    newNumbers.add(90);
    newNumbers.add(100);

    numbers.addAll(newNumbers);

    if (numbers.contains(100)) {
        System.out.println("100을 가지고 있습니다.");
    } else {
        System.out.println("100을 가지고있지 않습니다.");
    }
```
numbers에 100이라는 엘리먼트가 포함되어 있다면 문장을 출력하는 예제입니다.
당연히 객체는 존재할 것이고 100도 존재하니 아래와 같은 값이 나오겠죠?
<br/>
> 100을 가지고 있습니다.

만약 contains에 없는 객체를 넣었다면 else문을 탄 값이 출력될 것입니다.<br/>
<br/>

## forEach(Consumer<? super E> action)
`forEach`는 for-loop를 좀 더 효율적으로 사용하는 방법으로 Java8에서는 람다식으로 표현이 가능합니다.

```java
numbers.forEach(number -> System.out.println(number));
```
number 변수를 선언하여 람다식으로 출력하였습니다.
<br/>
> 10 15 20 30 40 50 60 70 80 90 100

정말 간단하게 for-loop를 구현할 수 있습니다.<br/>
<br/>

## get(int index)
`get()`은 ArrayList 내부의 엘리먼트를 가져올 수 있습니다.

```java
if (!numbers.isEmpty()) {
    System.out.println("첫 번째 엘리먼트 값 : " + numbers.get(0));
}
```
numbers가 비어있지 않다면 0번 인덱스를 출력하는 예제입니다.
여기서 isEmpty()는 아직 몰라도 되는 부분입니다. 추후에 나오게 됩니다. 😀
<br/>
> 첫 번째 엘리먼트 값 : 10

ArrayList의 0번 째에 위치해있는 엘리먼트가 출력됩니다.<br/>
<br/>

## indexOf(Object o)
`indexOf()`는 ArrayList 안에있는 엘리먼트가 어디에 있는지 찾기 위해 사용하는 메소드 입니다.
```java
ArrayList<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);
    numbers.add(40);
    numbers.add(50);

    numbers.add(1, 15);

ArrayList<Integer> newNumbers = new ArrayList<>();
        
    newNumbers.add(60);
    newNumbers.add(70);
    newNumbers.add(80);
    newNumbers.add(90);
    newNumbers.add(100);

    numbers.addAll(newNumbers);

    System.out.println(numbers.indexOf(30));
```
총 11개의 엘리먼트 중, 30에 대한 위치를 찾기 위한 예제입니다.
<br/>
> 3

30은 4번 째에 위치하고 있기 때문에 3이 출력됩니다.<br/>
<br/>

## isEmpty()