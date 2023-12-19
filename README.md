
## kms.sh

- Description: Auto install KMS Server
- Intro: https://teddysun.com/530.html
- **KMS Server Docker Image**: https://hub.docker.com/r/teddysun/kms

KMS is the abbreviation of Key Management System, which is the key management system. The KMS mentioned here is undoubtedly the Windows and Office KMS used to activate the VOL version. You can often see the KMS server address provided by someone online, so have you ever thought that you will also come up with such a service？And such services already exist on GithubOpen source codeRealized.
This article is based on this open source code, and has developed a script for the one-key installation of KMS service for the three major Linux distribution editions.

The environment of this script
System support: CentOS 6+, Debian 7+, Ubuntu 12+
Virtual technology: any
Storage requirements: ≥128M
Date: October 25, 2018


About this script
1. This script applies to the three major Linux distribution editions, while other versions do not support it.
2. After the KMS service is installed, it will be added to the boot itself.
3. The default record log, which is located in /var/log/vlmcsd.log.

method of use
Use root user login to run the following commands：
```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/kms.sh &" chmod +x kms.sh &"./kms.sh
```
After the installation is completed, enter the following order to check the monitoring of TCP port 1688
```
netstat -nxtlp | grep 1688
```
The return value is similar to the following, which means OK：

tcp        000.0.0.0:16880.0.0.0:*                   LISTEN      3200/vlmcsd                                
tcp        00:::1688:::*                        LISTEN      3200/vlmcsd  

After the installation of this script is completed, the KMS service will be added to the boot itself.

Use command：
Start:/etc/init.d/kms start
Stop:/etc/init.d/kms stop
Restart:/etc/init.d/kms restart
Status:/etc/init.d/kms status

Unloading method：
Use root user login to run the following commands：
```
./kms.sh uninstall
```

How to use KMS service
KMS service, used to activate the Windows and Office of the VOL version online.
The premise of activation is that your system is a batch authorized version, that is, the VL version, and the general enterprise version is the VL version. The VL version of the mirror generally contains GVLK key for KMS activation.
The VL version of the product contained in the following list or the product that can use key to enter the KMS channel supports the use of KMS activation.

Office 2019 & Office 2016：https://docs.microsoft.com/en-us/DeployOffice/vlactivation/gvlks
Office 2013：https://technet.microsoft.com/zh-cn/library/dn385360.aspx
Office 2010：https://technet.microsoft.com/zh-cn/library/ee624355(v=office.14).aspx
Windows：https://docs.microsoft.com/zh-cn/windows-server/get-started/kmsclientkeys

Use administrator permission to run cmd to view the system version, the command is as follows：
```
wmic os get caption
```
Use administrator permission to run cmd to install the key obtained from the above list, the order is as follows：
```
slmgr /ipk xxxxx-xxxxxx-xxxxxx-xxxxxx-xxxxxx
```

Use administrator permission to run cmd to set the KMS server address as your own IP or domain name. It is best to add the end slogan （:1688） to the back. The order is as follows：

slmgr /skms Your IP or Domain:1688
note：This step is the work done in this script. When your KMS service is out of start-up state, you can set up your own KMS server address here.
Use administrator permission to run cmd manual activation system, the commands are as follows：
```
slmgr /ato
```

Regarding the activation of the Office, the requirement must be the VOL version, otherwise it cannot be activated.
Find your office installation catalog, 32 defaults are generally C:\Program Files (x86)\Microsoft Office\Office16
64 defaults are generally C:\Program Files\Microsoft Office\Office16
Office16 is Office 2016, Office15 is Office 2013, Office14 is Office 2010.
To open the catalog mentioned above, there should be an OSPP.VBS file.
Use administrator permission to run cmd into the Office Directory, the commands are as follows：

cd "C:\Program Files (x86)\Microsoft Office\Office16"
Use administrator permission to run cmd registered KMS server address：
```
cscript ospp.vbs /sethst:Your IP or Domain
```
Use administrator permission to run cmd manual activation Office, the order is as follows：
```
cscript ospp.vbs /act
```
note： KMS method activation, which is only valid for 180 days.

Every once in a while, the system will automatically request a renewal from the KMS server, please ensure that your own KMS service is operating normally.

Commonly wrong countermeasures
If you encounter a mistake in the execution process, please check according to the following steps：
1. Is your KMS server hung？
2. Is your KMS service open normally？
3. Is your system or office a batch VL version？
4. Has your system or Office modified Key or not installed GVLK Key？
5. Do you run cmd with administrator authority？
6. Is your network connection normal？
7. Is your local DNS analysis normal？
8. If you rule out the above countermeasures, please search for the reasons by the error prompt code.

Update log
October 25, 2018：Correct the git link of vlmcsd, that is, each new installation is the latest official version. Note: If you want to upgrade the version, you need to stop the kms service first, then delete the /usr/bin/vlmcsd file, and then download the latest script installation.

Reference link
https://03k.org/kms.html
