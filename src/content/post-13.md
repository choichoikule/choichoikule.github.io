---
title: "운영체제 - 컴퓨터의 하드웨어 시스템"
date: "2021-10-12"
draft: true
path: "/blog/pos-13-operating-system-1"
---

컴퓨터 하드웨어는 크게 프로세서 메모리 주변장치로 구성되고 이들은 시스템버스로 연결한다.
프로세서는 연산장치 제어장치 레지스터로 구성되고 이들은 내부버스로 연결한다.
레지스터는 범용과 전용으로 구분할 수도 있고, 가시와 불가시로 구분할 수 도 있ㅇ며, 저장하는 정보에따라 데이터레지스터 주소레지스터 상태레지스터들으로 세부화할수도 있다.

가시레지스터는 사용자가 운영체제와 사용자 프로그램을 이용하여 정보를 변경할 수 있는 레지스터이다. 그래서 접근이 가능한 데이터와 주소, 일부 조건코드를 보관한다.
데이터 레지스터- 함수 연산에 필요한 데이터 값
주소레지스터
