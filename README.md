## gdut-redmi-ac2100
### 阅读本资料前请先了解以下注意事项：
#### 本资料暂未完成
##### 1.明确自己的目的，以及可以承受的代价（比如路由器购买的成本以及时间成本）
##### 2.刷机有风险，请提前做好资料备份。刷机可能会失去保修，请慎重考虑。
##### 3.本文只适用于红米AC2100,其他路由器请斟酌套用本资料，若出现变砖与本作者无关
##### 4.本文仅提供一种思路用于交流，请勿商用，敬请遵守学校规则以及法律法规
##### 5.本文不保证时效性，可能随环境变迁或者设备差异发生失效。
##### 6.注意，如果基于本文阅读，存在跳转到其他文章的其中某一步骤，之后可能再次回到本文步骤，请知悉。

### 准备所需物品：1.红米ac2100及其自带电源  2.带有网口的电脑（轻薄本请购买usb转网口转接线）  3.网线2条，务必两条   4.牙签或者取卡器或者回形针    5.*（此项非必须，但是十分建议）*  科学上网的工具   6.下载软件[WinSCP](./software/WinSCP-5.13.7-Setup.exe)

## 本教程非原创，在以下开发者的基础上进行改进:[在Dr.COM下使用路由器上校园网WIFI(以广东工业大学、极路由1S HC5661A、OpenWrt为例)](https://github.com/shengqiangzhang/Drcom-GDUT-HC5661A-OpenWrt)    本文可能存在不普适的情况，因其尽量使用简单快捷的方法

## 步骤一:对路由器刷入breed

<br />

在此之前，先略微解释什么是breed，相信接触这个方面的朋友们可能接触过手机刷机，breed与手机刷机的recovery有些相似，都是在底层刷入系统的工具。如果没有接触过recovery的朋友也不必理会，只需要知道装breed的目的是为了下一步刷入固件（ROM）。

