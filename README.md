# logrotate
centos自带的日志切割工具logrotate,方便机器每天分割累计的日志转存,默认每天晚上由crontab在12点执行

## 1.认识相关路径
```
/etc/logrotate.d/xxx        xxx放置自己需要的切割配置
/etc/cron.daily/logrotate   默认crontab每天运行内容
/etc/logrotate.config       主文件配置一般不用动
/var/lib/logrotate/status   记录上次转存的时间
```
## 2.配置
example 配置完保存在/etc/logrotate.d/下，若文件名假设为v2ray，即/etc/logrotate.d/v2ray，括号前是需要切割路径，内部配置细节
```
/var/log/v2ray/*.log {
	su root root
 	daily
 	rotate 10
 	olddir /home/v2ray-log
 	missingok
 	nocompress
	dateext
 	create 644 root root
}
```
详细参数说明
```
compress             通过gzip 压缩转储以后的日志
nocompress           不需要压缩时，用这个参数
dateext              为日志文件打上日期标签
copytruncate         用于还在打开中的日志文件，把当前日志备份并截断
nocopytruncate       备份日志文件但是不截断
create mode owner group 转储文件，使用指定的文件模式创建新的日志文件
nocreate             不建立新的日志文件
delaycompress 和 compress 一起使用时，转储的日志文件到下一次转储时才压缩
nodelaycompress      覆盖 delaycompress 选项，转储同时压缩。
errors address       专储时的错误信息发送到指定的Email 地址
ifempty              即使是空文件也转储，这个是 logrotate 的缺省选项。
notifempty           如果是空文件的话，不转储
mail address         把转储的日志文件发送到指定的E-mail 地址
nomail               转储时不发送日志文件
olddir               转储后的日志文件放入指定的目录，必须和当前日志文件在同一个文件系统
noolddir             转储后的日志文件和当前日志文件放在同一个目录下
prerotate/endscript  在转储以前需要执行的命令可以放入这个对，这两个关键字必须单独成行
postrotate/endscript 在转储以后需要执行的命令可以放入这个对，这两个关键字必须单独成行
daily                指定转储周期为每天
weekly               指定转储周期为每周
monthly              指定转储周期为每月
rotate count         指定日志文件删除之前转储的次数，0 指没有备份，5 指保留5 个备份
tabootext [+] list   让logrotate 不转储指定扩展名的文件，缺省的扩展名是：.rpm-orig, .rpmsave, v, 和 ~
size size            当日志文件到达指定的大小时才转储，Size 可以指定 bytes (缺省)以及KB (sizek)或者MB (sizem).
```
## 3.检查配置
```
logrotate -d /etc/logrotate.d/v2ray
```
如果-d的debug看起来没有异样如下
```
reading config file /etc/logrotate.d/v2ray
olddir is now /home/v2ray-log
Allocating hash table for state file, size 15360 B

Handling 1 logs

rotating pattern: /var/log/v2ray/access.log /var/log/v2ray/error.log  after 1 days (10 rotations)
olddir is /home/v2ray-log, empty log files are rotated, old logs are removed
considering log /var/log/v2ray/access.log
  log does not need rotating (log has been rotated at 2018-10-20 22:30, that is not day ago yet)
considering log /var/log/v2ray/error.log
  log /var/log/v2ray/error.log does not exist -- skipping
```
可以手动执行-f的force，适当打出细节-v
```
logrotate -vf /etc/logrotate.d/v2ray
```
