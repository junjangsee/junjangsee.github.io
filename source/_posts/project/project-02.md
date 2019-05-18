---
title: StudyRoom(SRoom) 프로젝트 - 02(AWS EC2 & RDS 구축하기)
date: 2019-03-18 14:03:23
tags: AWS
---

# SRoom 프로젝트

![images](/images/sroom/studyroom.jpg)<br/>

먼저 AWS의 계정을 생성합니다.
AWS 홈페이지 : [https://aws.amazon.com/ko/console/](https://aws.amazon.com/ko/console/)

## AWS EC2 생성하기
### 1. 인스턴스 생성

![aws](/images/sroom/aws1.png) AWS에 가입하였다면 우상단에 국가를 **서울**로 변경합니다.<br/>
![aws](/images/sroom/aws2.png) 그리고 EC2를 검색하여 클릭하여 이동합니다.<br/>
![aws](/images/sroom/aws3.png) EC2 중앙에 있는 **인스턴스 시작** 버튼을 클릭합니다.<br/>
![aws](/images/sroom/aws4.png) Amazon Linux를 선택하여 AMI를 생성합니다.<br/>
![aws](/images/sroom/aws5.png) 인스턴스 유형은 t2.micro를 선택하고 다음:인스턴스 세부 정보 구성을 클릭합니다.<br/>
![aws](/images/sroom/aws6.png) 네트워크, 서브넷은 default값 그대로 사용합니다.
그리고 다음:스토리지 추가를 클릭합니다.<br/>
![aws](/images/sroom/aws7.png) 스토리지는 저장장치의 역할을 하는데 무료로 30GB까지 사용이 가능하기 때문에 30GB로 수정하고 다음:태그 추가를 클릭합니다.<br/>
![aws](/images/sroom/aws8.png)
![aws](/images/sroom/aws8-1.png) 태그는 인스턴스에 할당하는 키를 나타내는데
다른 태그 추가를 누른 후 본인의 키와 값을 넣어준 후 다음:보안 그룹 구성을 클릭합니다.<br/>
![aws](/images/sroom/aws9.png) 이 보안그룹은 요금 폭탄을 맞을 수 있는 조심해야하는 공간인데 IP를 전체오픈을 해버릴 경우 타인이 접속하여 사용할 수 있으므로 본인 집 IP를 기본적으로 추가하고 다른 장소의 경우 SSH 규칙을 추가하는 것이 안전합니다.
HTTPS 및 HTTP는 접근하기 위한 용도로 열어놓습니다.
다 되었다면 검토 및 시작 버튼을 클릭합니다.<br/>
![aws](/images/sroom/aws10.png) 인스턴스 시작 검토에서 자신이 입력한 정보와 맞는지 확인 후 시작 버튼을 클릭합니다.<br/>
![aws](/images/sroom/aws11.png) 시작 버튼을 클릭하면 Modal창이 등장하는데 새 키 페어 생성 후 다운로드 한 후 인스턴스 시작을 합니다.
여기서 다운로드 받은 키는 해당 인스턴스를 접근하기 위해 필요한 키 입니다.<br/>
![aws](/images/sroom/aws12.png) 생성된 인스턴스를 클릭합니다.<br/>
![aws](/images/sroom/aws13.png) running 상태를 표시하고 있다면 실행되고 있는 것입니다.
하지만 이 상태로 두게 되면 고정IP가 없어서 인스턴스 실행시마다 IP가 할달되어 도메인 연결을 할 수 없습니다.
그래서 고정 IP를 할당하기 위해 탄력적IP로 이동합니다.<br/>

### 2. EIP 등록

새 주소 할당 버튼을 클릭하여 받은 IP를 주소 연결을 시켜줍니다.
![aws](/images/sroom/aws14.png) 새 주소 할당 클릭<br/>
![aws](/images/sroom/aws14-1.png) 할당 클릭합니다.<br/>
![aws](/images/sroom/aws14-2.png) 할당 받은 IP입니다.<br/>
![aws](/images/sroom/aws14-3.png) 주소 연결 클릭합니다.<br/>
![aws](/images/sroom/aws14-4.png) 생성한 인스턴스와 IP를 등록합니다.<br/>
![aws](/images/sroom/aws14-5.png)
연결된 주소를 최종 확인합니다.

### 3. EC2 터미널 접속

Mac OS 환경으로 진행하기 때문에 터미널로 진행하였습니다.
만약 윈도우를 사용하시는 중이라면 아래의 링크를 참고하면 됩니다.
- [Window에서 EC2 접근하기](http://pyrasis.com/book/TheArtOfAmazonWebServices/Chapter07/02)


먼저 쉽게 SSH 접속을 하기 위해 권한을 변경하겠습니다.

```
pem키명 ~/.ssh/
```

![aws](/images/sroom/aws15.png) cp(copy)명령어를 통해 복사합니다.<br/>
그리고 pem 키 권한을 변경 해야합니다. 아래의 명령어를 통하여 변경합니다. 
```
chmod 600 본인pem키위치
```
![aws](/images/sroom/aws16.png) 권한이 바뀐 것을 확인할 수 있습니다.<br/>
다음 config 파일을 생성하여 Host등록을 합니다.
```
vim config
```
![aws](/images/sroom/aws17.png)

```
 Host 본인이 원하는 이름
       HostName 탄력적IP
       User ec2-user
       IdentityFile ~/.ssh/pem키
```
입력할 땐 **I** 저장할 땐 **:wq**로 저장 & 종료 하시면 됩니다.

- TIP! : **"Esc"**를 눌러야 명령어 모드로 바뀝니다.<br/>

![aws](/images/sroom/aws18.png) 위처럼 config 파일이 생성된 것을 볼 수 있습니다.<br/>
![aws](/images/sroom/aws19.png)

```
ssh 등록한Host값
```
위의 명령어를 입력했을 때 위와 같은 화면이 보이면 접속된 것입니다.<br/>
여기서 커널 업데이트를 하라는 문구가 나오면 아래 명령어로 업데이트를 진행합니다.
```
sudo yum update
```
<br/>여기까지 EC2 설정이 끝났습니다.
<br/>
## AWS RDS 설정하기

이제 AWS에서 Database서비스인 RDS를 설정해야합니다.

### 1. RDS 생성하기

![rds](/images/sroom/rds1.png) 검색창에 RDS를 검색하여 클릭합니다.<br/>
![rds](/images/sroom/rds2.png) 데이터베이스 생성 버튼을 클릭합니다.<br/>
![rds](/images/sroom/rds3.png) MySQL 기반으로 만들어진 MariaDB를 사용하겠습니다.<br/>
![rds](/images/sroom/rds4.png) 프리티어인 선택사항들을 선택해줍니다.<br/>
![rds](/images/sroom/rds5.png) 네트워크 및 보안 설정입니다.<br/> 
![rds](/images/sroom/rds5-1.png) 데이터베이스 옵션 설정입니다.<br/>
![rds](/images/sroom/rds5-2.png) 백업 설정입니다.<br/>
![rds](/images/sroom/rds5-3.png) 모니터링 및 로그 설정입니다.<br/>
![rds](/images/sroom/rds5-4.png) 유지관리 및 삭제방지 설정입니다.
프리티어는 최대 20GB까지 설정 가능합니다.<br/>
![rds](/images/sroom/rds6.png) 생성하게 되면 상태에 **생성 중**인 부분을 볼 수 있습니다.<br/>
![rds](/images/sroom/rds6-1.png)시간이 지나면 **사용 가능**으로 바뀌어 사용이 가능해집니다.<br/>
![rds](/images/sroom/rds7.png) 상세보기를 클릭하면 위의 화면과 같이 출력됩니다.
그리고 보안그룹을 추가하기 위해 보안그룹을 클릭합니다.<br/>
![rds](/images/sroom/rds7-1.png) 그룹 ID를 복사하고 보안 그룹 생성을 클릭합니다.<br/>
![rds](/images/sroom/rds7-2.png) 복사한 그룹 ID를 하나 추가하고 본인 IP를 추가합니다.<br/>
보안 그룹 ID를 추가한 이유
- EC2와 RDS의 통신을 위해서입니다.<br/>

IP를 추가한 이유
- 현재 접속한 IP를 등록하여 PC에서 DB Client Tool (Workbench, Sequel Pro 등)으로 RDS에 접속하기 위함입니다.<br/>

![rds](/images/sroom/rds7-3.png)
다시 RDS 페이지로 돌아가 수정 버튼을 클릭합니다.<br/>
![rds](/images/sroom/rds7-4.png)
보안 그룹에서 방금 생성한 보안 그룹을 추가합니다.
여기서 중요한 것은 기존에 등록되어 있던 보안 그룹은 삭제합니다.<br/>
![rds](/images/sroom/rds7-5.png) 보안 그룹에 추가된 것을 확인할 수 있습니다.
<br/>

### 2. PC에서 RDS 접근 확인

이제 RDS가 생성되었으니 확인을 해볼 차례입니다.
본인의 OS에 맞춰서 DB 클라이언트를 선택해주시면 됩니다.

- [MySQl Workbench](https://cloud.hosting.kr/mysql-workbench/)
- [Sequel Pro(Mac OS)](https://geniusdo.tistory.com/9)<br/>

전 Mac OS을 사용하기 때문에 Sequal Pro를 사용하였습니다.<br/>

![rds](/images/sroom/rds8.png) RDS에 출력된 **엔드포인트**라는 부분이 외부에서 RDS로 붙을 수 있는 주소입니다.<br/>
![rds](/images/sroom/rds9.png)
위 주소를 복사해서 다운받은 Sequel Pro에 자신이 생성했던 아이디와 비밀번호를 입력하고 Save, Connect를 합니다.<br/>
접속 되었다면 아래 명령어를 실행합니다.
```
show variables like 'c%';
```
위 명령어는 DB의 character-set 설정을 볼 수 있습니다.<br/>
![rds](/images/sroom/rds10.png)
체크박스를 보시면 **latin1**로 표기가 되어있어 한글이 전부 깨지는 현상이 발생하기 때문에 전부 **UTF-8**로 변환해주어야 합니다.<br/>
![rds](/images/sroom/rds11.png) 좌측에 파라미터 그룹을 클릭하고 생성을 클릭합니다.
생성하는 이유는 default값을 바꿀 수 없기 때문에 새로 추가해야만 합니다.<br/>
![rds](/images/sroom/rds11-1.png) 그룹을 생성합니다.<br/>
![rds](/images/sroom/rds11-2.png) 생성한 그룹을 클릭합니다.<br/>
![rds](/images/sroom/rds11-3.png) 파라미터 편집을 클릭합니다.<br/>
![rds](/images/sroom/rds11-4.png) 위 체크박스에 해당하는 것들을 편집합니다.<br/>
![rds](/images/sroom/rds11-5.png) 다시 RDS페이지에서 수정 버튼을 클릭합니다.<br/>
![rds](/images/sroom/rds11-6.png) 파라미터 그룹을 방금 추가한 그룹으로 선택합니다.<br/>
![rds](/images/sroom/rds11-7.png)수정하면 수정사항을 즉시 적용하기 위해 즉시 적용을 체크하고 수정합니다.<br/>
![rds](/images/sroom/rds11-8.png) 수정사항을 반영하기 위해 인스턴스에서 재부팅을 합니다<br/>
![rds](/images/sroom/rds11-9.png) 위 사진을 보면 수정이 안된 것을 알 수 있는데 직접 명령어를 통해 해결해야합니다<br/>
```
ALTER DATABASE 본인database
CHARACTER SET = 'utf8'
COLLATE = 'utf8_general_ci';
```
위의 명령어를 통해 UTF-8을 적용시킵니다.<br/>
![rds](/images/sroom/rds11-10.png) 명령어를 입력하면 수정된 것을 볼 수 있습니다.<br/>
```
CREATE TABLE test (
  id bigint(20) NOT NULL AUTO_INCREMENT,
  content varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB;

insert into test(content) values ('테스트');

select * from test;
```
위의 쿼리문을 하나씩 실행해보면 실제로 한글 적용이 되었는지 확인할 수 있습니다<br/>
![rds](/images/sroom/rds11-11.png) 한글로 출력되는 모습<br/><br/>
### 3. EC2에서 RDS 접근 확인

```
ssh config에 지정한 이름
```
![rds](/images/sroom/rds11-12.png) 기존에 생성해준 EC2 서버로 ssh 접속합니다.<br/>
![rds](/images/sroom/rds11-13.png)  그리고 MySQL을 EC2에서 커맨드 라인을 사용하기 위해 설치합니다.
```
sudo yum install mysql
```
<br/>
설치가 다 되었다면 계정, 비밀번호, Host 주소를 입력하여 접속합니다.
```
mysql -u 계정 -p -h Host주소
```

![rds](/images/sroom/rds11-14.png) 접속한 화면입니다.<br/>
생성한 RDS인지 확인하기 위해 아래 명령어를 입력합니다.
```
show databases;
```
![rds](/images/sroom/rds11-15.png) 생성된 RDS가 있는 것을 확인할 수 있습니다.
<br/>
다음 시간엔 EC2서버에 Git 설치와 기존에 생성했던 SpringBoot를 Clone해보겠습니다.