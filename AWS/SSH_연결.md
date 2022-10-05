# SSH를 사용하여 Linux 인스턴스에 연결
클라이언트 SSH 접속 
- private key 저장 (/home/ejkim/AWS_Study_Key.pem) 
- SSH 명령어 입력
```
ssh -i /home/vada/ejkim/AWS_Study_Key.pem ubuntu@3.39.233.79
```
<br>

발생할 수 있는 에러
- private key 의 퍼미션이 너무 공개되어 생긴 문제
```
VADA Manager 3.0
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0660 for '/home/vada/ejkim/AWS_Study_Key.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "/home/vada/ejkim/AWS_Study_Key.pem": bad permissions
Permission denied (publickey).
```
- 권한설정 명령어 입력
```
chmod 600 ~/.ssh/your-key.pem
```
