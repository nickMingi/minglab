---
layout: "post"
title: "MAC basic knowledge"
author: "mingi hong"
date: 2020-01-25 14:05:23 -0700
categories: Computer Mac
permalink: /:categories/:title.html
---

01. �ƺ� ��й�ȣ ����

option Ű�� ���� ���·� �ƺ��� �� ��,
ȭ�鿡�� recovery HD�� �����ؼ� �����մϴ�.

��ƿ��Ƽ - �͹̳ο��� resetpassword�� �Է��մϴ�.

������ ���ο� ��ȣ�� 2ȸ �Է��� �� �����ϰ� ������� �մϴ�.

02. ���α׷� ��������

- ������ɾ� 1)
cd/Applications/ �Է��� ����

sudo rm -rf ���̸�.app/ �Է� �� ����

Password: �� �� ��й�ȣ�� �Է��� �� ����

- ������ɾ� 2)
sqlite3 ~/Library/Application\ Support/Dock/*.db ��DELETE
from apps WHERE title=�����̸���,��&& killall Dock


03. ���� ĸ���ϱ�

sudo screen -m -d bash -c ��sleep 30;screencapture /
 Users/����ڰ���/Desktop/ĸ�������̸�.png��

30 : 30�� �� ĸ��, ���ϴ� ���ڷ� �����ּ���.

����ڰ���: �ڽ��� �ƺ� ������ �����ּ���(nickhopper)

ĸ�������̸�: ����� ������ �̸��Դϴ�.


04. ���ͳݿ��� �ٿ�ε� ���â ����

 �ٿ�ε� ���Ͽ��� ��� ����

xattr -d -r com.apple.quarantine ~/Downloads

���������� ��� �ý��� ��� ����

defaults write com.apple.LaunchServices LSQuarantine -bool NO

NO ��� YES�� �����ø� �ٽ� ��� �˾��� ����Ͻ� �� �ֽ��ϴ�.


05. �ƺ� �͹̳� �����丮 ����

history�� �Է��ϰ� ���͸� Ĩ�ϴ�

history�� �����ϰ� �ʹٸ� history -c �� �Է� 
