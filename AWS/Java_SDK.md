## 1. 라이센스 설정
아래와 같은 내용으로 확장자가 없는 txt 파일을 만든다.
```
[default]
aws_access_key_id = YOUR_AWS_ACCESS_KEY_ID
aws_secret_access_key = YOUR_AWS_SECRET_ACCESS_KEY
```
운영체제별 파일경로
- Windows : C:\Users\<yourUserName>\.aws\credentials
- Linux, macOS, Unix : ~/.aws/credentials
<br><br><br>
## 2. IAM 계정 정책
- AdministratorAccess 
<br><br><br>
## 3. pom.xml 설정
```
	<dependencyManagement>
		<dependencies>
		<dependency>
			<groupId>com.amazonaws</groupId>
			<artifactId>aws-java-sdk-bom</artifactId>
			<version>1.11.245</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
		</dependencies>
	</dependencyManagement>
```

```
  <dependencies>
		<dependency>
			<groupId>com.amazonaws</groupId>
			<artifactId>aws-java-sdk-ec2</artifactId>
		</dependency>
  </dependencies>
```
<br><br>
## 4. EC2 인스턴스에 대한 정보 추출 코드
```
package com.jiransnc.vada.aws;
import com.amazonaws.regions.Regions;
import com.amazonaws.services.ec2.AmazonEC2;
import com.amazonaws.services.ec2.AmazonEC2ClientBuilder;
import com.amazonaws.services.ec2.model.DescribeInstancesRequest;
import com.amazonaws.services.ec2.model.DescribeInstancesResult;
import com.amazonaws.services.ec2.model.Instance;
import com.amazonaws.services.ec2.model.Reservation;

public class DescribeInstances{
    public static void main(String[] args){
        String instanceStr = "";
        // *리전 서울로 지정
        final AmazonEC2 ec2 = AmazonEC2ClientBuilder.standard().withRegion(Regions.AP_NORTHEAST_2).build();
        boolean done = false;
        DescribeInstancesRequest request = new DescribeInstancesRequest();
        while(!done) {
            DescribeInstancesResult response = ec2.describeInstances(request);
            for(Reservation reservation : response.getReservations()) {
                for(Instance instance : reservation.getInstances()) {
                    instanceStr = 
                        "-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-\n" +
                        "InstanceId : "         +instance.getInstanceId()+"\n"+
                        "ImageId : "            +instance.getImageId()+"\n"+
                        "InstanceType : "       +instance.getInstanceType()+"\n"+
                        "privateIpAddress : "   +instance.getPrivateIpAddress()+"\n"+
                        "publicIpAddress : "    +instance.getPublicIpAddress()+"\n"+
                        "State : "              +instance.getState()+"\n"+
                        "SubnetId : "           +instance.getSubnetId()+"\n"+
                        "VpcId : "              +instance.getVpcId()+"\n"+
                        "Tags : "               +instance.getTags()+"\n"+
                        "PlatformDatails :  "   +instance.getPlatform()+"\n"+
                        "NetworkInterfaces : "  +instance.getNetworkInterfaces()+"\n"+
                        "-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-\n";
                }
            }
            request.setNextToken(response.getNextToken());
            if(response.getNextToken() == null) {
                done = true;
            }
        }
        System.out.print(instanceStr);
    }
}
```  
<br><br><br>
참조 : https://docs.aws.amazon.com/code-samples/latest/catalog/java-ec2-pom.xml.html
<br>
참조 : https://docs.aws.amazon.com/code-samples/latest/catalog/java-ec2-src-main-java-aws-example-ec2-DescribeInstances.java.html

