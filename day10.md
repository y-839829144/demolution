## 今天工作安排

1. 生成小程序并进行测试
2. 测试成功后修改样式
3. 修改样式成功后添加功能





Llinux微信web开发者工具安装



```
linux 下使用微信web开发者工具
wx dev tools 
nw.js
微信开发者工具, 原理是 微信开发者工具 本质是 nw.js 程序. 负责编译 wxml 和 wxss 的 wcc 和 wcsc (可能还有其他功能), 则利用 wine 来跑即可


下载项目和初始化:
git clone https://github.com/cytle/wechat_web_devtools.git
cd wechat_web_devtools
# 自动下载最新 `nw.js` , 同时部署目录 `~/.config/wechat_web_devtools/`
./bin/wxdt install

启动ide,开发和调试网页
运行准备:

GUI环境
./bin/wxdt # 启动
启动ide，开发和预览小程序
运行准备:

GUI环境
需要安装wine
并且已经执行过./bin/wxdt install
./bin/wxdt # 启动

命令行和HTTP调用
运行准备:

GUI环境，命令行和HTTP调用会自动启动ide(服务器没条件的可以使用docker)
并且已经执行过./bin/wxdt install
在ide的设置中开启服务端口： 设置 -> 安全 -> 服务端口(开启)
命令行工具所在位置: <安装路径>/bin/cli

端口号文件位置：~/.config/wechat_web_devtools/Default/.ide

其它说明
安装Wine
请参考搜索引擎安装 Wine，以下是Ubuntu下两种安装

1. 安装wine-binfmt
sudo apt-get install wine-binfmt
sudo update-binfmts --import /usr/share/binfmts/wine
2. 正常安装wine
dpkg --add-architecture i386 \
  && wget -nc https://dl.winehq.org/wine-builds/winehq.key \
  && apt-key add winehq.key \
  && apt-add-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ bionic main' \
  && apt-get update \
  && apt-get install -y --no-install-recommends --allow-unauthenticated winehq-stable
./bin/wxdt install 报错失败
./nw: error while loading shared libraries: libnw.so: cannot open shared object file: No such file or directory

该错误是由 nw.js 下载失败所致. 删除缓存, 重新下载即可.

rm -rf /path/to/wechat_web_devtools/dist
rm -rf /tmp/wxdt_xsp
# 请务必等待执行完成
./bin/wxdt install
参考

https://github.com/cytle/wechat_web_devtools/issues/49#issuecomment-350478295
wcc 和 wcsc 编译错误
是wine没安装好导致的，或是没有成功替换wcc 和 wcsc两个二进制文件

方案一: 安装wine并且执行./bin/wxdt install
方案二: 安装wine-binfmt
完成后, 点击 编译 即可.

参考:

https://github.com/cytle/wechat_web_devtools/issues/66#issuecomment-368434141
https://github.com/cytle/wechat_web_devtools/issues/56#issuecomment-371999385
更新到最新版
方案一: 直接从当前项目源码 进行 更新 (稳定, 推荐)
git pull origin
方案二: 使用腾讯原始安装程序 进行 自助复制更新 (及时, 自行折腾)
注: 如果抽风了, 可以尝试使用 git reset --hard 等操作, 还原到最初的状态.

执行更新, 自动下载最新 Windows x64 版开发者工具, 并且使用7z解压.  

./bin/update_package_nw.sh
Tips

运行没问题，欢迎PR
```







总结今天所遇到的问题

1. 安装微信开发者工具
   * 1. 可以从github上直接git下来
     2. 之后进行解包 就OK了 这个是装完了 
     3. 然后之后进行的就是环境的搭建
     4. cd到devtool下 执行 ./bin/wxdt install
     5. 他会自己下载nw最新版本
     6. 之后就是wine的下载
     7. sudo apt-get install wine-binfmt
     8. wine下载完之后还是会报之前的错误
     9.  Syntax error: word unexpected (expecting ")")
     10. 是 语法错误：字词意外（预期为“）”）
     11. 所以想到是不是ubuntu字体的问题
     12. 然后去https://raw.githubusercontent.com/kakkoyun/linux.files/master/fonts/Consolas.ttf下载Consolas.ttf至usr/local/share/fonts Or~/.fonts
     13. 进行字体的配置
     14. sudo fc-cache-f重建字体缓存
     15. 确认字体安装成功
     16. sudo fc-list|grep Consol
     17. 结果:
     18. .fonts/Consolas.ttf.Consolas:style=Regular
     19. 重新启动微信开发者工具
     20. 记得每次启动前要输
     21. ./bin/wxdt install 成功后输入 ./bin/wxdt
     22. 这下不会报字体问题了
     23. 目前还是报错是VM58:1 appJSON["tabBar"][1]["pagePath"] "pages/list-product/index" 需在 pages 数组中

2. linux一些命令
   * dpkg 被中断,手工运行 'sudo dpkg --configure -a'解决
   * 如果软件包没有可用,但是它被其它的软件包引用了,这可能意味着这个缺失的软件包可能已经被废弃,或者只能在其他发布源中找到