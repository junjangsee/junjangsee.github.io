---
title: StudyRoom(SRoom) 프로젝트 - 04(TravisCI & CodeDeploy로 배포 자동화 하기)
date: 2019-03-22 13:26:59
tags: AWS
---

# SRoom 프로젝트

![images](/images/sroom/studyroom.jpg)<br/>

## CI(지속적 통합)란 ?
코드 버전 관리를 하는 VCS 시스템에 push가 되면 Test, Build가 수행되고 Build된 결과를 운영 서버에 배포까지 자동으로 진행되는 과정을 **CI(지속적 통합)**이라고 합니다.<br/>
목적은 **테스팅 자동화**에 있으며 배포하는 프로젝트가 완전한 상태임을 보장하는 테스트 코드가 구현되어 있어야 합니다.
<br/>
## Travis CI 연동하기
Travis CI는 Github에서 제공하는 무료 CI입니다.
Jenkins(젠킨스) 등 CI 툴도 있으나 설치형이기 때문에 이를 활용하기 위해서는 EC2에 새로운 인스턴스를 필요로 하게 되어 초보자에겐 부담스러운 면이 없지않아 있습니다.
그래서 초보자인 저는 Travis CI를 이용하기로 결정하였습니다.<br/>
[Travis CI 홈페이지](https://travis-ci.org/)
<br/>
### Travis 웹 서비스 설정
![tra](/images/sroom/tra1.png) 본인 Github 계정으로 [Travis CI](https://travis-ci.org/)에 로그인 한 뒤 Setting를 클릭합니다.<br/>
![tra](/images/sroom/tra2.png) 자신이 배포할 서비스의 상태바를 활성화 시킵니다.<br/>
![tra](/images/sroom/tra3.png) 자신의 저장소를 가면 추가된 저장소를 볼 수 있습니다<br/>
이걸로 Travis CI 웹사이트 설정을 마쳤습니다.
여기에 대한 세부설정은 프로젝트의 yml파일로 진행합니다.
<br/>
### 프로젝트 설정
Travis CI의 세부 설정은 프로젝트의 **.travis.yml**를 통해 설정합니다. 프로젝트에 **.travis.yml**를 생성하고 아래 코드를 입력합니다.
```
language: java
jdk:
  - openjdk8

branches:
  only:
    - master

# Travis CI 서버의 Home
cache:
  directories:
    - '$HOME/.m2/repository'
    - '$HOME/.gradle'

script: "./gradlew clean build"

# CI 실행 완료시 메일로 알람
notifications:
  email:
    recipients:
      - junjang.see@gmail.com 
```
- branches - master 브랜치에서 push 될 때만 구동합니다.
- cache - Gradle을 통해 의존성을 받게 되면 이를 해당 디렉토리에 캐시하여, 같은 의존성은 다음 배포때부터 다시 받지 않도록 설정합니다.
- script - master 브랜치에 push 되었을 때 수행합니다.
- notifications - Travis CI 실행 완료시 자동으로 알람이 갑니다.
<br/>

![tra](/images/sroom/tra4.png) 위 코드를 입력한 yml 파일입니다.
yml을 추가하였다면 master 브랜치로 Commit & Push를 해줍니다.<br/>
![tra](/images/sroom/tra5.png) 빌드가 성공하였습니다.
<br/>
![tra](/images/sroom/tra6.png) yml파일에 작성했던 이메일에도 성공했다는 소식이 전해집니다.<br/>
만약 Build가 실패하였다면 **.travis.yml**의 위치를 확인하고 사진과 같이 src외부에 있는지 확인해보면 됩니다.
기존에 yml파일처럼 resource내부에 위치시키면 Build가 실패합니다. **위치확인!!**<br/>
<br/>
### Travis CI 라벨 추가하기
Github에 등록된 오픈소스들을 보면 우측에 build passing이란 라벨 문구를 볼 수 있는데 이는 Travis CI 에서 제공하는 라벨입니다.

![tra](/images/sroom/tra7.png) Travis CI Build된 passing 라벨을 클릭하면 Modal창이 등장하는데 **Markdown**으로 선택하고 나온 값을 복사합니다.<br/>
![tra](/images/sroom/tra8.png) 복사한 값을 해당 프로젝트 README.md의 원하는 위치에 복사하여 붙여넣기 합니다.
그리고 master 브랜치에서 Commit & Push합니다.<br/>
![tra](/images/sroom/tra9.png) 짜잔! 배포하는 프로젝트에 라벨이 생겼습니다.
지금까지 **테스트**와 **빌드**를 자동화 하였습니다.
이제부터 **배포**를 자동화 해보겠습니다.
<br/>
## AWS Code Deploy 연동하기
이제 Test를 마치고 Build된 파일을 EC2 인스턴스로 전달시켜주는 작업을 진행하겠습니다.

![cd](/images/sroom/cd1.png) IAM을 검색하여 클릭합니다.<br/>
![cd](/images/sroom/cd2.png) 사용자 탭 클릭 후 사용자 추가 버튼을 클릭합니다<br/>
![cd](/images/sroom/cd3.png) 사용자 이름 입력, 프로그래밍 방식 엑세스에 체크합니다.<br/>
![cd](/images/sroom/cd4.png) AmazonS3FullAccess 체크합니다.<br/>
![cd](/images/sroom/cd5.png) AWSCodeDeployFullAccess 체크합니다.
두가지 선택한 현재 페이지는 정책을 선택하는 페이지로서 CodeDeploy와 S3(Build 파일 백업용) 권한만 가져가겠습니다.<br/>
![cd](/images/sroom/cd6.png) 선택한 두개가 맞는지 확인합니다<br/>
![cd](/images/sroom/cd7.png) 좌측 부분에 .scv를 다운로드 받아 놓습니다.
이제 Travis CI를 위한 계정이 완성된 것이고 이 계정으로 배포를 진행할 것입니다.<br/>
<br/>
### AWS S3 버킷 생성하기
Build된 jar 파일을 보관하는 장소도 필요합니다.
S3를 사용하여 저장해보도록 하겠습니다.

![S3](/images/sroom/s1.png) S3를 검색하여 클릭합니다<br/>
![S3](/images/sroom/s2.png) 버킷만들기를 선택합니다.<br/>
![S3](/images/sroom/s3.png) 제목을 원하는 제목으로 입력하고 서울로 선택하고 추가 선택사항 없이 쭉 다음을 눌러 생성합니다.<br/>
![S3](/images/sroom/s4.png) 생성된 버킷을 볼 수 있습니다.<br/>
<br/>
### IAM Role 추가하기
사용자를 대신해 access key & secret key를 사용하여 원하는 기능을 진행하게할 역할을 생성합니다.

![Role](/images/sroom/ro1.png) IAM을 검색하여 클릭합니다.<br/>
![Role](/images/sroom/ro2.png) 역할을 클릭 후 역할 만들기를 클릭합니다.<br/>
![Role](/images/sroom/ro3.png)
![Role](/images/sroom/ro4.png) AWS 서비스, EC2, 사용사례에서 EC2 세가지를 선택하고 다음을 클릭합니다<br/>
![Role](/images/sroom/ro5.png) AmazonEC2RoleforAWSCodeDeploy만 체크하고 다음을 클릭합니다.<br/>
![Role](/images/sroom/ro6.png) 역할 이름과 선택한 정책이 맞는지 확인하고 역할을 만듭니다.
보통 역할 이름의 뒤에는 **EC2CodeDeployRole** 붙입니다.<br/>

그리고 여기서 CodeDeploy를 생성하여 배포를 진행하는 역할을 추가해야합니다.
위와 과정은 동일합니다.<br/>
![Role](/images/sroom/ro7.png) AWS 서비스, CodeDeploy, CodeDeploy를 클릭하고 다음을 클릭합니다.<br/>
![Role](/images/sroom/ro8.png) 하나의 역할밖에 없어서 따로 체크가 필요 없습니다.<br/>
![Role](/images/sroom/ro9.png) 제목에는 **CodeDeployRole**를 붙여주었습니다.
확인되었다면 역할을 만듭니다.<br/>
이제 만든 역할들을 AWS에 할당하겠습니다.<br/>
<br/>
### EC2에 Code Deploy Role 추가하기

역할을 인스턴스에 연결시켜 보겠습니다.

![add](/images/sroom/add1.png) 인스턴스 우클릭을하여 IAM 역할 연결/바꾸기를 클릭합니다.<br/>
![add](/images/sroom/add2.png) 생성할 역할을 선택하여 적용합니다.<br/>
이제 Code Deploy Agent를 설치하러갈 차례입니다.<br/>
<br/>
### EC2에 Code Deploy Agent 설치하기

EC2에서 CodeDeploy에서 실행하는 이벤트를 받아서 처리할 수 있도록 Agent를 설치해야합니다. 

먼저 EC2 ssh에 접속합니다.<br/>
```
sudo yum -y update
```
먼저 위 명령어를 통해 업데이트가 안되어 있다면 업데이트를 먼저 실행합니다.
되어있다면 생략해도 무관합니다.<br/>
AWS를 커맨드로 다루기 위해서 **AWS CLI**를 설치하여 사용하겠습니다.
```
sudo yum install -y aws-cli
```
위 명령어를 통해 설치합니다.<br/>
설치 되었다면 /home/ec2-user/로 이동하고 AWS CLI에 accessKey & secretKey를 입력하는데 CodeDeploy를 연동할 때 다운받았던 .csv 파일을 열어 해당 값을 복사해 입력하면 됩니다.<br/>

```
cd /home/ec2-user/

sudo aws configure
```
위 명령어를 차례로 입력합니다.<br/>
![agent](/images/sroom/ag1.png) 위 사진처럼 입력합니다.
사진에 대한 설명을 아래와 같습니다.<br/>
* Access Key
* Secret Access Key
* region name
 * ap-northeast-2
 * 서울을 나타냅니다.
* output format
 * json
<br/>

다 입력하였다면 CLI 기본설정은 끝입니다.
이제 **AWS Code Deploy CLI**를 설치해야합니다.<br/>
```
aws s3 cp s3://aws-codedeploy-ap-northeast-2/latest/install . --region ap-northeast-2
```
위 명령어를 기존 경로에서 실행합니다.<br/>
설치가 되었다면 install 파일이 생성되는데 실행할 수 있는 권한을 아래 명령어로 부여합니다. (+x는 실행 권한을 얘기합니다.)
```
chmod +x ./install
```
![agent](/images/sroom/ag2.png) 설치되고 권한을 부여하였습니다.<br/>
권한을 부여하였다면 파일을 아래 명령어로 실행시켜 설치 & 실행합니다.
```
sudo ./install auto
```
![agent](/images/sroom/ag3.png) 설치되는 모습...<br/>
설치가 완료되었다면 아래 명령어로 Agent가 실행중인지 확인해봅니다.
```
sudo service codedeploy-agent status
```
![agent](/images/sroom/ag4.png) 실행하면 PID번호와 함께 running 하고 있다는 문구가 나옵니다.<br/>
마지막으로 EC2 인스턴스가 부팅되면 자동으로 Agent가 실행될 수 있도록 스크립트 파일을 추가하겠습니다.<br/>
TIP : 리눅스에 **/etc/init.d/** 라는 스크립트 파일이 있으면 부팅시 자동으로 실행됨<br/>
```
sudo vim /etc/init.d/codedeploy-startup.sh
```
위 명령어를 통해 파일을 생성합니다<br/>
그리고 스크립트 파일에 아래의 내용을 추가합니다.
```
#!/bin/bash

echo 'Starting codedeploy-agent'
sudo service codedeploy-agent start
```

![agent](/images/sroom/ag5.png) 위 사진처럼 설정하면 AWS 설정은 끝이 납니다.
<br/>
## .travis.yml & appspec.yml 설정하기

Travis CI, AWS CodeDeploy 둘 다 설치형이 아니기 때문에 yml 파일로 관리합니다.
<br/>
### Travis CI & S3 연동하기

CodeDeploy는 저장 기능이 없기 때문에 CI가 Build된 결과를 받아서 CodeDeploy가 가져가기 위해 저장을 할 수 있는 S3를 만들었었습니다. 이를 연동해보겠습니다.<br/>
.travis.yml 파일에 아래의 코드를 추가합니다.
```
deploy:
  - provider: s3
    access_key_id: $AWS_ACCESS_KEY # Travis repo settings에 설정된 값(Environment Variables)
    secret_access_key: $AWS_SECRET_KEY # Travis repo settings에 설정된 값(Environment Variables)
    bucket: sroom-deploy # 6-3-3에서 생성한 S3 버킷
    region: ap-northeast-2
    skip_cleanup: true
    acl: public_read
    local_dir: deploy # before_deploy에서 생성한 디렉토리
    wait-until-deployed: true
    on:
      repo: liveonly-today/sroom-api #Github 주소(travis에 있는 주소도 무방)
      branch: master
```
여기서 ACCESS_KEY & SECRET_KEY를 입력할 시 코드에 노출되기 때문에 다른 방법을 사용해야 합니다.<br/>
![yml](/images/sroom/yml1.png) 해당 프로젝트의 setting으로 들어갑니다.<br/>
![yml](/images/sroom/yml2.png) **Environment Variables** 항목에서 AWS_ACCESS_KEY, AWS_SECRET_KEY 두 변수 값을 추가하여 값을 입력하여 Add합니다.
두 변수를 사용하면 yml 파일에서 $AWS_ACCESS_KEY, $AWS_SECRET_KEY라는 변수 명으로 사용할 수 있습니다.<br/>
이제 실제로 연동이 되었는지 프로젝트를 Commit & Push 해봅니다.
![yml](/images/sroom/yml3.png) Build가 성공되면 생성했던 S3의 버킷에 성공된 결과가 나타난 것을 볼 수 있습니다.
하지만 파일들 하나하나 전부 복사하는건 시간소요가 많기 때문에 폴더를 압축하여 S3에 저장하도록 추가하였습니다.<br/>
추가한 코드는 아래와 같습니다.
```
before_deploy:
  - zip -r sroom-api *
  - mkdir -p deploy
  - mv sroom-api.zip deploy/sroom-api.zip

deploy:
    local_dir: deploy # before_deploy에서 생성한 디렉토리
```
<br/>
그래서 최종적인 .travis.yml의 코드는...
```
language: java
jdk:
  - openjdk8

branches:
  only:
    - master

# Travis CI 서버의 Home
cache:
  directories:
    - '$HOME/.m2/repository'
    - '$HOME/.gradle'

script: "./gradlew clean build"

# S3에 폴더를 압축하여 저장
before_deploy:
  - zip -r sroom-api *
  - mkdir -p deploy
  - mv sroom-api.zip deploy/sroom-api.zip

# CI 실행 완료시 메일로 알람
notifications:
  email:
    recipients:
      - junjang.see@gmail.com

# Travis CI & S3 연동
deploy:
  - provider: s3
    access_key_id: $AWS_ACCESS_KEY # Travis repo settings에 설정된 값(Environment Variables)
    secret_access_key: $AWS_SECRET_KEY # Travis repo settings에 설정된 값(Environment Variables)
    bucket: sroom-deploy # 6-3-3에서 생성한 S3 버킷
    region: ap-northeast-2
    skip_cleanup: true
    acl: public_read
    local_dir: deploy # before_deploy에서 생성한 디렉토리
    wait-until-deployed: true
    on:
      repo: liveonly-today/sroom-api #Github 주소
      branch: master
```
위 코드와 같습니다.
위 코드를 Commit & Push 후 다시 Travis CI의 Build 후 S3에 저장이 되면<br/>
![yml](/images/sroom/yml4.png) 위와 같이 폴더를 압축한 파일로 S3에 저장이 됩니다.<br/>
<br/>
### Travis CI & S3 & CodeDeploy 연동하기

이제 CodeDeploy까지 연동할 차례입니다.

![tsc](/images/sroom/tsc1.png) CodeDeploy를 검색하여 클릭합니다.<br/>
![tsc](/images/sroom/tsc2.png) 애플리케이션 생성 버튼을 클릭합니다.<br/>
![tsc](/images/sroom/tsc3.png) 이름과 컴퓨팅 플랫폼을 EC2/온프레미스를 선택합니다<br/>
![tsc](/images/sroom/tsc4.png) 만들어 졌다면 배포 그룹 생성 버튼을 클릭합니다.<br/>
![tsc](/images/sroom/tsc5.png) 
![tsc](/images/sroom/tsc6.png) 
![tsc](/images/sroom/tsc7.png) 
![tsc](/images/sroom/tsc8.png) 
![tsc](/images/sroom/tsc9.png) 
위와 같이 배포 그룹을 생성합니다.
이전 버전이 편하다면 위의 파란색 버튼에 이전 버전으로 돌아가는 버튼을 클릭하여 돌아가면 됩니다.<br/>
설정이 끝났다면 S3에 저장된 zip을 가져오기 위해 폴더를 생성하겠습니다.<br/>
```
mkdir /home/ec2-user/app/travis
mkdir /home/ec2-user/app/travis/build
```
Travis CI에서 Build가 끝나면 S3에 zip파일이 저장되고 이 파일은 만들어진 파일 내로 복사되어 압축을 풀 것입니다.<br/>
![tsc](/images/sroom/tsc10.png) AWS CodeDeploy 설정은 appspec.yml으로 진행합니다.<br/>
yml에 들어가는 코드는 아래를 입력합니다.
```
version: 0.0
os: linux
files:
  - source:  /
    destination: /home/ec2-user/app/travis/build/
```
코드 설명은 아래를 참고하시면 됩니다.
* version - 0.0
 * CodeDeploy 버전을 말하며 프로젝트 버전이 아니기 때문에 0.0 외에는 오류가 발생합니다.
* source - S3에 복사할 파일 위치를 말합니다.
* destiantion - zip 파일을 복사해 압출을 풀 위치를 말합니다.
<br/>

그리고 Travis CI가 CodeDeploy도 함께 실행시키기 위해서 .travis.yml에 설정을 추가합니다.
```
  - provider: codedeploy
    access_key_id: $AWS_ACCESS_KEY # Travis repo settings에 설정된 값
    secret_access_key: $AWS_SECRET_KEY # Travis repo settings에 설정된 값
    bucket: sroom-deploy # S3 버킷
    key: sroom-api.zip # 빌드 파일을 압축해서 전달
    bundle_type: zip
    application: sroom-api # 웹 콘솔에서 등록한 CodeDeploy 어플리케이션
    deployment_group: sroom-api-group # 웹 콘솔에서 등록한 CodeDeploy 배포 그룹
    region: ap-northeast-2
    wait-until-deployed: true
    on:
      repo: liveonly-today/sroom-api
      branch: master
```
<br/>
위 코드를 추가했다면 최종 .travis.yml 코드는 아래와 같습니다.
```
language: java
jdk:
  - openjdk8

branches:
  only:
    - master

# Travis CI 서버의 Home
cache:
  directories:
    - '$HOME/.m2/repository'
    - '$HOME/.gradle'

script: "./gradlew clean build"

# S3에 폴더를 압축하여 저장
before_deploy:
  - zip -r sroom-api *
  - mkdir -p deploy
  - mv sroom-api.zip deploy/sroom-api.zip

# CI 실행 완료시 메일로 알람
notifications:
  email:
    recipients:
      - junjang.see@gmail.com

# Travis CI & S3 연동
deploy:
  - provider: s3
    access_key_id: $AWS_ACCESS_KEY # Travis repo settings에 설정된 값(Environment Variables)
    secret_access_key: $AWS_SECRET_KEY # Travis repo settings에 설정된 값(Environment Variables)
    bucket: sroom-deploy # 6-3-3에서 생성한 S3 버킷
    region: ap-northeast-2
    skip_cleanup: true
    acl: public_read
    local_dir: deploy # before_deploy에서 생성한 디렉토리
    wait-until-deployed: true
    on:
      repo: liveonly-today/sroom-api #Github 주소
      branch: master

  - provider: codedeploy
    access_key_id: $AWS_ACCESS_KEY # Travis repo settings에 설정된 값
    secret_access_key: $AWS_SECRET_KEY # Travis repo settings에 설정된 값
    bucket: sroom-deploy # S3 버킷
    key: sroom-api.zip # 빌드 파일을 압축해서 전달
    bundle_type: zip
    application: sroom-api # 웹 콘솔에서 등록한 CodeDeploy 어플리케이션
    deployment_group: sroom-api-group # 웹 콘솔에서 등록한 CodeDeploy 배포 그룹
    region: ap-northeast-2
    wait-until-deployed: true
    on:
      repo: liveonly-today/sroom-api
      branch: master
```
위 코드를 가지고 Commit & Push를 합니다.<br/>
![tsc](/images/sroom/tsc11.png) 성공했다면 배포 그룹 상태에 성공이 나타납니다.<br/>
이제 EC2에서도 배포가 잘 되었는지 확인해보겠습니다.
```
cd /home/ec2-user/app/travis/build
ll
```
위 명령어를 작성하여 확인합니다.<br/>
![tsc](/images/sroom/tsc12.png) EC2에 자동으로 전송된 것을 확인할 수 있습니다.<br/>
<br/>

### CodeDeploy로 스크립트 실행하기

최종적으로 jar파일을 실행하여야 배포가 됩니다.
그래서 EC2에 AWS CodeDeploy로 받은 파일을 실행하는 스크립트를 만들어야 합니다.

```
mkdir /home/ec2-user/app/travis/jar
```
위 명령어로 jar파일을 모아놓을 디렉토리를 생성합니다.<br/>
```
vim /home/ec2-user/app/travis/deploy.sh
```
그리고 jar파일을 실행시킬 deploy.sh 파일을 생성합니다.
생성된 파일에 넣을 스크립트는 아래와 같습니다.<br/>
```
#!/bin/bash

REPOSITORY=/home/ec2-user/app/travis

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

echo "> Build 파일 복사"

cp $REPOSITORY/build/build/libs/*.jar $REPOSITORY/jar/

JAR_NAME=$(ls $REPOSITORY/jar/ |grep 'sroom-0' | tail -n 1)

echo "> JAR Name: $JAR_NAME"

nohup java -jar $REPOSITORY/jar/$JAR_NAME &
```
이전에 작성한 git/deploy.sh와 크게 다르지 않고 직접 build한 부분을 제거하고 이미 존재하는 build 파일을 복사해 실행합니다.<br/>
생성한 스크립트가 잘 작동하는지 보기 위해 아래 명령어로 테스트합니다.
```
/home/ec2-user/app/travis/deploy.sh
```
![tsc](/images/sroom/tsc13.png) 위처럼 정상적으로 실행되는 것을 확인하였습니다.
그렇다면 AWS CodeDeploy 배포가 끝나면 deploy.sh를 실행하도록 해야합니다.
그러기 위해선 appspec.yml에 코드를 추가해야합니다.<br/>
```
hooks:
  AfterInstall: # 배포가 끝나면 아래 명령어를 실행
    - location: execute-deploy.sh
      timeout: 180
```
위 코드를 통해 우회하여 실행하는 execute-deploy.sh 파일을 설정하도록 합니다.<br/>
execute-deploy.sh는 프로젝트 내부에 생성하여 실행 가능하게끔 합니다.
코드는 한 줄을 작성합니다.
```
#!/bin/bash
/home/ec2-user/app/travis/deploy.sh > /dev/null 2> /dev/null < /dev/null &
```
![tsc](/images/sroom/tsc14.png) 위 코드는 deploy.sh를 백그라운드에서 실행 후 로그 및 기타 내용을 남기지 않도록 처리한 것입니다.<br/>
<br/>
### 실제 배포 과정 진행하기

build.gradle에서 프로젝트 버전을 변경합니다.
그리고 잘 알아볼 수 있게 Spring 내용도 수정합니다.<br/>
그리고 Commit & Push 합니다.
![tsc](/images/sroom/tsc15.png) 최종적으로 자동배포 된 것을 확인할 수 있습니다.