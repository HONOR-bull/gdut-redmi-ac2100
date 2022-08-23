## gdut-redmi-ac2100
### 阅读本资料前请先了解以下注意事项：


#### 本资料暂未完成


##### 1.明确自己的目的，以及可以承受的代价（比如路由器购买的成本以及时间成本）
##### 2.刷机有风险，请提前做好资料备份。刷机可能会失去保修，请慎重考虑。
##### 3.本文只适用于红米AC2100,其他路由器请斟酌套用本资料，若出现变砖与本作者无关
##### 4.本文仅提供一种思路用于交流，请勿商用，敬请遵守学校规则以及法律法规
##### 5.本文不保证时效性，可能随环境变迁或者设备差异发生失效。
##### 6.注意，如果基于本文阅读，存在跳转到其他文章的其中某一步骤，之后可能再次回到本文步骤，请知悉。


#### 准备所需物品：1.红米ac2100及其自带电源  2.带有网口的电脑（轻薄本请购买usb转网口转接线）  3.网线2条，*务必两条*   4.牙签或者取卡器或者回形针    5.*（此项非必须，但是十分建议）*  科学上网的工具   6.下载软件[WinSCP](./software/WinSCP-5.13.7-Setup.exe)    7.2022年，WSL在本地的路径与2020年的不一致，不需要everything这个搜索软件


## 本教程非原创，在以下开发者的基础上进行改进:

