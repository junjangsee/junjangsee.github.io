---
title: '[JAVA - 자료구조] HashMap 메소드'
date: 2019-09-18 19:28:46
tags: [java, HashMap, Map, Method]
---

![images](/images/java/hashmap.png)<br/>

# HashMap
`HashMap`이란 자료구조의 한 종류로서 **Key**와 **value**를 묶어 하나의 entry로 저장한다는 특징을 가집니다.
그리고 hashing을 사용하기 때문에 많은양의 데이터를 검색하는데 뛰어난 성능을 보입니다.
이전 포스팅에서는 ArrayList에 대해서 다루었었는데 이와 동등하게 중요하고 자주 사용되는 자료구조인 HashMap을 살펴보도록 하겠습니다.
작성자인 저 또한 공부하면서 정리하는 부분이기 때문에 틀린 정보가 있을 수 있으니 혹시 틀린점이 있다면 피드백 부탁드립니다. :)<br/>
참고자료 : [HashMap (Java 8)](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)
<br/>

## HashMap 생성
```java
import java.util.HashMap;
HashMap<String , Integer> map = new HashMap<String , Integer>();
```
HashMap을 사용하려면 `객체`를 생성해야합니다. import는 `java.util.HashMap`로 하면 되겠습니다.
타입은 String, Integer로 선언하였습니다.<br/>
<br/>

## put(K key, V value)
HashMap에 Key, Value을 `삽입`하는 메소드입니다.
```java
map.put("신논현", 1);
map.put("강남", 2);
map.put("혜화", 3);
map.put("안양", 4);
map.put("수원", 5);
```
각 Key, Value에 대한 타입에 맞게 선헌하면 map 객체에 저장됩니다.<br/>
<br/>

## putAll(Map m)
하나의 Map을 또 다른 Map에 `추가`하는데 사용됩니다.
```java
        HashMap<String , Integer> map = new HashMap<String , Integer>();

            map.put("신논현", 1);
            map.put("강남", 2);
            map.put("혜화", 3);
            map.put("안양", 4);
            map.put("수원", 5);

        HashMap<String , Integer> map2 = new HashMap<String , Integer>();

            map.put("천안", 6);
            map.put("양평", 7);
            map.put("장성", 8);
            map.put("파주", 9);
            map.put("부산", 10);

            map.putAll(map2);
```
map에 map2를 넣어주게 되면 map2있던 데이터가 map으로 저장되게 됩니다.<br/>
<br/>

## remove(Object Key)
하나의 `Key`를 입력하여 데이터를 `삭제`합니다.
```java
HashMap<String , Integer> map = new HashMap<String , Integer>();

    map.put("신논현", 1);
    map.put("강남", 2);
    map.put("혜화", 3);
    map.put("안양", 4);
    map.put("수원", 5);

    map.remove("신논현");
```
remove하게 되면 신논현 이라는 String 타입의 Key를 가진 데이터는 삭제됩니다.<br/>
<br/>

## size()
HashMap에 저장된 엘리먼트의 `개수`를 반환합니다.
```java
HashMap<String , Integer> map = new HashMap<String , Integer>();

    map.put("신논현", 1);
    map.put("강남", 2);
    map.put("혜화", 3);
    map.put("안양", 4);
    map.put("수원", 5);

    System.out.println(map.size());
```
size를 사용하여 총 5개인 것을 확인할 수 있습니다.<br/>
<br/>

## values()
HashMap에 저장된 엘리먼트들을 전부 `컬렉션` 형태로 출력합니다.
```java
HashMap<String , Integer> map = new HashMap<String , Integer>();

    map.put("신논현", 1);
    map.put("강남", 2);
    map.put("혜화", 3);
    map.put("안양", 4);
    map.put("수원", 5);

    System.out.println(map.values());
```
values를 사용하여 총 5개의 엘리먼트가 `[]` 형태로 출력되는 것을 확인할 수 있습니다.<br/>
<br/>

