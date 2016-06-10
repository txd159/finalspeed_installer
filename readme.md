###开放端口命令
service iptables start
iptables -I INPUT -p tcp --dport 端口号 -j ACCEPT
iptables -I OUTPUT -p tcp --sport 端口号 -j ACCEPT
service iptables save


###安装 
rm -f install_fs.sh

wget  https://raw.githubusercontent.com/txd159/finalspeed_installer/master/install_fs.sh

chmod +x install_fs.sh

./install_fs.sh 2>&1 | tee install.log



###卸载
sh /fs/stop.sh ; rm -rf /fs

###启动
sh /fs/start.sh; tail -f /fs/server.log


###停止
sh /fs/stop.sh


###重新启动
sh /fs/restart.sh; tail -f /fs/server.log


###查看日志
tail -f /fs/server.log

###设置服务端口
默认udp 150和tcp 150 ,修改端口后服务端会自动修改防火墙.
linux版: mkdir -p /fs/cnf/ ; echo 端口号 > /fs/cnf/listen_port ; sh /fs/restart.sh
windows 版: 在cnf目录下新建文件listen_port,文件内容为端口号.
注意:由于finalspeed的工作原理,不要开放finalspeed所使用的tcp端口.

###设置开机启动
chmod +x /etc/rc.local
vi /etc/rc.local
加入
sh /fs/start.sh

###每天晚上3点自动重启
crontab -e
加入
0 3 * * *  sh /fs/restart.sh