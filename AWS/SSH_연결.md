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
<br><br>
## Java SSH 접속  
Java에서 ssh로 접속하려면 jsch 라이브러리를 사용하여 접속한다.  
일반적인 ssh 접속은 사용자의 아이디, 패스워드를 입력하여 접속하지만, AWS 의 EC2 서버에 접속하려면 pem 인증서로 접속을 한다.  
```java
    import com.jcraft.jsch.*;

    private String user = "ubuntu";
    private String host = "3.39.233.79";
    private int port = 22;
    private String privateKey = "/home/vada/ejkim/AWS_Study_Key.pem";
    private Session session;
    private ChannelExec channelExec;

	private VADAReturnCode AWSSSHTest(HttpSession s) throws Exception{
		VADAReturnCode r = VADAReturnCode.SUCCESS;		
		try{
			connectSSH(user, host, privateKey, port);
            channelExec = (ChannelExec) session.openChannel("exec");
            channelExec.setCommand("cat /etc/lsb-release");
            InputStream inputStream = channelExec.getInputStream();
            channelExec.connect();
            byte[] buffer = new byte[8192];
            int decodedLength;
            StringBuilder response = new StringBuilder();
            while ((decodedLength = inputStream.read(buffer, 0, buffer.length)) > 0)
                response.append(new String(buffer, 0, decodedLength));
			System.out.println(response.toString());
		}catch(Exception e){
			e.printStackTrace();
		}finally{
			if (session != null) session.disconnect();
			if (channelExec != null) channelExec.disconnect();
			System.out.println(" --------------======= CONNECTION END =======-------------- ");
		}
		return r;
	}
```

```java
	private void connectSSH(String user, String host, String privateKey, int port){
		System.out.println(" --------------======= CONNECTION START =======-------------- ");
		try{
			JSch jsch = new JSch();
			jsch.addIdentity(privateKey);
			session = jsch.getSession(user, host, port);
			session.setConfig("StrictHostKeyChecking","no");
			session.connect();
		}catch(Exception e){
			e.printStackTrace();
		}
	}    
```

```xml
		<dependency>
			<groupId>com.jcraft</groupId>
			<artifactId>jsch</artifactId>
			<version>0.1.54</version>
		</dependency>

		<dependency>
			<groupId>com.amazonaws</groupId>
			<artifactId>aws-java-sdk-ec2</artifactId>
			<version>1.12.322</version>
		</dependency>
```

```js
$("#aws_ssh_connect_submit").click(function(){
	var confirmWindow = $("#confirmDbDeleteWindow").data("kendoWindow");
	confirmWindow.title("TEST");
	confirmWindow.content($("#confirm_template").html()).center().open();
	$("#confirmDbDeleteWindow .confirm-message").html("CONNECTION 시도");
	var request = new Object();
	request.cmd = "aws_ssh_test";
	request.data = new Object();
	$("#confirmDbDeleteWindow #confirmBtn").click(function(){
		$.ajax({
			type: "POST",
			url: "../controller/config_system",
			contentType: 'application/json',
			dataType: "json",
			data: JSON.stringify(request),
			success: function(response) {
				viewDialog("run", "info");
				confirmWindow.close();
			}
		});
	});
	$("#confirmDbDeleteWindow #CancleBtn").click(function(){
		confirmWindow.close();
	});
});
```


