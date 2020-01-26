---
layout: "post"
title: "MAC basic knowledge"
author: "mingi hong"
date: 2020-01-25 14:05:23 -0700
categories: Computer Mac
permalink: /:categories/:title.html
---

01. 맥북 비밀번호 변경

option 키를 누른 상태로 맥북을 켠 뒤,
화면에서 recovery HD를 선택해서 부팅합니다.

유틸리티 - 터미널에서 resetpassword를 입력합니다.

열리면 새로운 암호를 2회 입력한 뒤 저장하고 재부팅을 합니다.

02. 프로그램 완전삭제

- 삭제명령어 1)
cd/Applications/ 입력후 엔터

sudo rm -rf 앱이름.app/ 입력 후 엔터

Password: 에 맥 비밀번호를 입력한 후 엔터

- 삭제명령어 2)
sqlite3 ~/Library/Application\ Support/Dock/*.db “DELETE
from apps WHERE title=‘앱이름’,”&& killall Dock


03. 예약 캡쳐하기

sudo screen -m -d bash -c “sleep 30;screencapture /
 Users/사용자계정/Desktop/캡쳐파일이름.png”

30 : 30초 후 캡쳐, 원하는 숫자로 적어주세요.

사용자계정: 자신의 맥북 계정을 적어주세요(nickhopper)

캡쳐파일이름: 저장될 파일의 이름입니다.


04. 인터넷에서 다운로드 경고창 끄기

 다운로드 파일에서 경고를 제거

xattr -d -r com.apple.quarantine ~/Downloads

영구적으로 경고 시스템 사용 끄기

defaults write com.apple.LaunchServices LSQuarantine -bool NO

NO 대신 YES를 적으시면 다시 경고 팝업을 사용하실 수 있습니다.


05. 맥북 터미널 히스토리 보기

history를 입력하고 엔터를 칩니다

history를 삭제하고 싶다면 history -c 를 입력 
