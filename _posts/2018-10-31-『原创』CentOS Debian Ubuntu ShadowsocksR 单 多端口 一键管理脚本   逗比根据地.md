## **文章目录**
#
* 本文最后更新于 2018年1月18日 12:32 可能会因为没有更新而失效。如已失效或需要修正，请留言！
* 最近经常有小白找我让我把他们安装ShadowsocksR服务端，一开始都是手动安装的，后来嫌麻烦，就打算用脚本，但是网上基本上只是安装一下就没了，只能算一键安装脚本，并不足够方便和适合懒人和小白，于是自己写了一个一键管理脚本，一键安装和一键管理的区别！
更多的Shadowsocks安装教程/一键脚本请看这里：[Shadowsocks指导篇](https://doub.ws/ss-jc26/#2.2.2、搭建Shadowsocks服务)
* 本脚本的 二维码图片链接，是调用我自建的 二维码API 来生成二维码图片( http://doub.pw/qr/qr.php?text=xxx )。
* 当访问API页面后，PHP网页文件会把 GET参数( ?text=xxx ) 传递给JS脚本，浏览器会加载JS脚本，然后由JS脚本根据 GET参数的文本 生成二维码图片！图片是在你本地浏览器中生成，服务器中不存在图片！
* 请确定你信任我和我的脚本，否则请不要用我的脚本，少BB！

## **系统要求**
#
> **CentOS 6+ / Debian 6+ / Ubuntu 14.04 +**
推荐**Debian 7 x64**，这个是我一直使用的系统，我的脚本在这个系统上面出错率最低。并且最容易安装锐速（锐速不支持OpenVZ）  
> CentOS根据大家的要求，加入了CentOS 6和7的支持，CentOS 7 自带防火墙问题(firewalld)自行解决，其他版本没有做测试。

### **脚本版本**
 >#### Ver: 2.0.37
 > 本脚本与另一个SSR脚本 [『原创』ShadowsocksR MudbJSON模式多用户一键脚本 支持流量限制等](https://doub.ws/ss-jc60/) 的区别是什么？  
 >ssrmu.sh 脚本是单服务器多用户脚本，使用的是 SSR服务端的MudbJSON模式，可以给每个用户(端口)设置不同的加密方式/协议/混淆/限制速度/设备数限制/可用总流量等功能。即实现单服务器多用户流量管理等功能。  
 >而 ssr.sh 则是单服务器单用户脚本，使用的是 SSR服务端的单用户配置方式，即使实现了多端口，但是还算不算多用户，不支持每个用户(端口)不同的加密方式/协议/混淆等，并且无法管理流量使用。  
 > **如何选择这两个脚本？**  
 > 根据你的需求选择，比如你仅仅是 一个或两个人使用，并且不需要流量管理功能，那么选择 ssr.sh 好了。而如果很多人使用，并且都需要限制流量来管理，那你适合使用 ssrmu.sh ，所以自己看着选，多试试（两个脚本不能共存）！

### 脚本特点：
>目前网上的各个ShadowsocksR脚本基本都是只有 安装/卸载 等基础功能，对于小白来说还是不够简单方便，要修改账号配置还要手动修改文件，所以那些ShadowsocksR脚本只能称得上**一键安装脚本**。既然没有我满意的ShadowsocksR一键管理脚本，那么我就自己造喽，于是特意学了Shell，然后写出来了这个ShadowsocksR**一键管理脚本！**
>> 1.支持 限制 端口限速  
>> 2.支持 限制 端口设备数  
>> 3.支持 显示 当前连接IP  
>> 4.支持 显示 SS/SSR连接+二维码  
>> 5.支持 切换管理 单/多端口  
>> 6.支持 一键安装 BBR  
>> 7.支持 一键安装 锐速  
>> 8.支持 一键安装 LotServer  
>> 9.支持 一键封禁 垃圾邮件(SMAP)/BT/PT  
## 安装步骤
> *简单的来说，如果你什么都不懂，那么你直接一路回车就可以了！*  
>本脚本需要Linux root账户权限才能正常安装运行，所以如果不是 root账号，请先切换为root，如果是 root账号，那么请跳过！
>```
> sudo su
>```
>输入上面代码回车后会提示你输入当前用户的密码，输入并回车后，没有报错就继续下面的步骤安装ShadowsocksR。  
>**v2.0.0 版本以后的脚本，请先卸载旧脚本ShadowsocksR服务端，再重新安装！**
>```
>wget -N --no-check-certificate https://softs.fun/Bash/ssr.sh && chmod +x ssr.sh && bash ssr.sh
>```
>**备用下载地址（上面的链接无法下载，就用这个）：**
>```
>wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh
>```
>下载运行后会提示你输入数字来选择要做什么。  
>输入 1 ，就会开始安装ShadowsocksR服务端，并且会提示你输入Shadowsocks的 **端口/密码/加密方式/ 协议/混淆（混淆和协议是通过输入数字选择的）** 等参数。  
>如果安装过程中报错，请看 [常见问题解决方法](https://doub.ws/ss-jc42/#其他说明)。  
>如果协议是origin，那么混淆也必须是plain !
## 使用说明
>运行脚本，
>>```
>>1. bash ssr.sh
>输入对应的数字来执行相应的命令。
>>1. 请输入一个数字来选择菜单选项  
>>2. 1. 安装ShadowsocksR  
>>3. 2. 更新ShadowsocksR  
>>4. 3. 卸载ShadowsocksR  
>>5. 4. 安装 libsodium(chacha20)  
>>6. ————————————  
>>7. 5. 查看账号信息  
>>8. 6. 显示连接信息  
>>9. 7. 设置用户配置  
>>10. 8. 手动修改配置  
>>11. 9. 切换端口模式  
>>12. ————————————  
>>13. 10. 启动ShadowsocksR  
>>14. 11. 停止ShadowsocksR  
>>15. 12. 重启ShadowsocksR  
>>16. 13. 查看ShadowsocksR日志  
>>17. ————————————  
>>18. 14. 其他功能  
>>19. 15. 升级脚本  
>>20. 当前状态:已安装并已启动
>>21. 当前模式:单端口
>>22. 请输入数字(1-15)：  
>>当你为 **单端口模式**时，使用 7. **设置 用户配置** 是 修改 单端口账号配置。  
>>当你为 **多端口模式时**，使用 7. **设置 用户配置** 是 添加/删除/修改 多端口账号配置。  
## 文件位置
>```
>安装目录：/usr/local/shadowsocksr
>配置文件：/etc/shadowsocksr/user-config.json
>```
## 其他说明

>ShadowsocksR 安装后，自动设置为 系统服务，所以支持使用服务来启动/停止等操作，同时支持开机启动。  
>>1. 启动 ShadowsocksR：/etc/init.d/ssr start
>>2. 停止 ShadowsocksR：/etc/init.d/ssr stop
>>3. 重启 ShadowsocksR：/etc/init.d/ssr restart
>>4. 查看 ShadowsocksR状态：/etc/init.d/ssr status
>
>ShadowsocksR 默认支持UDP转发，服务端无需任何设置。  
>本脚本已经集成了 安装/卸载 锐速(ServerSpeeder)开心版，但是是否支持请查看 [Linux支持内核列表](https://www.91yun.co/wp-content/plugins/91yun-serverspeeder/systemlist.html) 。（锐速不支持OpenVZ）  
>**v2.0.0 以前的旧版本下载地址：**  
点击展开 查看更多
**ShadowsocksR目前支持的协议和混淆：**  
**协议（Protocol）**：origin，auth_sha1_v4，auth_aes128_md5，auth_aes128_sha1，auth_chain_a，auth_chain_b  
**混淆（Obfs）**：plain，http_simple，http_post，random_head，tls1.2_ticket_auth，tls1.2_ticket_fastauth(需06/04日以后的服务端版本)  
>origin 和 plain 是原版，加粗的是推荐使用的。  
>>如果你想要使用 tls1.2_ticket_fastauth 混淆插件，那么服务端选择 tls1.2_ticket_auth，客户端选择 tls1.2_ticket_fastauth 即可。  
>>如果服务端 设置混淆参数为：tls1.2_ticket_auth_compatible (兼容原版)  
>>那么客户端 可使用的混淆为：plain / tls1.2_ticket_auth / tls1.2_ticket_fastauth
tls1.2_ticket_auth 与 tls1.2_ticket_fastauth 的区别为，后者不会等待服务器回应，所以不会增加延迟。适合于，因为混淆插件增加延迟的原因不得不选择原版混淆 plain，但是又因为QOS等因素而处于延迟与干扰/限速等之间抉择的时候，可以选择 tls1.2_ticket_fastauth 客户端混淆插件！

### **使用阿里云/腾讯云等存着安全组或规则组一类外部防火墙的请注意**
点击展开 查看更多
### **ShadowsocksR 端口限速中 单线程限速 和 端口总限速 的区别**
>注意：如果要使用脚本中的这个功能，需要重新下载脚本，并重装安装 2月15日 以后的ShadowsocksR服务端才行。  
>请查看这个文章：[ShadowsocksR服务端 限制设备连接数 和 限制端口速度 的方法](https://doub.ws/ss-jc47/)
### **解决 可使用原版协议，但无法使用ShadowsocksR协议 的问题**
点击展开 查看更多
### **提示 Media change: please insert the disc labeled‘Debian GNU/Linux 7.0.0 Wheezy — Official amd64 CD 等信息是 apt源 的问题，更换 apt源**
点击展开 查看解决办法
### **ShadowsocksR启动失败，日志提示：Exception: libsodium not found 的错误**
>这是你使用了 chacha20 系列加密方式，但是却没有安装 libsodium支持库，导致ShadowsocksR无法启动，运行脚本选择选项 4 安装 libsodium支持库即可，如果安装失败，请选择其他的加密方式，对速度影响不大。  
### **提示wget: unknown host "softs.fun" 之类的错误**
>这是无法解析我的域名，多半是DNS的问题，请更换DNS为谷歌DNS。  
>点击展开 查看更多  
### **提示 wget: command not found 的错误**
>这是你的系统精简的太干净了，wget都没有安装，所以需要安装wget。  
>点击展开 查看更多
## 升级脚本
>升级脚本只需要重新下载脚本文件就可以了，会自动覆盖原文件。
## 定时重启
>一些人可能需要定时重启ShadowsocksR服务端来保证稳定性等，所以这里用 crontab 定时。  
>点击展开 查看更多
>本脚本只是本人的第一个Shell脚本学习练手作品，在逻辑结构上问题不少，大家遇到什么BUG请积极反馈！
## 更新日志
>**2018年01月02日，版本 v2.0.37**
>>1. 修复 Debian9 系统下，无法使用 显示连接信息 功能的问题。
>
>**2017年11月12日，版本 v2.0.36**
>>1. 优化 显示链接信息功能的 显示内容排版(对齐了一下)。
>
>**2017年11月03日，版本 v2.0.35**
>>1. 修改 SSR服务端安装方式为：ZIP压缩包安装（考虑到SSR服务端不更新了，所以为了降低git依赖安装出错率，就改成zip压缩包了）。  
>>2. 修改 JQ安装方式为：集成与SSR服务端文件夹内（减少了一个安装JQ的下载步骤，节省时间）。
>
>**2017年10月06日，版本 v2.0.34**
>>1. 恢复 libsodium以前安装方式。
>
>**2017年09月22日，版本 v2.0.33**
>>1. 修复 因为系统缺少automake，而libsodium安装失败的问题。
>
>点击展开 查看更多更新日志  
**更多的Shadowsocks安装教程/一键脚本请看这里：**[Shadowsocks指导篇](https://doub.ws/ss-jc26/#2.2.2、搭建Shadowsocks服务)  
**转载请超链接注明：**[逗比根据地](https://doub.ws/ss-jc42/) » 『[原创』CentOS/Debian/Ubuntu ShadowsocksR 单/多端口 一键管理脚本](https://doub.ws/ss-jc42/)  
**责任声明**：本站一切资源仅用作交流学习，请勿用作商业或违法行为！如造成任何后果，本站概不负责！