方法一：在此文章链接的步骤二[在Dr.COM下使用路由器上校园网WIFI(以广东工业大学、极路由1S HC5661A、OpenWrt为例)](https://github.com/shengqiangzhang/Drcom-GDUT-HC5661A-OpenWrt)
此方法较为原始，可以称之为正道

方法二：此方法来自[恩山论坛文章红米【AC2100】一键刷BREED【30秒刷完】|无需Telnet](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=4066963)，通过个人简化说明以及步骤，属于歪门邪道，但是同样可以刷入breed，如果介意请走正道。

1.无论拿到手的ac2100是什么版本的固件，我都建议你将其进行降级操作，可以在本方法的文章链接中自取文件（请注意选择红米rm ac2100，而非xiaomi ac2100，寄了别怪我没提醒），或者在我的仓库中下载

2.下载完成后，电脑网线插入路由器的LAN口，然后打开浏览器进入后台，192.168.31.1->常用设置->系统状态->手动升级，加载刚刚下载的降级固件，可以保留数据->开始升级

3.升级（降级）操作完成后会自动重启，再次进入后台192.168.31.1，此时需要用到第二条网线，第二条网线连接墙上网口与路由器WAN口，然后在路由器设置中进行拨号上网（请自己交网费和输入校园网账号密码），此时电脑网线仍然连接路由器LAN口，并且是能上网的状态。

3注：简而言之，保证路由器与电脑有网。

4.本步骤存在图片，请跳转上方文章仔细阅读。      *本步骤取消坏块检查*    桌面新建一个记事本（txt），根据恩山文章所说，替换以下链接中CCCCC部分，CCCCC为自己路由器的stok字符串

`http://192.168.31.1/cgi-bin/luci/;stok=CCCCCCCCCCC/api/misystem/set_config_iotdev?bssid=Xiaomi&user_id=longdike&ssid=%0Acd%20%2Ftmp%0Acurl%20-o%20B%20-O%20https%3A%2F%2Fbreed.hackpascal.net%2Fr1286%2520%255b2020-10-09%255d%2Fbreed-mt7621-xiaomi-r3g.bin%20-k%20-g%0A%5B%20-z%20%22%24(sha256sum%20B%20%7C%20grep%20242d42eb5f5aaa67ddc9c1baf1acdf58d289e3f792adfdd77b589b9dc71eff85)%22%20%5D%20%7C%7C%20mtd%20-r%20write%20B%20Bootloader%0A`

此代码就是用来刷BREED的，将替换好的这一大串链接从记事本复制到路由器地址栏，按回车，稍等一会，会出现:  {"code":0} 

>如果路由器在60秒内重启则代表刷BREED成功(灯会从蓝变橘，最终变蓝进入系统)。   成功转下一步，不成功 请查阅疑难解答

<br />

## 步骤二:进入breed
成功后拔掉电源，拿事先准备好的牙签或者卡针，按住reset（电源插孔隔壁的小洞洞），之后接上电源等2秒，路由器灯会蓝橘色闪烁，停止闪烁后松开reset，即可进入breed，电脑浏览器地址栏输入192.168.1.1

>如果没重启，可能是stok过期了。进入后台复制新的stok即可。也有可能下载的BREED损坏，从新运行代码。也有可能没网络。

>如果刷完后可能无法进入原厂系统，进BREED删变量：normal_firmware_md5



<br />


## 步骤三:编译openwrt系统的铺垫
有了breed这个基础，我们就可以建房子了，就是编译属于自己的固件，偷懒的话其实也可以不编译(乐)

1.打开Microsoft Store（windows自带应用商店），搜索Ubuntu，选择Ubuntu20.04.4 LTS进行安装。

2.屏幕左下角windows图标右键，选择Windows powershell（管理员），输入`wsl --install`，然后回车      相关代码以及解释来源于使用 [WSL 在 Windows 上安装 Linux](https://docs.microsoft.com/zh-cn/windows/wsl/install)

3.打开安装好的ubuntu，会提示Installing, this may take a few minutes...     如果有问题移步疑难解答
过大概几秒钟，会要求你输入一个用户名和密码，尽量简单容易记住就可以，用户名和密码是本地账户，没什么安全隐患。密码要输入两次确保正确，输入密码时不可见，请尽量短且容易记（如1111），创建好后不要关闭。

4.此时还不需要打开科学上网，除非你上GitHub打开了，此时请关闭。之后参考[Lean 的 Openwrt 源码仓库](https://github.com/coolsnowwolf/lede)中的代码，以下说一些注意事项

[点击，从这一步开始](https://github.com/coolsnowwolf/lede#%E7%BC%96%E8%AF%91%E5%91%BD%E4%BB%A4)

这一步中的安装依赖，有一个代码框，其中包含三条代码，请分开，每次一条执行，每个sudo apt开头的为一条，就是两条很短的+一条很长的。如何使用呢，复制第一条，然后在WSL中（刚刚装的Ubuntu，此时没有关闭，关闭了请再开），右键即可粘贴代码，回车运行，此时会要求你输入设好的密码，输入密码后回车，他会自动执行安装命令。执行完后继续第二第三条命令。

>下载源代码，更新 feeds 并选择配置
这里下面的代码也是逐一执行，分开来容易知道错误在哪里。

在执行`make menuconfig`命令前，请再次执行更新feeeds的代码，确保提示already up to date，确保下载完全。

5.粘贴`make menuconfig`，等会，界面会发生改变，可以用上下键进行移动，回车键进行选择，左右键切换select和exit进行选择和离开，

6.选架构，选LucI，applications，privoxy

7.选好后save，改名，exit，关闭

8.everything，寻找刚刚改名的config文件，复制到桌面上。

## 步骤四：进行openwrt的云编译
来源：[使用 GitHub Actions 云编译 OpenWrt](https://p3terx.com/archives/build-openwrt-with-github-actions.html)

文件名填写为.config，把生成的.config 文件的内容复制粘贴到下面的文本框中，刚刚的config

在 Actions 页面选择Build OpenWrt，然后点击Run Workflow按钮，即可开始编译。SSH connection to Actions的值要为false。

一两个小时后完成，取回本地


## 步骤五：刷入openwrt

先刷底包，然后进后台，升级，新包

进新的固件，修改密码admin

winscp  ssh 22端口 账号密码  之后返回上一级 /tmp 把drcom的ipk拖入，然后点击上方命令行
opkg install 2100.ipk
configure即为成功，重启路由器
进行drcom和privoxy的代理
