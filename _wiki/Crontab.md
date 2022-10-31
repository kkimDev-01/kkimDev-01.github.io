---
layout: wiki
title: Crontab
summary:
date: 2022-10-31 00:00:00 +0900
updated: 2022-10-31 00:00:00 +0900
tag: linux
toc: true
public: true
parent: [[Linux]]
latex: true
---

## 참고문헌

- [리눅스 크론탭(Linux Crontab) 사용법](https://jdm.kr/blog/2)
- 배꼈습니다. 정리를 잘해주셔서 감사합니다.

## 크론탭

- 개요
  - 스케줄러
  - 특정 시간에 특정 작업을 하고 싶을 때 사용
- 기본 명령어
  1. crontab -e
  - 편집할 수 있는 곳이 로딩됨 : 크론탭을 설정
  - 각종 크론탭 명령어를 입력후 ":wq"로 저장+종료하여 크론탭을 갱신
  - 예시
    - - - - - - ls -al
    - 매분마다 ls -al 명령을 실행
  1. crontab -l
  - 현재 크론탭의 내용 확인
  2. crontab -r
  - (거의 사용하지 않겠지만) 크론탭을 지울 때
- 주기 결정
  - 분(0-59) 시간(0-23) 일(1-31) 월(1-12) 요일(0-7)
- 주기별 예제

  1. 매분 실행

  - - - - - - /home/script/test.sh
  - 매분 test.sh 실행

  2. 특정 시간 실행

  - 45 5 \* \* 5 /home/script/test.sh
  - 매주 금요일 오전 5시 45분에 test.sh 를 실행

  3. 반복 실행

  - 0,20,40 \* \* \* \* /home/script/test.sh
  - 매일 매시간 0분, 20분, 40분에 test.sh 를 실행

  4. 범위 실행

  - 0-30 1 \* \* \* /home/script/test.sh
  - 매일 1시 0분부터 30분까지 매분 tesh.sh 를 실행

  5. 간격 실행

  - _/10 _ \* \* \* /home/script/test.sh
  - 매 10분마다 test.sh 를 실행

  6. 조금 복잡하기 실행

  - _/10 2,3,4 5-6 _ \* /home/script/test.sh
  - 5일에서 6일까지 2시,3시,4시에 매 10분마다 test.sh 를 실행

- 크론 사용 팁

  1. 한 줄에 하나의 명령만 쓴다.

  - 좋은 예
    - - - - 5 5 /home/script/test.sh
  - 안좋은 예
    - - - - 5 5
    - /home/script/test.sh

  2. 주석을 단다.

  - #을 입력해서, 그 뒤로 나오는 모든 문자를 주석 처리할 수 있다.

- 크론 로깅

  - 크론탭으로 인한 정기적인 작업의 처리 내역에 대해서 로그를 남기고 싶을 때
  - - - - - - /home/script/test.sh > /home/script/test.sh.log 2>&1
    * 매분마다 test.sh.log 파일이 갱신되어 작업 내용이 어떻게 처리되었는지 알 수 있다.
    * 2>&1를 제거하면 쉘스트립트에서 표준 출력 내용만 나옴 ([[리눅스 특수문자 정리]] 글을 참고)
  - - - - - - /home/script/test.sh >> /home/script/test.sh.log 2>&1
    * > 는 하나의 로그파일을 업데이트(갱신), >>는 추가 생성
    * 계속 로그가 누적됨
  - - - - - - /home/script/test.sh > /dev/null 2>&1
    * 로그가 필요없는 크론의 경우
  - 로그가 과도하게 쌓이면 리눅스 퍼포먼스에 영향을 주므로, 가끔씩 비워주거나 파일을 새로 만들어주어야 함

- 크론탭 백업
  - 혹시라도 crontab -r을 쓰거나, 실수로 crontab 디렉토리를 날려버려서 기존 크론 내역들이 날라갈 경우를 대비
  - crontab -l > /home/bak/crontab_bak.txt
    - 크론탭 백업 명령어
  - 50 23 \* \* \* crontab -l > /home/bak/crontab_bak.txt
    - 크론탭 백업을 크론탭으로 자동화
    - 매일 오후 11시 50분에 크론탭을 백업
