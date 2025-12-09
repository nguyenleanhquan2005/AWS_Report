---
title: "Blog 2"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

## [AWS for Industries](https://aws.amazon.com/blogs/industries/)

# Connect to automotive or manufacturing plant displays using VNC and AWS IoT Secure Tunneling

by Andrew Timpone, Jonathan Johnson, and Joseph O'Such on 18 SEP 2025 in [Automotive](https://aws.amazon.com/blogs/industries/category/industries/automotive/), [Industries](https://aws.amazon.com/blogs/industries/category/industries/), [Internet of Things](https://aws.amazon.com/blogs/industries/category/internet-of-things/) [Permalink](https://aws.amazon.com/blogs/industries/connect-to-automotive-or-manufacturing-plant-displays-using-vnc-and-aws-iot-secure-tunneling/)  [Comments](https://aws.amazon.com/blogs/industries/connect-to-automotive-or-manufacturing-plant-displays-using-vnc-and-aws-iot-secure-tunneling/#Comments)  [Share](https://aws.amazon.com/blogs/industries/connect-to-automotive-or-manufacturing-plant-displays-using-vnc-and-aws-iot-secure-tunneling/#)

In the rapidly evolving automotive industry, remote access to vehicle displays has become a game-changer for maintenance, development, and support. By using VNC technology and AWS IoT Secure Tunneling, automotive professionals can now connect to remote vehicle displays for a wide array of applications. This capability enables remote diagnostics and troubleshooting, allowing technicians to securely access infotainment systems or instrument cluster displays of vehicles in the field, while engineers can remotely view and interact with advanced driver assistance system (ADAS) displays to help determine the need for debugging and tuning. During over-the-air (OTA) updates, engineers can monitor the progress and status screens remotely to confirm successful deployment. Fleet managers can use this technology to view driver information displays or telematics screens of multiple vehicles to help monitor performance, fuel efficiency, and improve driver behavior. In automotive manufacturing, quality control personnel can benefit from the ability to remotely access and inspect digital displays on vehicles moving through assembly lines. For vehicle testing and development, test engineers can remotely access prototype vehicle displays during road tests or simulations to help enhance safety and efficiency in testing procedures. Beyond these applications, the technology opens up possibilities for various other maintenance and support use cases, including onboard troubleshooting and guided co-browsing experiences, revolutionizing the way automotive professionals interact with and support vehicles in the field.

### **Overview of solution**

