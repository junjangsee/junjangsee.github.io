---
title: docker 명령어 및 옵션 정리
date: 2019-05-19 18:22:24
tags: docker
---

![images](/images/docker/docker.png)<br/>

# docker 명령어 및 옵션
Docker를 사용하게 되면서 생소한 명령어와 옵션이 많아 기초부터 차근차근 정리하여 언젠간 모든 명령어와 옵션을 작성해보려 합니다.<br/>
<br/>

## 옵션
- -e: 환경변수
- -d: daemon 명
- -name: docker 프로세스 명
- -i: 인터렉티브 모드
- -t: target 이 되는 컨테이너
- bash: 실행할 명령어

<br/>
## 명령어
### 동작 중인 프로세스 보기
```bash
docker ps
```
<br/>
### 동작하지 않는 컨테이너 보기
```bash
docker ps -a
```
<br/>
### 컨테이너 삭제
```bash
docker rm 컨테이너명
```
<br/>
### 이미지 삭제
```bash
docker rmi 이미지명
```
<br/>
### 컨테이너 중지
```bash
docker stop 컨테이너명
```
<br/>
### 컨테이너 시작
```bash
docker start 컨테이너명
```