---
title: StudyRoom(SRoom) 프로젝트 - 03(AWS EC2에 Git 설치 및 프로젝트 Clone 하기)
date: 2019-03-19 13:55:34
tags: AWS
---

# SRoom 프로젝트

![images](/images/sroom/studyroom.jpg)<br/>

## EC2에 Git 설치 및 프로젝트 Clone
배포하기 전 Git과 Java를 설치합니다.

<br/>
### Java 8 설치하기

```
ssh Host이름
```
![ec2](/images/sroom/git1.png) 먼저 EC2에 접속하여 줍니다.<br/>
그리고 Java8버전으로 업그레이드 하겠습니다.
```
sudo yum install -y java-1.8.0-openjdk-devel.x86_64
```
위의 명령어를 사용하여 Java8로 업그레이드 합니다.<br/>
![ec2](/images/sroom/git1-1.png) 설치되면 위와 같은 화면이 출력됩니다.<br/>
설치가 완료되었으면 인스턴스의 Java 버전을 8로 변경합니다.
```
sudo /usr/sbin/alternatives --config java
```
위의 명령어를 실행합니다.<br/>
![ec2](/images/sroom/git1-2.png) 위의 화면에서 1.8 버전인 2번을 선택하면 8버전으로 바뀝니다.<br/>
```
sudo yum remove java-1.7.0-openjdk
```
위의 명령어를 통해 기존에 존재하던 Java7을 삭제합니다.<br/>
![ec2](/images/sroom/git1-3.png) 삭제된 것을 확인할 수 있습니다.<br/>
```
java -version
```
위 명령어를 작성하여 최종적으로 버전이 적용되었는지 확인합니다.<br/>
<br/>
### Git 설치 및 Clone
Java8 설치가 끝났으면 Git을 설치합니다.
```
ssh Host이름
```
기존과 동일한 과정으로 ssh에 접속합니다.<br/>
이제 Git을 설치해보겠습니다.
```
sudo yum install git
```
위 명령어를 통하여 git을 설치합니다.<br/>
![git](/images/sroom/git2.png) 설치된 화면입니다.<br/>
```
git --version
```
설치가 되었다면 위 명령어를 이용해 버전 확인을 합니다.
<br/>
git이 성공적으로 설치 되었다면 이제 Git에 Clone할 디렉토리를 생성해야합니다.
```
mkdir app
mkdir app/git
```
위 명령어로 생성합니다.<br/>
```
cd ~/app/git
```
![git](/images/sroom/git2-2.png) 위 명령어로 git 디렉토리로 이동합니다.<br/>
```
git clone SSH주소
```
위 명령어를 통해 git을 clone합니다.
여기서 혹시 SSH로 등록이 안된다면 SSH Key가 없어서 그러는 현상이니 KEY를 등록하고 clone하면 되겠습니다.<br/>
![git](/images/sroom/git2-3.png) git clone하면 위와같이 출력됩니다.<br/>
```
cd 프로젝트명
```
위 명령어로 clone한 폴더로 들어가 **ll**, **ls -al** 명령어를 통해 복사되었는지 확인합니다.<br/>
![git](/images/sroom/git2-4.png) 제대로 clone된 것을 볼 수 있습니다.<br/>
TIP : **git branch -a** 명령어를 통해 현재 브랜치도 확인이 가능합니다.<br/>
<br/>
이제 프로젝트가 잘 받아졌는지 **테스트**를 해보겠습니다.<br/>
```
git pull
```
위 명령어를 이용하여 **pull**을 합니다.<br/>
```
./gradlew test
```
위 명령어를 통해 테스트를 진행합니다.<br/>
![git](/images/sroom/git2-5.png) 성공적으로 test 된 것을 볼 수 있습니다.<br/>
<br/>
## 배포 스크립트 생성하기
**배포**란? 작성한 코드를 실제 서버에 반영하는 것을 말합니다.
하지만 배포를 하기 위해서 개발자가 일일히 명령어를 작성하여 실행하기엔 현실적으로 힘든 부분이 있기 때문에 스크립트를 생성하여 한 번에 해결하도록 만들어보려고 합니다.

