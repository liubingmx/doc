#### docker
- logs
```
	$ dozcker-compose logs -f --tail=20

	$ docker logs --tail="10" imageid
```

#### git

- 
```
	$ git pull origin develop:develop # checkout remote branch, auto merge
	$ git checkout -b develop # checkout from local current branch
	
	$ git push origin new_branch    # push new branch to remote
	$ git branch --set-upstream-to=origin/<branch> # set tracking information for this branch
	
	$ git remote -v
	$ git remote set-url origin https://***   # change remote repository
	
	$ git checkout develop # switch branch
	$ git reflog show branch # show origin branch
	
	$  git branch -d branch_name # delete local branch
	# if SSL certificate problem: self signed certificate
	$ git -c http.sslVerify=false pull origin develop:develop
	
	$ git stash # save temp change
	$ git stash save "comment"
	
	$ git stash pop 
	$ git stash list 
	$ git stash apply stash@{0}


create a new repository on the command line
echo "# Java" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/liubingmx/Java.git
git push -u origin master
…or push an existing repository from the command line
git remote add origin https://github.com/liubingmx/Java.git
git push -u origin master
```
#### maven

```
	$ mvn clean install -DskipTests
```

#### redis
- 
```
	$ expire key 100
	$ TTL key #get expired time 

	$ FLUSHALL #remove all keys
```

#### mac

- inspect port
```
$ netstat -an | grep LISTEN
$ lsof -i:port
```

#### k8s

- create secret 
```
$  kubectl create secret docker-registry name-credentials \
 	 --docker-server=https://server \
	 --docker-username=name \
	 --docker-password=password \
	 --docker-email=test@email.com \
	 --namespace=${namespace}


$  kubectl create -f filename
$  kubectl delete -f filename
```

- manager many k8s-context
```
export KUBECONFIG=/path/to/standalone/.kube/config
```

#### helm
```
$ helm del --purge bundle
```










- create configmap from file
```
$ kubectl create configmap confnginx --from-file=nginx.conf
```


#### postgresql

```
psql -U 用户名 -d database

\password           设置密码。
\q                  退出。
\h                  查看SQL命令的解释，比如\h select。
\?                  查看psql命令列表。
\l                  列出所有数据库。
\c [database_name]  连接其他数据库。
\d                  列出当前数据库的所有表格。
\d [table_name]     列出某一张表格的结构。
\du                 列出所有用户。
\e                  打开文本编辑器。
\conninfo           列出当前数据库和连接的信息。
```