This secure connectivity solution makes use of IoT Secure Tunneling to establish bidirectional communication to remote devices over a secure connection that is managed by [AWS IoT](https://aws.amazon.com/iot/). The solution uses a client machine with VNC Viewer installed, an intermediary server that acts as a secure local proxy, and an automotive display that you are connecting to.

![Figure 1 Architecture IoT Secure Tunneling components and messages](/images/image1_02.png)

*Figure 1: Architecture IoT Secure Tunneling components and messages*

### **Prerequisites**

For this walkthrough, you should have the following prerequisites:

* An [AWS account](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fportal.aws.amazon.com%2Fbilling%2Fsignup%2Fresume&client_id=signup)
* A VPC with a private subnet
* Basic familiarity with Linux commands and AWS IoT services
* A device to act as your IoT thing (destination)
  * Recommend using a Raspberry Pi 4
  * VNC server installed and enabled
  * AWS CLI installed and configured
  * AWS IoT secure tunneling local proxy binary
* A server to act as a proxy relay (source)
  * This can be an EC2 instance or ECS/EKS container
  * AWS CLI installed and configured
  * AWS IoT Secure Tunneling local proxy binary or container
* A client machine
  * Recommend using either a Windows Bastion host in AWS or a personal computer
  * VNC viewer installed
  * Network connectivity to your proxy relay (source)

### **Walkthrough**

#### Setup your Raspberry Pi

The first step is to install and enable the VNC server, configure your Raspberry Pi as an IoT thing, and install or build the IoT Secure Tunneling local proxy.

Note if the Pi does not recognize that it has a display to share, connect it to a monitor.

* Install VNC server:

```bash
sudo apt update
sudo apt install realvnc-vnc-server
```

* Enable VNC server through raspi-config:

```bash
sudo raspi-config
Navigate to "Interface Options" > "VNC" > Enable

```


* Set VNC password:

```bash
vncpasswd
```

#### Configure your Raspberry Pi as an IoT thing

* Register your Raspberry Pi as an AWS IoT thing:

```bash
aws iot create-thing --thing-name 
raspberry-pi-vnc
```

* Create required IoT policies and certificates (see detailed steps in setup instructions):

```bash
  aws iot create-policy \
  --policy-name SecureTunnelingPolicy \
  --policy-document '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": "iotsecuretunneling:RotateTunnelAccessToken",
        "Resource": "*"
      },
      {
        "Effect": "Allow",
        "Action": "iotsecuretunneling:SubscribeToTunnel",
        "Resource": "*"
      },
      {
        "Effect": "Allow",
        "Action": "iot:Connect",
        "Resource": "*"
      },
      {
        "Effect": "Allow",
        "Action": "iot:Subscribe",
        "Resource": "*"
      }
    ]
  }'
```

* Follow the AWS IoT documentation to create and download certificates ([here](https://docs.aws.amazon.com/iot/latest/developerguide/device-certs-create.html)):

```bash
  aws iot create-keys-and-certificate \
  --set-as-active \
  --certificate-pem-outfile certificate_filename.pem \
  --public-key-outfile public_filename.key \
  --private-key-outfile private_filename.key
```

* Attach the policy to the certificate:

```bash
aws iot attach-policy --policy-name SecureTunnelingPolicy --target <certificate-arn>
```

* Install and configure the local proxy (instructions can be found AWS IoT Secure Tunneling Local Proxy or in the Setup Local Proxy sections below)

#### Create an IoT secure tunnel

* From the command line, issue the following AWS CLI command to your account and region where you are deploying your resources:
aws iotsecuretunneling open-tunnel \

```bash
--destination-config thingName=raspberry-pi-vnc,services=VNC \
--region <your-aws-region>
```

* The command will return a JSON payload that contains the `sourceAccessToken` and `destinationAccessToken` (see documentation here). Note these down for later use. Best practice is to place the source and destination tokens into their own .txt files for easy use.

#### Setup the local proxy using a binary file

Within the private subnet, create an EC2 Ubuntu instance and follow the steps from [AWS IoT Secure Tunneling Local Proxy](https://github.com/aws-samples/aws-iot-securetunneling-localproxy) to build the local proxy binary. Ensure that you can communicate from the Windows bastion host (described below) or personal device to the EC2 Ubuntu instance over TCP port 5000, and the instance has an IAM Role allowing Systems Manager access.

* Connect into the EC2 Ubuntu instance via AWS Session Manager or SSH.
* Run the following command from the command line from the directory that :

```bash
the localproxy binary is located
./localproxy -s 5000 -b 0.0.0.0 -r <your-aws-region> -t <sourceAccessToken>
```

* On your Raspberry Pi, run the following command from the command to start the local proxy binary:

```bash
./localproxy -d 5000 -b 0.0.0.0 -r <your-aws-region> -t <destinationAccessToken>
```

#### Setup the local proxy using a container

Reference the [Docker documentation](https://docs.docker.com/engine/install/debian/) to install docker before proceeding. The local proxy container can run on Amazon Elastic Kubernetes Service (EKS) and Amazon Elastic Container Service (ECS), if desired.

To download the container image and run on your EC2 instance, run the following command from the folder where your source token is stored `sourceToken.txt`. Replace `<distro>` with the appropriate distribution option for your machine from the list here:

```bash
  docker run -d -it --network=host \
  public.ecr.aws/aws-iot-securetunneling-localproxy/<distro>-bin:amd64-latest \
  --region us-east-1 \
  -s 5000 \
  -c /etc/ssl/certs \
  -t $(cat sourceToken.txt) \
  -b 0.0.0.0
```

For raspberry Pi, run the following command where your destinationToken is stored `destinationToken.txt`. Replace `<distro>` with the appropriate distribution option for your machine from the list here:

```bash
  docker run -d -it --network=host \
  public.ecr.aws/aws-iot-securetunneling-localproxy/<distro>-bin:amd64-latest \
  --region us-east-1 \
  -d 5000 \
  -c /etc/ssl/certs \
  -t $(cat destinationToken.txt)
```

#### Setup the Windows bastion host with VNC Viewer

![Figure 2 VPC with bastion and Ubuntu local proxy](/images/image2_02.png)

*Figure 2: VPC with bastion and Ubuntu local proxy*

In order to experiment with IoT Secure Tunnel and the IoT LocalProxy, create a VPC with a public subnet containing a Windows bastion host, Internet Gateway, and NAT Gateway.

If you are going to use a personal device as the client machine, place your Ubuntu EC2 instance with LocalProxy in this public subnet instead.

#### Setup the client machine

On your client machine (either Windows Bastion host or personal computer), you will need to install a VNC viewer application. Follow the instructions here. If you are using a personal device, please ensure your EC2 proxy is in a public subnet.

* Get the IP address of the EC2 Ubuntu instance. If using your personal device, make sure this is the public IP address.

![VNC Viewer application](/images/image3_02.png)

* Open the VNC Viewer application. Create a new connection. For the connection address, use the IP Address of the Ubuntu server and specify the port as 5000 (i.e. `172.31.59.116:5000`)

*If you are using a personal computer, you will need to make sure your Ubuntu server has a public IP and is in a public subnet*

* When prompted, enter the authentication information (i.e. username and password you setup on the Raspberry Pi for VNC)

### **Cleaning up**

To help avoid incurring unnecessary future charges, stop using resources that are no longer needed:

* Delete the AWS IoT thing, certificates, and policies you created
* Close and delete any open tunnels using the AWS IoT console or CLI
* Stop the VNC server if no longer needed
* Terminate and delete any EC2 instances or ECS containers created
* Delete your VPC and VPC resources you created

### **Conclusion**

In this post, we demonstrated how to establish secure user interface access to an IoT thing using AWS IoT Secure Tunneling and VNC. This solution can be adapted for various automotive, manufacturing, and industrial IoT scenarios where secure direct screen sharing access is crucial. Examples include remote diagnostics of vehicle infotainment systems and real-time monitoring of manufacturing equipment displays. We also shared details on leveraging built-in IoT messaging to automate the setup and teardown of the IoT secure tunnel and local proxy. Furthermore, this blog presented an architecture showcasing how to expand IoT secure tunnel automation with an AWS serverless architecture. This approach helps efficiently manage the creation and termination of IoT secure tunnels across a fleet of IoT things, whether they're connected to vehicle or other displays. By implementing these solutions, automotive manufacturers and industrial operators can help significantly enhance their remote monitoring, troubleshooting, and maintenance capabilities, creating the potential for improved efficiency and reduced downtime.

To learn more, check out the [AWS IoT Secure Tunneling documentation](https://docs.aws.amazon.com/iot/latest/developerguide/secure-tunneling.html) or explore other [AWS IoT solutions](https://aws.amazon.com/iot/).

---

### **About the authors**

![Andrew Timpone](/images/image4_02.png)

**Andrew Timpone**

Six years of experience in cloud architecture, Andrew helps large enterprise customers solve their business problems using AWS. Andrew has over 25 years of IT experience with expertise in enterprise integration patterns. Andrew is married with three children and resides just south of Cleveland, OH, where he enjoys bicycle riding, archery, and vegetable gardening.

![Jonathan Johnson](/images/image5_02.jpg)

**Jonathan Johnson**

Jonathan Johnson ("JJ") is a Senior Solutions Architect AWS. He has worked in the cloud architecture space for 10 years and 25+ years in IT & application development. He enjoys working with customers to design solutions for complex business requirements. More recently, JJ has been a champion of using Amazon Q Developer to build out solutions. When not designing AWS architectures, JJ can be found on his old Hunter sailboat 'Seas the Day'.

![Joseph O'Such](/images/image6_02.png)

**Joseph O'Such**

Joseph O'Such is a Partner Solutions Architect at AWS primarily supporting companies in the AWS Partner Network. Joseph often engages with IoT related projects and partners, stemming from his background in Mechanical engineering. Joseph lives and works in the Northern Virginia area outside of Washington DC, where he enjoys being active, including rock climbing, volleyball, and running.

[image1]: /images/image1.png
[image2]: /images/image2.png
[image3]: /images/image3.png
[image4]: /images/image4.png
[image5]: /images/image5.png
[image6]: /images/image6.png
