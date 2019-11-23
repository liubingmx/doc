- Q: git clone 
```
fatal: unable to access 'https://github/liubing/demo.git': SSL certificate problem: self signed certificate
```
- A: ignore http.verify
```
git  -c http.sslVerify=false clone https://github/liubing/demo.git
```

- Q: oracle ORA-00257: archiver error. Connect internal only, until freed
```
oracle 审计日志清理
--进入审计日志目录：

cd $ORACLE_BASE/admin/$ORACLE_SID/adump

--删除3个月前的审计文件：

find ./ -type f -name "*.aud" -mtime +91|xargs rm -f

--一次清空所有审计文件

find ./ -type f -name "*.aud"|xargs rm-f

find ./ -mtime +7 -name "*.aud" -type f –delete
```
