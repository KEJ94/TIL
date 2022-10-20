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
			<version>1.11.245</version> // 높은 버전을 사용할것.. platform detail 이 없음..
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
## 4. EC2 인스턴스에 대한 정보 불러오기
```
package com.jiransnc.vada.aws;
import java.util.List;
import com.amazonaws.regions.Regions;
import com.amazonaws.services.ec2.AmazonEC2;
import com.amazonaws.services.ec2.AmazonEC2ClientBuilder;
import com.amazonaws.services.ec2.model.DescribeImagesRequest;
import com.amazonaws.services.ec2.model.DescribeImagesResult;
import com.amazonaws.services.ec2.model.DescribeInstancesRequest;
import com.amazonaws.services.ec2.model.DescribeInstancesResult;
import com.amazonaws.services.ec2.model.Image;
import com.amazonaws.services.ec2.model.Instance;
import com.amazonaws.services.ec2.model.Reservation;

public class DescribeInstances{
    public static void main(String[] args){
        // 리전 서울로 지정
        final AmazonEC2 ec2 = AmazonEC2ClientBuilder.standard().withRegion(Regions.AP_NORTHEAST_2).build();
        
        boolean done = false;
        DescribeInstancesRequest request = new DescribeInstancesRequest();
        DescribeImagesRequest img_request = new DescribeImagesRequest();

        // AMI 정보
        img_request.withImageIds("ami-026ad5c508c016f6f"); // instance.getImageId()
        DescribeImagesResult describeImagesResult = ec2.describeImages(img_request);
        List<Image> images = describeImagesResult.getImages();
        for (Image image : images) {
            System.out.println(image.getImageId() + "==" + image.getImageLocation() + "==" + image.getName());
        }
        
        String instanceStr = "";

        // 인스턴스 정보
        while(!done) {
            DescribeInstancesResult response = ec2.describeInstances(request);
            for(Reservation reservation : response.getReservations()) {
                for(Instance instance : reservation.getInstances()) {
                    instanceStr = 
                        "\n" +
                        "-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-\n" +
                        "Architecture : "                   +instance.getArchitecture()+"\n"+
                        "ClientToken : "                    +instance.getClientToken()+"\n"+
                        "Hypervisor : "                     +instance.getHypervisor()+"\n"+
                        "ImageId : "                        +instance.getImageId()+"\n"+
                        "InstanceId : "                     +instance.getInstanceId()+"\n"+
                        "InstanceLifecycle : "              +instance.getInstanceLifecycle()+"\n"+
                        "InstanceType : "                   +instance.getInstanceType()+"\n"+
                        "KernelId : "                       +instance.getKernelId()+"\n"+
                        "KeyName : "                        +instance.getKeyName()+"\n"+
                        "Platform : "                       +instance.getPlatform()+"\n"+
                        "PrivateDnsName : "                 +instance.getPrivateDnsName()+"\n"+
                        "PrivateIpAddress : "               +instance.getPrivateIpAddress()+"\n"+
                        "PublicDnsName : "                  +instance.getPublicDnsName()+"\n"+
                        "PublicIpAddress : "                +instance.getPublicIpAddress()+"\n"+
                        "RamdiskId : "                      +instance.getRamdiskId()+"\n"+
                        "RootDeviceName : "                 +instance.getRootDeviceName()+"\n"+
                        "RootDeviceType : "                 +instance.getRootDeviceType()+"\n"+
                        "SpotInstanceRequestId : "          +instance.getSpotInstanceRequestId()+"\n"+
                        "SriovNetSupport : "                +instance.getSriovNetSupport()+"\n"+
                        "StateTransitionReason : "          +instance.getStateTransitionReason()+"\n"+
                        "SubnetId : "                       +instance.getSubnetId()+"\n"+
                        "VirtualizationType : "             +instance.getVirtualizationType()+"\n"+
                        "VpcId : "                          +instance.getVpcId()+"\n"+
                        "AmiLaunchIndex : "                 +instance.getAmiLaunchIndex()+"\n"+
                        "BlockDeviceMappings : "            +instance.getBlockDeviceMappings()+"\n"+
                        "EbsOptimized : "                   +instance.getEbsOptimized()+"\n"+
                        "ElasticGpuAssociations : "         +instance.getElasticGpuAssociations()+"\n"+
                        "EnaSupport : "                     +instance.getEnaSupport()+"\n"+
                        "IamInstanceProfile : "             +instance.getIamInstanceProfile()+"\n"+
                        "LaunchTime : "                     +instance.getLaunchTime()+"\n"+
                        "Monitoring : "                     +instance.getMonitoring()+"\n"+
                        "NetworkInterfaces : "              +instance.getNetworkInterfaces()+"\n"+
                        "Placement : "                      +instance.getPlacement()+"\n"+
                        "ProductCodes : "                   +instance.getProductCodes()+"\n"+
                        "SecurityGroups : "                 +instance.getSecurityGroups()+"\n"+
                        "SourceDestCheck : "                +instance.getSourceDestCheck()+"\n"+
                        "State : "                          +instance.getState()+"\n"+
                        "StateReason : "                    +instance.getStateReason()+"\n"+
                        "Tags : "                           +instance.getTags()+"\n"+
                        "-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-\n";
                        System.out.print(instanceStr);
                }
            }
            request.setNextToken(response.getNextToken());
            if(response.getNextToken() == null) {
                done = true;
            }
        }
    }
}
```  
<br><br><br>
참조 : https://docs.aws.amazon.com/code-samples/latest/catalog/java-ec2-pom.xml.html
<br>
참조 : https://docs.aws.amazon.com/code-samples/latest/catalog/java-ec2-src-main-java-aws-example-ec2-DescribeInstances.java.html

