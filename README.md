# logrotate
centos自带的日志切割工具logrotate,方便机器每天分割累计的日志转存

## 1.认识相关路径
```
/etc/logrotate.d/xxx        xxx放置自己需要的切割配置
/etc/logrotate.config        主文件配置一般不用动
/var/lib/logrotate/status   记录上次转存的时间
```
