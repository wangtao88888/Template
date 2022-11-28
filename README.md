<h1 align="center">RequestTemplate</h1>

中文文档 | [English](https://github.com/1n7erface/RequestTemplate/blob/main/README_EN.md)
## 始终相信代码有别，好的项目总是需要给用户时间去发现......

![](https://img.shields.io/badge/KCon-%E5%85%B5%E5%99%A8%E8%B0%B1-red)
![](https://img.shields.io/badge/ReaTeam-%E6%AD%A6%E5%99%A8%E5%BA%93-red) ![](https://img.shields.io/badge/version-1.0.6-brightgreen) ![](https://img.shields.io/badge/author-1n7erface-blueviolet)


# 0x00 工具介绍

RequestTemplate是一款两端并用的红队渗透工具以及甲方自查工具，其在内网渗透过程中有着不可替代的作用。客户端使用Golang以其精巧、快速的特点打造而成,快速发现内网中脆弱的一环。复现端使用Java以其生态稳定、跨平台、UI精美的特点打造而成,最小的发包量和平台的集成性验证脆弱的一环。

# 0x01 应用场景
- 红蓝对抗中红队的内网利器
- 甲方建设中内网的自查帮手

# 0x02 具备特点

- 网段探测:   检测当前机器连通的网段情况
- 横向移动:   多种弱口令爆破模块,可通过同目录下config.json配置
- WEB扫描:   集成Xray三百多种POC检测
- 漏洞验证:   使用Java端配置代理对扫描结果进行复现验证截图

# 0x03 RequestTemplate Client Usage

```bash
root@VM-4-13-ubuntu:~# ./App_darwin -h

 _____                    _       _
|_   _|                  | |     | |
  | | ___ _ __ ___  _ __ | | __ _| |_ ___
  | |/ _ \ '_'  _ \| '_ \| |/ _' | __/ _ \
  | |  __/ | | | | | |_) | | (_| | ||  __/
  \_/\___|_| |_| |_| .__/|_|\__,_|\__\___|
                   | |  by 1n7erface
                   |_|
Usage of ./App_darwin:
  -a string
    	auto check network conn (default "false")
  -b string
    	only brute , not webscan (default "false")
  -c string
    	auto check 192 or 172 or 10
  -e string
    	print error log (default "false")
  -i string
    	IP address of the host you want to scan,for example: 192.168.11.11-255 or 192.168.1.1/24 or /22 /15...
  
```
# 0x04 RequestTemplate Client 参数讲解

- -a true :只会去探测网段的连通性,探测包括10.1.1.1-10.255.255.255 和 192.168.1.1-192.168.255.255 和 172.16.1.1-172.31.255.255

- -b true :默认扫描端会进行web漏洞的扫描和弱口令的爆破,此参数只会进行爆破,如果需要对收集到的密码进行频繁的测试加上此参数

- -c 192 or 172 or 10 :对10.1.1.1-10.255.255.255 和 192.168.1.1-192.168.255.255 和 172.16.1.1-172.31.255.255的连通性进行测试,测试完后进行漏洞扫描和服务口令爆破

- -e true :默认扫描端只会对存活IP存活端口存在漏洞的信息进行打印,加上此参数可以将探测信息进行输出,通常用于错误调试

- -i CIDR :此参数支持IP地址的CIDR表达式,但如果对10/16/8，192/16/8，172/16/8的扫描建议使用-c参数,此参数用于/24最常用

- -i与-c的区别:-c参数会对网段连通性进行探测，探测完毕进行扫描。而-i则直接进行扫描

# 0x05 RequestTemplate Server Usage (need JDK1.8)

```bash
root@VM-4-13-ubuntu:~# java -jar RequestTemplate.jar 
 _____                    _       _       
|_   _|                  | |     | |      
  | | ___ _ __ ___  _ __ | | __ _| |_ ___ 
  | |/ _ \ '_' _  \| '_ \| |/ _' | __/ _ \
  | |  __/ | | | | | |_) | | (_| | ||  __/
  \_/\___|_| |_| |_| .__/|_|\__,_|\__\___|
                   | |  by 1n7erface
                   |_|
Opened database successfully

```
# 0x06 RequestTemplate Server 参数讲解

- 代理管理
<img width="799" alt="image" src="https://user-images.githubusercontent.com/52184829/174012441-a675348b-e3fd-45de-ad7f-da2b0aa57ec3.png">

- 目标管理
<img width="796" alt="image" src="https://user-images.githubusercontent.com/52184829/174012776-6fc00c43-0357-4b8f-9353-a4cb657ecacd.png">

- 攻击利用
<img width="794" alt="image" src="https://user-images.githubusercontent.com/52184829/174012945-4c3b1c10-0b4e-44e1-8964-b38189ef3d4e.png">

# 0x07 config.json 参数讲解

<img width="600" alt="image" src="https://user-images.githubusercontent.com/52184829/174014625-831f94ce-cfb6-4800-889c-771df224abd4.png">

- 将config.json放置与扫描端同一目录下,即可对扫描端的字典,端口进行添加
- 注意:程序中默认自带简易字典和端口,账号密码的添加只需要添加信息收集到的复杂密码，端口应排除以下默认端口进行添加

``` shell
Ports = []int{21, 22, 23, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 98, 99, 135, 139, 443, 445, 800, 801, 808, 880, 888, 889, 1000, 1010, 1080, 1081, 1082, 1118, 1433, 1521, 1888, 2008, 2020, 2100, 2375, 2379, 3000, 3008, 3128, 3306, 3505, 5432, 5555, 6080, 6379, 6648, 6868, 7000, 7001, 7002, 7003, 7004, 7005, 7007, 7008, 7070, 7071, 7074, 7078, 7080, 7088, 7200, 7680, 7687, 7688, 7777, 7890, 8000, 8001, 8002, 8003, 8004, 8006, 8008, 8009, 8010, 8011, 8012, 8016, 8018, 8020, 8028, 8030, 8038, 8042, 8044, 8046, 8048, 8053, 8060, 8069, 8070, 8080, 8081, 8082, 8083, 8084, 8085, 8086, 8087, 8088, 8089, 8090, 8091, 8092, 8093, 8094, 8095, 8096, 8097, 8098, 8099, 8100, 8101, 8108, 8118, 8161, 8172, 8180, 8181, 8200, 8222, 8244, 8258, 8280, 8288, 8300, 8360, 8443, 8448, 8484, 8800, 8834, 8838, 8848, 8858, 8868, 8879, 8880, 8881, 8888, 8899, 8983, 8989, 9000, 9001, 9002, 9008, 9010, 9043, 9060, 9080, 9081, 9082, 9083, 9084, 9085, 9086, 9087, 9088, 9089, 9090, 9091, 9092, 9093, 9094, 9095, 9096, 9097, 9098, 9099, 9100, 9200, 9443, 9448, 9800, 9981, 9986, 9988, 9998, 9999, 10000, 10001, 10002, 10004, 10008, 10010, 10250, 11211, 12018, 12443, 14000, 16080, 18000, 18001, 18002, 18004, 18008, 18080, 18082, 18088, 18090, 18098, 19001, 20000, 20720, 21000, 21501, 21502, 27017, 28018, 20880}
```

# 0x07 感谢

感谢@shadow1ng 对扫描端疑问的解答
https://github.com/shadow1ng/fscan

感谢@j1anFen 的项目对可复现端的参考
https://github.com/SafeGroceryStore/MDUT