### deploy 스크립트 생성하기
EC2 인스턴스의 **~/app/git/**에 deploy.sh 파일을 생성합니다.
```
vim ~/app/git/deploy.sh
```
![src](/images/sroom/scr1.png) 혹시 잘못 생성하셨다면...
```
rm -f 잘못설정한파일이름
```
위 명령어를 입력하여 삭제합니다.<br/>
그리고 아래의 코드를 추가합니다.

```
#!/bin/bash

REPOSITORY=/home/ec2-user/app/git

cd $REPOSITORY/sroom-api/

echo "> Git Pull"

git pull

echo "> 프로젝트 Build 시작"

./gradlew build

echo "> Build 파일 복사"

cp ./build/libs/*.jar $REPOSITORY/

echo "> 현재 구동중인 애플리케이션 pid 확인"

CURRENT_PID=$(pgrep -f sroom)

echo "$CURRENT_PID"

if [ -z $CURRENT_PID ]; then
    echo "> 현재 구동중인 애플리케이션이 없으므로 종료하지 않습니다."
else
    echo "> kill -15 $CURRENT_PID"
    kill -15 $CURRENT_PID
    sleep 5
fi

echo "> 새 어플리케이션 배포"

JAR_NAME=$(ls $REPOSITORY/ |grep 'sroom-0' | tail -n 1)

echo "> JAR Name: $JAR_NAME"

nohup java -jar $REPOSITORY/$JAR_NAME &
```
추가 후 **:wq** 를 통해 저장 및 종료를 하였다면 clone 했던 git폴더로 이동합니다.
위의 코드들은 아래 사진으로 정리했습니다.<br/>
![src](/images/sroom/scr2.png) 위에 사진에 대한 설명입니다. (jojoldu님의 블로그에서 가져왔습니다.)
* REPOSITORY=/home/ec2-user/app/git
 * 프로젝트 디렉토리 주소는 스크립트 내에서 자주 사용하는 값이기 때문에 이를 변수로 저장합니다.
 * 쉘에선 타입 없이 선언하여 저장을 합니다.
 * 쉘에선 $변수명으로 변수를 사용할 수 있습니다.
* cd $REPOSITORY/sroom-api/
 * 제일 처음 git clone 받았던 디렉토리로 이동합니다.
 * 바로 위의 쉘 변수 설명처럼 $REPOSITORY으로 /home/ec2-user/app/git을 가져와 /sroom-api/를 붙인 디렉토리 주소로 이동합니다.
* git pull
 * 디렉토리 이동후, master브랜치의 최신 내용을 받습니다.
* ./gradlew build
 * 프로젝트 내부의 gradlew로 build를 수행합니다.
* cp ./build/libs/*.jar $REPOSITORY/
 * build의 결과물인 jar파일을 복사해 jar파일을 모아둔 위치로 복사합니다.
* CURRENT_PID=$(pgrep -f sroom)
 * 기존에 수행중이던 스프링부트 어플리케이션을 종료합니다.
 * pgrep은 process id만 추출하는 명령어입니다.
 * -f 옵션은 프로세스 이름으로 찾습니다.
 * 좀 더 자세한 옵션을 알고 싶으시면 [공식 홈페이지](https://linux.die.net/man/1/pgrep)를 참고하시면 좋습니다.
* if ~ else ~ fi
 * 현재 구동중인 프로세스가 있는지 없는지 여부를 판단해서 기능을 수행합니다.
 * process id값을 보고 프로세스가 있으면 해당 프로세스를 종료합니다.
 * kill -9는 spring의 @PreDestroy의 실행을 보장하기 힘드므로 -15(SIGTERM)을 활용하는게 좋습니다.
* JAR_NAME=$(ls $REPOSITORY/ |grep 'sroom-api' | tail -n 1)
 * 새로 실행할 jar 파일명을 찾습니다.
 * 여러 jar파일이 생기기 때문에 tail -n로 가장 나중의 jar파일(최신 파일)을 변수에 저장합니다.
* nohup java -jar $REPOSITORY/$JAR_NAME &
 * 찾은 jar파일명으로 해당 jar파일을 nohup으로 실행시킵니다.
 * 스프링부트의 장점으로 특별히 외장 톰캣을 설치할 필요가 없습니다.
 * 내장 톰캣을 사용해서 jar 파일만 있으면 바로 웹 어플리케이션 서버가 실행할수 있습니다.
 * 좀 더 자세한 스프링부트의 장점을 알고 싶으시면 jojoldu님의
 [SpringBoot의 깨알같은 팁](https://jojoldu.tistory.com/43)을 참고하시면 좋습니다.
 * 일반적으로 Java를 실행시킬때는 java -jar라는 명령어를 사용하지만, 이렇게 할 경우 사용자가 터미널 접속을 끊을 경우 어플리케이션도 같이 종료가 됩니다.
 * 어플리케이션 실행자가 터미널을 종료시켜도 어플리케이션은 계속 구동될 수 있도록 nohup명령어를 사용합니다.<br/>
<br/>

```
vim nohup.out
```
![src](/images/sroom/scr10.png) 위 명령어는 nohup.out을 만들고, 출력하는 역할을 담당합니다.
nohup은 실행시킨 jar 파일의 로그 내용을 nohup.out에 남기게 되는데 여기서 Class오류가 난다면 SpringBoot 버전 문제일 가능성이 높으니 1점대 2점대 버전에 맞는 gradle문법을 사용하셔야 오류가 나지 않습니다.<br/>
이제 만든 deploy.sh를 실행시키기 위해 실행권한을 줍니다.
```
chmod 755 ./deploy.sh
```
위 명령어를 실행하여 권한을 줍니다.
이제 권한을 준 스크립트를 실행하기 위하여 생성해놓은 SpringBoot 프로젝트로 이동합니다.<br/>
![src](/images/sroom/scr3.png) SpringBoot의 버전과 내용을 수정히고 commit & push합니다.
여기서 수정한 내용은 프로젝트 build/libs 폴더에 jar파일이 생성되어 이를 스크립트가 git폴더로 복사할 것입니다.<br/>
![src](/images/sroom/scr4.png) 아까 권한을 준 deploy.sh를 git폴더에서 입력하여 실행합니다.<br/>
```
ps -ef|grep 프로젝트명
```
![src](/images/sroom/scr5.png) 위 명령어를 통해서 실행된 프로세스를 확인할 수 있습니다.
프로젝트의 jar 파일이 최근 것과 일치한다면 push된 최신 프로젝트를 실행하였다는 것입니다.
<br/>

### 외부에서 서비스 접속하기
현재 제 프로젝트는 9000 포트를 사용하고 있습니다.
그래서 EC2에서도 9000 포트를 사용하여 외부에서 접근하도록 해야합니다.

![src](/images/sroom/scr6.png) EC2 대시보드에서 보안그룹의 인바운드에서 편집을 클릭합니다.<br/>
![src](/images/sroom/scr7.png) 사용자 지정 TC로 두고 자신이 사용하는 포트 범위를 지정하고 소스는 위와 같이 입력합니다.<br/>
![src](/images/sroom/scr8.png) 포트를 입력하였다면 인스턴스의 퍼블릭 DNS 주소를 입력하여 뒤에 포트 번호를 입력하여 접근이 가능합니다.<br/>
![src](/images/sroom/scr9.png) 위 주소로 접근한 모습입니다.<br/>
<br/>
현재 진행한 것은 스크립트를 작성해 수동으로 build와 배포를 진행하였습니다. 만드는 것도 힘들었지만 일일히 수동으로 해야한다는 점이 많이 불편했습니다.<br/>
다음엔 이 수동 방식을 자동화하는 작업을 진행하겠습니다.