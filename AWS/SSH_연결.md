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
```
import com.jcraft.jsch.*;

public class JConnectEC2shell{
    //여기에 .pem 파일의 절대경로를 지정한다.
    private static String keyname = "pem file";
    //여기에 EC2 instance 도메인 주소를 적는다.
    private static String publicDNS = "EC2 instance 주소";
    public static void main(String[] arg){

        try{
            JSch jsch=new JSch();

            String user = "ec2-user";
            String host = publicDNS;
            int port = 22;
            String privateKey = keyname;

            jsch.addIdentity(privateKey);
            System.out.println("identity added ");

            Session session = jsch.getSession(user, host, port);
            System.out.println("session created.");

            // disabling StrictHostKeyChecking may help to make connection but makes it insecure
            // see http://stackoverflow.com/questions/30178936/jsch-sftp-security-with-session-setconfigstricthostkeychecking-no
            //
            session.setConfig("StrictHostKeyChecking","no");
            session.setConfig("GSSAPIAuthentication","no");
            session.setServerAliveInterval(120 * 1000);
            session.setServerAliveCountMax(1000);
            session.setConfig("TCPKeepAlive","yes");

            session.connect();

            Channel channel=session.openChannel("shell");

            // Enable agent-forwarding.
            //((ChannelShell)channel).setAgentForwarding(true);

            channel.setInputStream(System.in);
      /*
      // a hack for MS-DOS prompt on Windows.
      channel.setInputStream(new FilterInputStream(System.in){
          public int read(byte[] b, int off, int len)throws IOException{
            return in.read(b, off, (len>1024?1024:len));
          }
        });
       */

            channel.setOutputStream(System.out);

      /*
      // Choose the pty-type "vt102".
      ((ChannelShell)channel).setPtyType("vt102");
      */

      /*
      // Set environment variable "LANG" as "ja_JP.eucJP".
      ((ChannelShell)channel).setEnv("LANG", "ja_JP.eucJP");
      */

            //channel.connect();
            channel.connect(3*1000);
        }
        catch(Exception e){
            e.printStackTrace();
        }
    }
}
```
