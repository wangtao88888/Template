# Template - 启发式内网扫描
![GitHub Repo stars](https://img.shields.io/github/stars/1n7erface/Template?color=success)
![GitHub forks](https://img.shields.io/github/forks/1n7erface/Template)
![GitHub all release](https://img.shields.io/github/downloads/1n7erface/Template/total?color=blueviolet)
![](https://img.shields.io/badge/KCon-%E5%85%B5%E5%99%A8%E8%B0%B1-red)
## 0x01 免责声明
本工具旨在提供安全评估和漏洞扫描等相关服务，但使用本工具时请注意以下事项：
- 本工具的使用者应对其使用产生的结果和后果负全部责任。本工具仅作为辅助工具提供，不对使用者所进行的操作和决策承担责任。
- 本工具尽力提供准确、及时的信息和评估，但无法保证其完全无误。使用者应自行判断和验证本工具提供的信息，并对使用本工具所产生的结果进行独立评估。

请在使用本工具之前仔细阅读并理解上述免责声明。使用本工具即表示您同意遵守上述条款，并自行承担相应责任。
## 0x02 工具优势
- 所有模块皆采用生产者消费者模型,即生即消.
> 在端口扫描一组数据后将数据发送到队列中,由爆破模块和指纹模块即刻进行消费,随时结束扫描进程拿到扫描结果.摆脱传统的等待端口扫描结束进行后续的模型.
- 所有模块皆采用启发式扫描,诣在最少的发包探测目标
> 在端口扫描时,通过协议识别的方式识别TOP15协议,对其协议进行探测.漏洞探测是通过对WEB指纹进行识别后进行探测.摆脱传统的端口绑定以及漏洞探测发包量大的问题.
- 强大的WEB指纹支撑
> 感谢棱角社区对本工具WEB指纹的支撑、目前指纹900+,指纹识别快人一步.
- 极致的应用并发
> 在爆破模块以及漏洞探测、指纹识别、端口扫描所有模块采用数据原子化的方式进行极致的并发.
## 0x03 参数说明
```
❯ ./App-arm64darwin-noupx

 _____                    _       _
|_   _|                  | |     | |
  | | ___ _ __ ___  _ __ | | __ _| |_ ___
  | |/ _ \ '_'  _ \| '_ \| |/ _' | __/ _ \
  | |  __/ | | | | | |_) | | (_| | ||  __/
  \_/\___|_| |_| |_| .__/|_|\__,_|\__\___|
                   | |  by 1n7erface
                   |_|
[=] Load Success 
Usage of ./App-arm64darwin-noupx:
  -bt int
    	BruteModule threadNum (default 200)
  -c string
    	auto check 192 or 172 or 10
  -e	print error log
  -i string
    	IP address of the host you want to scan,for example: 192.168.11.11-255 or 192.168.1.1/24 or /22 /15...
  -it int
    	InfoModule threadNum (default 200)
  -nobrute
    	skip brute
  -noping
    	skip icmp alive
  -nopoc
    	skip poc
  -onping
    	only ping
  -p string
    	custom port example: 80,8088,1-3000
  -pw string
    	Define a password dictionary for blasting
  -t int
    	Timeout (default 4)
  -us string
    	Define a user dictionary for blasting
[=] end...... 
```
- bt参数说明
> 此参数期望接收一个数值类型,用于爆破模块开启的协程数量,默认的数量为200
- c参数说明
> 此参数期望接收一个字符串，存在于"192、172、10"三个字符串之间,程序会自动的探测网段存活，并对其进行扫描。包括192的192.168.0.1-192.168.255.255、172的172.16.0.1-172.31.255.255、10的10.0.0.1-10.255.255.255.值得一提的是,网段探测存活的算法是从每一个c段选择1,255 以及期间的随机3个ip进行检测,如果一个存活将判定为网段存活.在一定几率下存在漏段的可能无法避免.(10段不建议使用,网段太大可能产生预期之外的错误).
- e参数说明
> 此参数不接收任何值,打印期间的错误日志,用于排查扫描的问题.
- i参数说明
> 此参数接收一个字符串,字符串用于声明要扫描的网段,例如192.168.11.11-255 or 192.168.1.1/24 or /22 /15... ,此参数支持CIDR的表达式.
- it参数说明
> 此参数期望接收一个数值类型,用于信息探测模块开启的协程数量,例如ip存活、端口扫描、web指纹.
- nobrute参数说明
> 此参数不接收任何值,不对信息探测的结果进行暴力破解模块.
- noping参数说明
> 此参数不接收任何值,在目标网段不支持ICMP协议时,通过TCP进行探测.
- nopoc参数说明
> 此参数不接收任何值,不对探测的WEB服务进行漏洞探测的模块。
- onping参数说明
> 此参数不接收任何值,对目标网段只进行ICMP的ip存活探测,其余一律不做.
- p参数说明
> 此参数接收一个字符串,指定端口扫描的端口.例如,80,8088,1-3000 (注:此参数一经指定则不进行程序自带端口的扫描)
- pw参数说明
> 此参数接收一个字符串,指定爆破的密码字典,例如 -pw ffnxjfl123,fgmgergn334 （注:此参数一经指定则不进行程序自带密码的扫描)
- t参数说明
> 此参数期望接收一个数值类型,用于在漏洞扫描,WEB识别时的超时时间设置.
- us参数说明
> 此参数期望接收一个字符串,指定爆破的用户名字典,例如 -us fwefwf,fwefwf  (注:此参数一经指定则不进行程序自带用户名的扫描)
- 如果想指定端口、爆破的用户名和密码并且仍然使用程序自带的端口、密码、用户名进行扫描.可以在当前程序的同级目录上传名为"config.json"的文件
> 内容为:   {"pass":["ffnxjfl123","fgmgergn334"],"user":["fwefwf","fwefwf"],"ports":[9999,8888]}
## 0x04 使用截图
<img width="1000" alt="image" src="https://github.com/1n7erface/Template/assets/52184829/e14e0992-2931-4c57-a502-e3d029f41020">
<img width="1000" alt="image" src="https://github.com/1n7erface/Template/assets/52184829/42b66291-57dc-40fe-9f13-6dc75ed6fb48">

## 0x05 写在最后
> 对待一个产品或工具,我希望注入自己百分百的心血与付出,我可以一直去重构直到我认为的满意,这大概是一个技术人的执着.
> 
> 不有趣,毋宁死.