## isEmpty()
isEmpty는 컬렉션에서 엘리먼트의 `유무`를 체크합니다.
```java
    HashMap<String , Integer> map = new HashMap<String , Integer>();

        map.put("신논현", 1);
        map.put("강남", 2);
        map.put("혜화", 3);
        map.put("안양", 4);
        map.put("수원", 5);

        if (map.isEmpty()) {
            System.out.println("엘리먼트가 없습니다.");
        } else {
            System.out.println("엘리먼트가 있습니다.");
        }
```
위 코드에서는 map에 엘리먼트가 있으니 else문을 탈 것입니다. 더 나아가 `null`체크도 가능하겠죠?<br/>
<br/>

## get(Object Key)
get은 `Key`값에 대한 `엘리먼트`를 가져옵니다.
```java
    HashMap<String , Integer> map = new HashMap<String , Integer>();

        map.put("신논현", 1);
        map.put("강남", 2);
        map.put("혜화", 3);
        map.put("안양", 4);
        map.put("수원", 5);

        System.out.println(map.get("강남"));
```
**강남**인 Key를 가져와 Value를 출력합니다.<br/>
<br/>

## clear()
map에 있는 엘리먼트를 깔끔하게 `비워`냅니다.
```java
    HashMap<String , Integer> map = new HashMap<String , Integer>();

        map.put("신논현", 1);
        map.put("강남", 2);
        map.put("혜화", 3);
        map.put("안양", 4);
        map.put("수원", 5);

        map.clear();
```
clear를 하는 순간 map에 있던 엘리먼트는 모조리 사라집니다.<br/>
<br/>

## clone()
clone은 해당 map의 엘리먼트드를 `복제`합니다.
```java
    HashMap<String , Integer> map = new HashMap<String , Integer>();

        map.put("신논현", 1);
        map.put("강남", 2);
        map.put("혜화", 3);
        map.put("안양", 4);
        map.put("수원", 5);

    HashMap<String, Integer> map2 = (HashMap<String, Integer>) map.clone();
```
map2에 기존 map을 clone하여 주었습니다. 즉, map에 있는 엘리먼트는 map2도 가지고 있게됩니다.<br/>
<br/>

## containsKey(Object Key)
map의 엘리먼트 중 `Key`를 포함하고 있는지 여부를 확인합니다.
```java
    HashMap<String , Integer> map = new HashMap<String , Integer>();

        map.put("신논현", 1);
        map.put("강남", 2);
        map.put("혜화", 3);
        map.put("안양", 4);
        map.put("수원", 5);

        System.out.println(map.containsKey("혜화"));
```
만약 엘리먼트 내에 Key가 포함되어있다면 `boolean` 값으로 true, 아니라면 false가 리턴됩니다.<br/>
<br/>

## containsValue(Object Value)
map의 엘리먼트 중 `Value`를 포함하고 있는지 여부를 확인합니다.
```java
    HashMap<String , Integer> map = new HashMap<String , Integer>();

        map.put("신논현", 1);
        map.put("강남", 2);
        map.put("혜화", 3);
        map.put("안양", 4);
        map.put("수원", 5);

        System.out.println(map.containsValue(5));
```
containsKey와 마찬가지로 엘리먼트 내에 Value가 포함되어있다면 `boolean` 값으로 true, 아니라면 false가 리턴됩니다.<br/>
<br/>

## Set entrySet()
HashMap에 저장된 Key, Value값을 엔트리(키와 값을 결합)의 형태로 `Set`에 저장하여 반환합니다.
```java
    HashMap<String , Integer> map = new HashMap<String , Integer>();

        map.put("신논현", 1);
        map.put("강남", 2);
        map.put("혜화", 3);
        map.put("안양", 4);
        map.put("수원", 5);

        Set set = map.entrySet();

        System.out.println(set);
```
Set의 형태로 `[]` 내부에 값이 출력됩니다.<br/>
<br/>