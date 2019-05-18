---
title: SpringBoot - 의존성관리
date: 2019-04-16 10:59:45
tags: [SpringBoot, parent]
---

![images](/images/springboot/springboot.png)<br/>
# SpringBoot 의존성 관리의 원리

## parent
'parent' 라는 의존성을 넣으면 더 상위 의존성을 대려와 알아서 필요한 의존성 내용들을 추가해줍니다.
<br/>
![dependency](/images/springboot/dependency/de1.png) pom.xml에 추가된 parent입니다.<br/>
![dependency](/images/springboot/dependency/de2.png) parent 내용으로 들어가보면 상위 부모인 spring-boot-dependencies가 나타납니다.<br/>
![dependency](/images/springboot/dependency/de3.png) 한 번 더 상위로 이동하면 최상위 의존성이 등장합니다.<br/>
![dependency](/images/springboot/dependency/de4.png) 최상위 부모에서는 위와 같이 버전은 한꺼번에 관리하고 있습니다.<br/>
![dependency](/images/springboot/dependency/de5.png) 또한 Maven에 해당되는 의존성들을 한 번에 볼 수도 있습니다.
<br/>
## 의존성 관리를 하면 좋은점은?
1. 상위 부모가 알아서 관리를 해주기 때문에 직접 관리하는 의존성이 줄어듭니다.(우리의 일이 줄어듬)
2. 버전 수정을 하게 될 일이 있더라고 라이브러리 버전을 따로 맞춰줄 필요가 없습니다.

<br/>
## 만약 parent를 사용하지 않는다면?
```xml
<dependencyManagement>
		<dependencies>
		<dependency>
			<!-- Import dependency management from Spring Boot -->
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-dependencies</artifactId>
			<version>2.1.1.RELEASE</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
	</dependencies>
</dependencyManagement>
```
위 의존성으로 대체할 수 있지만 parent 자체에 정의되어있는 고유 의존성 기능들을 온전히 사용할 수 없기 때문에 parent로 사용하는 것을 **추천**합니다.
<br/>
# SpringBoot 의존성 관리 활용하기
이러한 의존성들을 어떻게 하면 활용할 수 있을지 여러가지 방법을 통해서 알아봅시다.
<br/>
## SpringBoot가 버전 관리를 하는 의존성 추가 예시 
![dependency](/images/springboot/dependency/de6.png) JPA를 pom.xml에 추가합니다. 좌상단에 보이는 아이콘이 나타난 의미는 버전 관리를 자동으로 해주고 있다는 표시입니다.<br/>
![dependency](/images/springboot/dependency/de7.png) 상위 parent에 버전관리가 되어 있는 것을 볼 수 있습니다.<br/>
![dependency](/images/springboot/dependency/de8.png) Maven에서 확인해보면 의존성이 추가된 것을 볼 수 있습니다.<br/>
<br/>
## SpringBoot가 버전 관리를 안하는 의존성 추가 예시 
![dependency](/images/springboot/dependency/de9.png) [라이브러리 검색](https://mvnrepository.com/)에서 필요한 라이브러리를 찾아 복사합니다.<br/>
![dependency](/images/springboot/dependency/de10.png) JPA와는 다르게 버전이 있으며 좌상단에 아이콘이 없습니다.<br/>
<br/>
## SpringBoot의 자체 의존성을 변경하는 예시(Spring버전)
![dependency](/images/springboot/dependency/de11.png) 최상위 parent에서 스프링 버전을 확인합니다.<br/>
![dependency](/images/springboot/dependency/de12.png) pom.xml에 **properties**태그로 원하는 버전으로 수정합니다.
기존에 있었던 버전을 pom.xml에서 **오버라이딩(재정의)**하여 새롭게 의존성을 주입해주는 것입니다. 스프링 버전 수정 뿐만 아니라 모든 버전 및 의존성에 적용 가능합니다.<br/>
![dependency](/images/springboot/dependency/de13.png) maven 라이브러리에서 수정된 것을 확인할 수 있습니다.<br/>
<br/>
## Gradle을 사용하여 의존성을 변경하는 방법
```
ext['foo.version'] = '1.1.0.RELEASE'
```
build.gradle 파일에 ext 문법을 사용하여 변경합니다.
혹은 gradle.properties 파일에
```
foo.version=1.1.0.RELEASE
```
위 문법을 사용하여 변경하면 됩니다.<br/>