[在Dr.COM下使用路由器上校园网WIFI(以广东工业大学、极路由1S HC5661A、OpenWrt为例)](https://github.com/shengqiangzhang/Drcom-GDUT-HC5661A-OpenWrt)

本文可能存在不普适各个路由器的情况，因本文尽量使用简单快捷的方法，且目标路由器为红米AC2100





## 步骤一:对路由器刷入breed

在此之前，先略微解释什么是breed，相信接触这个方面的朋友们可能接触过手机刷机，breed与手机刷机的recovery有些相似，都是在底层刷入系统的工具。如果没有接触过recovery的朋友也不必理会，只需要知道装breed的目的是为了下一步刷入固件（ROM）。

方法一：在此文章链接的步骤二[在Dr.COM下使用路由器上校园网WIFI(以广东工业大学、极路由1S HC5661A、OpenWrt为例)](https://github.com/shengqiangzhang/Drcom-GDUT-HC5661A-OpenWrt#%E6%AD%A5%E9%AA%A4%E4%BA%8C%E5%88%B7%E5%85%A5%E4%B8%8D%E6%AD%BBbreed)

此方法较为原始，可以称之为大道



方法二：此方法来自[恩山论坛文章红米【AC2100】一键刷BREED【30秒刷完】|无需Telnet](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=4066963)

通过个人简化说明以及步骤，属于新道路，但是同样可以刷入breed，如果介意请走方法一。

1.无论拿到手的ac2100是什么版本的固件，我都建议你将其进行降级操作，可以在[恩山文章链接](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=4066963)中自取文件

（请注意选择红米rm ac2100，而非xiaomi ac2100），或者在我的[仓库中下载](https://github.com/HONOR-bull/gdut-redmi-ac2100/tree/main/%E9%99%8D%E7%BA%A7)


2.下载完成后，电脑网线插入路由器的LAN口，然后打开浏览器进入后台地址：192.168.31.1->右上角->系统升级->手动升级，加载刚刚下载的降级固件，可以保留数据->开始升级

3.升级（降级）操作完成后会自动重启，再次进入后台192.168.31.1，此时需要用到第二条网线，第二条网线连接墙上网口与路由器WAN口，然后在路由器设置中进行拨号上网（请自己交网费和输入校园网账号密码），此时电脑网线仍然连接路由器LAN口，并且是能上网的状态。

注：简而言之，保证路由器与电脑有网。

4.*本步骤掠过恩山文章的坏块检查*    

桌面新建一个记事本（txt），根据恩山文章所说，复制浏览器地址栏的stok字符串，粘贴到记事本中。替换以下链接中CCCCC部分，CCCCC是刚刚复制的stok字符串

```http://192.168.31.1/cgi-bin/luci/;stok=CCCCCCCCCCC/api/misystem/set_config_iotdev?bssid=Xiaomi&user_id=longdike&ssid=%0Acd%20%2Ftmp%0Acurl%20-o%20B%20-O%20https%3A%2F%2Fbreed.hackpascal.net%2Fr1286%2520%255b2020-10-09%255d%2Fbreed-mt7621-xiaomi-r3g.bin%20-k%20-g%0A%5B%20-z%20%22%24(sha256sum%20B%20%7C%20grep%20242d42eb5f5aaa67ddc9c1baf1acdf58d289e3f792adfdd77b589b9dc71eff85)%22%20%5D%20%7C%7C%20mtd%20-r%20write%20B%20Bootloader%0A```

此代码就是用来刷BREED的，*将替换好stok*的这一大串链接从记事本复制到路由器地址栏，按回车，稍等一会，会出现:  {"code":0} 

>如果路由器在60秒内重启则代表刷BREED成功(灯会从蓝变橘，最终变蓝进入系统)。   成功转下一步，不成功 请查阅疑难解答

<br />




## 步骤二:进入breed

成功后拔掉电源，拿事先准备好的牙签或者卡针，按住reset（电源插孔隔壁的小洞洞），之后接上电源等2秒，路由器灯会蓝橘色闪烁，停止闪烁后松开reset，电脑与**路由器的LAN口**连接，电脑浏览器地址栏输入192.168.1.1，即可进入breed

>如果没重启，可能是stok过期了。进入后台复制新的stok即可。也有可能下载的BREED损坏，从新运行代码。也有可能没网络。

>如果刷完后可能无法进入原厂系统，进BREED删变量：normal_firmware_md5

如果不想自行编译固件可以跳到[步骤五：刷入固件](https://github.com/HONOR-bull/gdut-redmi-ac2100#%E6%AD%A5%E9%AA%A4%E4%BA%94%E5%88%B7%E5%85%A5openwrt)

***固件可以自行到恩山论坛获取，或者使用官方的英文原版固件。***

刷入固件后请跳转到[步骤六：配置privoxy](https://github.com/HONOR-bull/gdut-redmi-ac2100/edit/main/README.md#%E6%AD%A5%E9%AA%A4%E5%85%AD%E8%BF%9B%E8%A1%8Cprivoxy%E7%9A%84%E4%BB%A3%E7%90%86)

<br />


## 步骤三:编译openwrt系统的铺垫
有了breed，我们就可以建房子了，就是编译**属于自己的固件**，偷懒的话其实也可以不编译(乐)，空降[步骤五](https://github.com/HONOR-bull/gdut-redmi-ac2100#%E6%AD%A5%E9%AA%A4%E4%BA%94%E5%88%B7%E5%85%A5openwrt)

1.打开Microsoft Store（windows自带应用商店），搜索Ubuntu，选择Ubuntu20.04.4 LTS进行安装。

2.屏幕左下角windows图标右键，或者**win+x**，选择Windows powershell（**管理员**），输入`wsl --install`，然后回车      
相关代码以及解释来源于使用 [WSL 在 Windows 上安装 Linux](https://docs.microsoft.com/zh-cn/windows/wsl/install)

3.打开安装好的ubuntu，会提示Installing, this may take a few minutes...     如果有问题移步疑难解答
过大概几秒钟，会要求你输入一个用户名和密码，尽量简单容易记住就可以。密码要输入两次确保正确，输入密码时**不可见**，请尽量短且容易记（如1111），创建好后ubuntu窗口不要关闭。

4.此时还不需要打开科学上网，除非你上GitHub打开了，此时请关闭。之后参考[Lean的Openwrt 源码仓库](https://github.com/coolsnowwolf/lede)中的代码，**这里说一下注意事项**

准备工作我们已经完成，[从这一步开始](https://github.com/coolsnowwolf/lede#%E7%BC%96%E8%AF%91%E5%91%BD%E4%BB%A4)

[链接](https://github.com/coolsnowwolf/lede#%E7%BC%96%E8%AF%91%E5%91%BD%E4%BB%A4)中有一个代码框，其中包含三条代码，分开使用，首先复制`sudo apt update -y`到刚刚装的Ubuntu，**如果关闭了请再开**，右键即可粘贴代码，回车运行，此时会要求你输入设好的密码，输入密码后回车，他会自动执行安装命令。之后是这条命令：`sudo apt full-upgrade -y`，与刚刚类似操作，只是不需要再输密码，第三条是下面这块，**完成后第三条请多执行一次**
```
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pip libpython3-dev qemu-utils \
rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
```

>下载源代码，更新 feeds 并选择配置
这里下面的代码也是逐一执行，分开来容易知道错误在哪里。
```
git clone https://github.com/coolsnowwolf/lede

cd lede

./scripts/feeds update -a

./scripts/feeds install -a
```
在执行`make menuconfig`命令前，请再次执行`./scripts/feeds update -a`和`./scripts/feeds install -a`，确保提示already up to date，确保完全。

5.`make menuconfig`，等会，界面会发生改变，可以用***上下键进行移动，回车键进行选择，左右键切换select和exit进行选择和离开***，

6.选架构如图所示的**mediatek MIPS**，  CPU型号**MT7921**，点多次下方向键，选LucI----applications----寻找luci-app-privoxy，按两次空格，使左边为 * 号。到此结束，右方向键选exit回到原始界面

![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/%E6%9E%B6%E6%9E%84.png)

![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/privoxy.png)

![pic](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/pic/save.png)
7.然后右方向键选save，改名为（如ac.config），save。改好名字后选exit，Ubuntu窗口可以关闭

8.刚刚的config文件本地路径在`\\wsl$\Ubuntu-20.04\home\用户名\lede`中（至少win10 21H1在这里）

## 步骤四：进行openwrt的云编译
来源：[使用 GitHub Actions 云编译 OpenWrt](https://p3terx.com/archives/build-openwrt-with-github-actions.html)

>准备工作
>GitHub 账号
>搭建编译环境，生成.config文件。(步骤三已完成)

1.进入[P3TERX/Actions-OpenWrt](https://github.com/P3TERX/Actions-OpenWrt) 项目页面，点击页面中的Use this template（使用这个模版）按钮。

2.>填写仓库名称，然后点击Create repository from template（从模版创建储存库）按钮。
！[pic](https://imgcdn.p3terx.com/post/20200201183634.png#vwid=814&vhei=533)

3.>经过几秒钟的等待，页面会跳转到新建的仓库。然后点击Add file-----再点create new file按钮。
！[pic](https://imgcdn.p3terx.com/post/20200201183637.png#vwid=1130&vhei=551)

4.>文件名填写为.config，把生成的.config 文件的内容复制粘贴到下面的文本框中
就是步骤三中获得的config文件，记事本打开该文件，全选复制粘贴到文本框中
！[pic](https://imgcdn.p3terx.com/post/20200201183635.png#vwid=1114&vhei=456)

5.>翻到页面最下方，点击Commit new file（提交新文件）按钮。
！[pic](https://imgcdn.p3terx.com/post/20200201185116.png#vwid=1125&vhei=405)

6.在 Actions 页面选择Build OpenWrt，SSH connection to Actions的值要为false，然后点击Run Workflow按钮，即可开始编译。
！[pic](https://imgcdn.p3terx.com/post/20201011212837.png#vwid=1068&vhei=586)

7.等待一两个小时后完成，同样在action这里取回压缩包到本地


## 步骤五：刷入openwrt
1.在有网的状态下先下载底包，可以是我仓库里的[底包](https://github.com/HONOR-bull/gdut-redmi-ac2100/blob/main/%E5%BA%95%E5%8C%85mt7621redmiac2100.bin)，也可是openwrt官方的

2.然后进入breed，详见[步骤二](https://github.com/HONOR-bull/gdut-redmi-ac2100#%E6%AD%A5%E9%AA%A4%E4%BA%8C%E8%BF%9B%E5%85%A5breed)
左边**环境变量**编缉菜单，新增字段xiaomi.r3g.bootfw值为2，然后点保存

3.然后选择左方**固件更新**，在固件那一行右方选择浏览，选择刚刚下载的底包，选好后点击上传，自动重启已经自动勾选

4.进度条走完多等一会，知道路由器蓝灯常亮或者闪蓝灯，新开一个网页，地址栏输入192.168.1.1（本教程是该地址，**其他固件**地址或有改变，请留意）

5.此时进入后台管理界面，账号是root，密码是password（本教程是该账号密码，**其他固件**或有改变，请留意）

6.登录进去后上方选择system（系统）----backup/flash firmware（备份/升级固件），选择固件（步骤四或其他），keep setting（保留配置）不要勾选，点击flash image（刷写固件）。再点，Proceed
！[pic](https://img-blog.csdnimg.cn/img_convert/9c12df55e8321350fb8b42e609a81dcb.png)
！[pic](https://img-blog.csdnimg.cn/img_convert/2488880bceea98f3c5a3a84b73c7ab75.png)

7.转圈圈等待1-2分钟左右关闭网页，重新进入192.168.1.1，此时语言已经为中文，此时的固件就是自己编辑的固件/别人的固件了     疑难解答


## 步骤六：进行privoxy的代理
[来源](https://blog.csdn.net/qq_33825817/article/details/87755836)
#### 来源文章顺序不太合理，请按照本文操作，[来源文章](https://blog.csdn.net/qq_33825817/article/details/87755836)翻到步骤六

1.无论是自己编译固件还是使用他人固件都**跳过同步时间这一步**

2-1.如果不是按照本文编译的固件，或者别人编译的固件，请按照来源文章 安装好 Privoxy，注意：**其他来源的固件需要先联网再安装privoxy**，方法：来源文章的步骤五

>安装 Privoxy。进入路由器管理页面，点击 System(系统) -> Software（软件包）。
>点击 Update lists（刷新列表）按钮，等待几分钟。如果提示好几条“Signature check passed”那么这一步执行成功；如果卡死了，几分钟后再进入这个页面，看到了很长很长的软件列表，那也是成功了。
>在 Filter（过滤器）中填写luci-app-privoxy，点击 Find package（查找软件包）按钮。点击下方“luci-app-privoxy”对应的 Install（安装）按钮。如果提示好几条“Configuring xxxx”，那么就是执行成功了；如果卡死后再进入管理页面，看到有一个 Services（服务）菜单，菜单里有 Privoxy WEB proxy（Privoxy 网络代理），那也是成功了。


2-2.按本文编译的固件按来源文章配置 Privoxy 设置。
>点击 Services -> Privoxy WEB proxy。
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

4.此处不用来源文章步骤，来源文章该步骤存在不确定性问题，打开winscp，左上角创建新连接，协议选ssh，22端口地址是192.168.1.1，输入**路由器后台的账号密码**，点击连接。

5.右方是路由器的文件目录，最上方的...返回上一级，进入etc文件夹--bin文件夹---privoxy文件夹，把本地的match-all.action复制到路由器的目录，进行**替换**     **此处忘记了，回学校补**

## 步骤七：配置drcom拨号上网
[来源文章](https://blog.csdn.net/qq_33825817/article/details/87755836)
1.从步骤六返回到最底层目录，进入tmp文件夹，把2100.ipk复制到tmp文件夹中，然后点击上方命令行，输入`opkg install 2100.ipk`,显示xxxxxx  configure即为成功，拔掉路由器电源再插上，此时是重启路由器

2.路由器重启完成后再次进入192.168.1.1，上方服务按钮，点击drcom，进行配置。方法来源于[文章](https://blog.csdn.net/qq_33825817/article/details/87755836)步骤五的修改Dr.com客户端的配置，但是来源文章存在多余步骤，请按第3点。

3.drcom认证和认证拨号打✔，下面接口选`eth0.2`，输入自己的**校园网账号和密码**，MAC地址不要填写，选对应的校区，drcom版本保持不变，然后**点保存&应用**，等待十来秒转圈。

4.点击上方网络----接口，查看wan口有没有数据包，有证明已经完成上网的基本配置与drcom的配置。

## 步骤八：检验成果
验证防检测效果，电脑打开[ua分析](http://www.all-tool.cn/Tools/ua/)，显示为privoxy3.0.28即为成功

## 步骤九：定时重启与wifi的设置    未完成
1.路由器后台，点击上方系统-----定时重启，选择每天，时间选择5：00即可，**点保存&应用**

2.点击上方网络-----无线

3.当前页面存在两个无线频段，点击基本设置，可以wifi名称，或者是`wifi-2.4`，`wifi-5g`这样设置，**也可以自己设置名字**

4.回到无线页面，高级设置，TX power的值设置为30，两个频段都要这样设置，**否则影响路由器性能以及自动重启。**

### 大功告成！开始多设备畅游广工之旅吧
