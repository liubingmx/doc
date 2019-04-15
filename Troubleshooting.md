- Q: git clone 
```
fatal: unable to access 'https://github/liubing/demo.git': SSL certificate problem: self signed certificate
```
- A: ignore http.verify
```
git  -c http.sslVerify=false clone https://github/liubing/demo.git
```
