#### docker
- logs
```
	$ dozcker-compose logs -f --tail=20

	$ docker logs --tail="10" imageid
```

#### git

- 
```
	$ git push origin new_branch    # push new branch to remote
	
	$ git remote -v
	$ git remote set-url origin https://***   # change remote repository
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

- create configmap from file
```
$ kubectl create configmap confnginx --from-file=nginx.conf
```



