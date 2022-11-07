## gdut-redmi-ac2100
### 阅读本资料前请先了解以下注意事项：


##### 1.明确自己的目的，以及可以承受的代价（比如路由器购买的成本以及时间成本）
##### 2.刷机有风险，请提前做好资料备份。刷机可能会失去保修，请慎重考虑。
##### 3.本文只适用于红米AC2100,其他路由器请斟酌套用本资料，若出现变砖与本作者无关
##### 4.本文仅提供一种思路用于交流，请勿商用，敬请遵守学校规则以及法律法规
##### 5.本文不保证时效性，可能随环境变迁或者设备差异发生失效。
##### 6.注意，如果基于本文阅读，存在跳转到其他文章的其中某一步骤，之后可能再次回到本文步骤，请知悉。


###### 准备所需物品：
1.红米ac2100及其自带电源  
2.带有网口的电脑（轻薄本请购买usb转网口转接线）  
3.网线2条，*务必两条*   
4.牙签或者取卡器或者回形针   
5.*（此项非必须，但是十分建议）*  科学上网的工具   
6.下载软件[WinSCP](https://winscp.net/eng/download.php)    
7.2022年，WSL2（基于windows系统的linux子系统）在本地的路径与2020年的不一致，不需要everything这个搜索软件


## 本教程非原创，在以下开发者的基础上进行改进:

[在Dr.COM下使用路由器上校园网WIFI(以广东工业大学、极路由1S HC5661A、OpenWrt为例)](https://github.com/shengqiangzhang/Drcom-GDUT-HC5661A-OpenWrt)

本文可能存在不普适各个路由器的情况，因本文尽量使用简单快捷的方法，且目标路由器为红米AC2100





## 步骤一:对路由器刷入breed

breed与手机刷机的recovery有些相似，都是在底层刷入系统的工具。如果没有接触过recovery的朋友也不必理会，只需要知道装breed的目的是为了下一步刷入固件（ROM）。

###### 方法一：在此文章链接的步骤二[老古董玩意儿](https://github.com/shengqiangzhang/Drcom-GDUT-HC5661A-OpenWrt#%E6%AD%A5%E9%AA%A4%E4%BA%8C%E5%88%B7%E5%85%A5%E4%B8%8D%E6%AD%BBbreed)


### 方法一较为原始，能不用尽量不用



方法二：此方法来自[恩山论坛文章一键刷BREED](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=4066963)

同样可以刷入breed，简单又快捷，过于有空的请走方法一。

1.首先进行降级操作，可以在[恩山文章链接](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=4066963)中自取文件

（请注意选择红米rm ac2100，而非xiaomi ac2100），或者在我的[仓库中下载](https://github.com/HONOR-bull/gdut-redmi-ac2100/tree/main/%E9%99%8D%E7%BA%A7)


2.下载完成后，电脑网线连接路由器的LAN口，然后打开浏览器进入后台地址：192.168.31.1->右上角->系统升级->手动升级，加载刚刚下载的降级固件，可以保留数据->开始升级

3.升级（降级）操作完成后会自动重启，再次进入后台192.168.31.1，此时需要用到第二条网线，第二条网线连接墙上网口与路由器WAN口，然后在路由器设置中进行拨号上网（请自己交网费和输入校园网账号密码），此时电脑网线仍然连接路由器LAN口，并且是能上网的状态。

注：简而言之，保证路由器与电脑有网。

4.*掠过恩山文章的坏块检查*    

桌面新建一个记事本（txt），复制浏览器地址栏的stok字符串，粘贴到记事本中。替换以下链接中CCCCC部分，CCCCC是刚刚复制的stok字符串

![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/stock.png)

```http://192.168.31.1/cgi-bin/luci/;stok=CCCCCCCCCCC/api/misystem/set_config_iotdev?bssid=Xiaomi&user_id=longdike&ssid=%0Acd%20%2Ftmp%0Acurl%20-o%20B%20-O%20https%3A%2F%2Fbreed.hackpascal.net%2Fr1286%2520%255b2020-10-09%255d%2Fbreed-mt7621-xiaomi-r3g.bin%20-k%20-g%0A%5B%20-z%20%22%24(sha256sum%20B%20%7C%20grep%20242d42eb5f5aaa67ddc9c1baf1acdf58d289e3f792adfdd77b589b9dc71eff85)%22%20%5D%20%7C%7C%20mtd%20-r%20write%20B%20Bootloader%0A```

代码是用来刷BREED的，*将替换好stok*的这一大串链接从记事本复制到路由器地址栏，按回车，稍等一会，会出现:  {"code":0} 

>如果路由器在60秒内重启则代表刷BREED成功(灯会从蓝变橘，最终变蓝进入系统)。 

<br />




## 步骤二:进入breed

刷入breed成功后拔掉电源，电脑与**路由器的LAN口**连接，拿事先准备好的牙签或者卡针，先按住reset（电源插孔隔壁的小洞洞），之后接上电源等2秒，路由器灯会红色闪烁，停止闪烁后松开reset，电脑浏览器地址栏输入192.168.1.1/index.html，即可进入breed

>如果没重启，可能是stok过期了。进入后台复制新的stok即可。也有可能下载的BREED损坏，重新运行代码。也有可能没网络。

>如果刷完后可能无法进入原厂系统，进BREED删变量：normal_firmware_md5

如果不想自行编译固件可以跳到[步骤五：刷入固件](https://github.com/HONOR-bull/gdut-redmi-ac2100#%E6%AD%A5%E9%AA%A4%E4%BA%94%E5%88%B7%E5%85%A5openwrt)


<br />


## 步骤三:编译openwrt系统的铺垫
有了breed，我们就可以建房子了，就是编译**属于自己的固件**，偷懒的话其实也可以不编译(乐)，空降[步骤五](https://github.com/HONOR-bull/gdut-redmi-ac2100#%E6%AD%A5%E9%AA%A4%E4%BA%94%E5%88%B7%E5%85%A5openwrt)

1.打开windows自带应用商店，搜索Ubuntu，选择Ubuntu20.04 LTS进行安装。

2.屏幕左下角windows图标右键，或者**win+x**，选择Windows powershell（**管理员**），输入`wsl --install`，然后回车      
相关代码以及解释来源于使用 [WSL 在 Windows 上安装 Linux](https://docs.microsoft.com/zh-cn/windows/wsl/install)

3.打开应用商店安装的ubuntu，会提示Installing, this may take a few minutes...     
过大概几秒钟，会要求你输入一个用户名和密码，尽量简单容易记住就可以。密码要输入两次确保正确，输入密码时**不可见**，请尽量短且容易记（如111），创建好后ubuntu窗口不要关闭。

4.此时打开科学上网，要对linux进行代理，clash代理[详细方法](https://www.jianshu.com/p/af6a02f98d5a)

或者clash打开允许局域网连接，之后win+r调出运行,输入`\\wsl$`，点ubuntu文件夹，如果没文件请先打开ubuntu再刷新，进入home文件夹，再进刚刚设置的用户名找到一个`.bashrc`文件，文末添加

```
export hostip=$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*')
export https_proxy="http://${hostip}:7890";
export http_proxy="http://${hostip}:7890";

curl google.com
```
即可实现每次开启自动代理

5.分别执行以下命令进项更换镜像源
```
sudo sed -i "s@http://.*archive.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
```

6.[Lean的Openwrt 源码仓库](https://github.com/coolsnowwolf/lede)中的代码，其中包含三条代码，分开使用，右键即可粘贴代码，回车运行，**第三条大块命令请多执行一次**
```
sudo apt update -y

sudo apt full-upgrade -y

```

>下载源代码，更新 feeds 并选择配置
这里下面的代码也是逐一执行,
```
git clone https://github.com/coolsnowwolf/lede

cd lede

./scripts/feeds update -a

./scripts/feeds install -a

cd packge

git clone https://github.com/GJXS1980/ODP.git

git clone https://github.com/CHN-beta/xmurp-ua.git package/xmurp-ua

cd ..
```

以下命令为个性化固件部分
```
sed -i 's/OpenWrt/Windy/g' package/kernel/mac80211/files/lib/wifi/mac80211.sh

sed -i 's/wireless.default_radio${devidx}.encryption=none/wireless.default_radio${devidx}.encryption=psk-mixed/g' package/kernel/mac80211/files/lib/wifi/mac80211.sh

sed -i '/encryption/a\set wireless.default_radio${devidx}.key=123456789' package/kernel/mac80211/files/lib/wifi/mac80211.sh
```

请再次执行`./scripts/feeds update -a`和`./scripts/feeds install -a`，确保提示already up to date，确保完全。

5.`make menuconfig`，界面会发生改变，可以用***上下键进行移动，回车键进行选择，空格进行勾选，左右键切换select和exit进行选择和离开***，

6.选架构如图所示的**mediatek MIPS**，  CPU型号**MT7921**，点多次下方向键，选LucI----applications----寻找luci-app-privoxy，按两次空格，使左边为 * 号，返回开始的页面下寻找Network，寻找gdut-drcom，按两次空格，使左边为 * 号。到此结束，右方向键选exit回到原始界面

![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/%E6%9E%B6%E6%9E%84.png)

![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/privoxy.png)

7.另外，如果使用xmurp-ua插件，在kernel————other module中选择xmurp-ua，*号，主页base system中勾选zram-swap，*号。（privoxy和xmurp-ua二选一即可）

8.然后右方向键选save。后选exit

9.刚刚的config文件本地路径在`\\wsl$\Ubuntu-20.04\home\用户名\lede`中（至少win10 21H1在这里）

## 步骤四：进行编译
云编译：来源：[使用 GitHub Actions 云编译 OpenWrt](https://p3terx.com/archives/build-openwrt-with-github-actions.html)

>准备工作
>注册GitHub 账号

>搭建编译环境，生成.config文件。(步骤三已完成)

1.进入[P3TERX/Actions-OpenWrt](https://github.com/P3TERX/Actions-OpenWrt) 项目页面，点击页面中的Use this template（使用这个模版）按钮。

2.
>填写仓库名称，然后点击Create repository from template（从模版创建储存库）按钮。
![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/553.png)

3.
>经过几秒钟的等待，页面会跳转到新建的仓库。然后点击Add file-----再点create new file按钮。
![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/551.png)

4.
>文件名填写为.config，把生成的.config 文件的内容复制粘贴到下面的文本框中
就是步骤三中获得的config文件，记事本打开该文件，全选复制粘贴到文本框中
![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/456.png)

5.
>翻到页面最下方，点击Commit new file（提交新文件）按钮。
![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/405.png)

6.在 Actions 页面选择Build OpenWrt，SSH connection to Actions的值要为false，然后点击Run Workflow按钮，即可开始编译。
![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/586.png)

7.等待一两个小时后完成，同样在action这里取回压缩包到本地


本地编译，根据lede仓库步骤进行
```
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

make download -j8

make V=s -j1
```

## 步骤五：刷入openwrt
1.在有网的状态下先下载底包，可以是我仓库里的[底包](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/%E5%BA%95%E5%8C%85mt7621redmiac2100.bin)，也可是openwrt官方的

2.然后进入breed，详见[步骤二](https://github.com/HONOR-bull/gdut-redmi-ac2100#%E6%AD%A5%E9%AA%A4%E4%BA%8C%E8%BF%9B%E5%85%A5breed)
左边**环境变量**编缉菜单，新增字段xiaomi.r3g.bootfw值为2，然后点保存

3.然后选择左方**固件更新**，在固件那一行右方选择浏览，选择刚刚下载的底包，选好后点击上传，自动重启已经自动勾选

4.进度条走完多等一会，知道路由器蓝灯常亮或者闪蓝灯，新开一个网页，地址栏输入192.168.1.1

5.进入后台管理界面，账号是root，密码是password

6.登录进去后上方选择system（系统）----backup/flash firmware（备份/升级固件），最下方的choose file选择固件，keep setting（保留配置）不要勾选，点击flash image（刷写固件）。再点，Proceed
![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/flash%20image.png)
![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/verify.png)

7.转圈圈等待1-2分钟左右关闭网页，重新进入192.168.1.1，此时语言已经为中文，此时的固件就是自己编辑的固件/别人的固件了     


## 步骤六：进行privoxy的代理
[来源](https://blog.csdn.net/qq_33825817/article/details/87755836)
#### 来源文章顺序不太合理，请按照本文操作，[来源文章](https://blog.csdn.net/qq_33825817/article/details/87755836)翻到步骤六

1.**跳过同步时间这一步**

2-1.如果不是按照本文编译的固件，或者别人编译的固件，请按照来源文章 安装好 Privoxy，注意：**其他来源的固件需要先联网再安装privoxy**，方法：来源文章的步骤五

2-2.按本文编译的固件按来源文章配置 Privoxy 设置。
>点击 Services（服务） -> Privoxy WEB proxy。
>Files and Directories（文件和目录）：Action Files **删除到只剩match-all.action**。`Filter files` 和 `Trust files` 均留空。
>Access Control（访问控制）：Listen addresses 填写`0.0.0.0:8118`，Permit access 填写`192.168.0.0/16`。Enable action file editor 勾选。
>Miscellaneous（杂项）：Accept intercepted requests 勾选。
>Logging（日志）：全部取消勾选。

**点击 Save & Apply。**

3.点击上方网络------防火墙------自定义规则
>在大框框里另起一行，添加下面的代码：
```
iptables -t nat -N http_ua_drop
iptables -t nat -I PREROUTING -p tcp --dport 80 -j http_ua_drop
iptables -t nat -A http_ua_drop -m mark --mark 1/1 -j RETURN
iptables -t nat -A http_ua_drop -d 0.0.0.0/8 -j RETURN
iptables -t nat -A http_ua_drop -d 127.0.0.0/8 -j RETURN
iptables -t nat -A http_ua_drop -d 192.168.0.0/16 -j RETURN
iptables -t nat -A http_ua_drop -p tcp -j REDIRECT --to-port 8118
```

>点击 Restart Firewall（重启防火墙）按钮。

4.打开winscp，创建新连接，协议选ssh，22端口地址是192.168.1.1，输入**路由器的账号密码**，点击连接。

5.右方是路由器的文件目录，最上方的...返回上一级，进入etc文件夹--bin文件夹---privoxy文件夹，把本地的match-all.action拖入到路由器的目录，进行**替换**     

## 步骤七：配置drcom拨号上网
本教程drcom插件已经编译进固件，在服务选项卡中点击即可

3.drcom认证和认证拨号打✔，下面接口选`eth0.2（或者eth1）`，输入自己的**校园网账号和密码**，MAC地址不填，选对应的校区，drcom版本保持不变，然后**点保存&应用**，等待十来秒转圈。

4.点击上方网络----接口，查看wan口有没有数据包，有证明已经完成上网的基本配置与drcom的配置。

## 步骤八：检验成果
验证防检测效果，电脑打开[ua分析](http://www.all-tool.cn/Tools/ua/)，显示为privoxy3.0.28即为成功，xmurp-ua不要开启软件分流加速。

## 步骤九：定时重启与wifi的设置    
1.路由器后台，点击上方系统-----定时重启，选择每天，时间选择5：00即可，**点保存&应用**

2.点击上方网络-----无线

3.当前页面存在两个无线频段，点击基本设置，可以wifi名称，或者是`wifi-2.4`，`wifi-5g`这样设置，**也可以自己设置名字**

4.回到无线页面，高级设置，TX power的值设置为16，两个频段都要这样设置，**否则影响路由器性能以及自动重启。**

### 大功告成！开始多设备畅游广工之旅吧
