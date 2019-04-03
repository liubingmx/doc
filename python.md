### Django connect mysql for macos

- You may need to install the Python and MySQL development headers and libraries
```
brew install mysql-connector-c


```
macOS (Homebrew) (Currently, it has bug.)
Modification of mysql_config resolves these issues as follows.

 Change
```
sudo vim /usr/local/bin/mysql_config 

# on macOS, on or about line 112:
# Create options
libs="-L$pkglibdir"
libs="$libs -l "
```
to 
``` 
# Create options
libs="-L$pkglibdir"
libs="$libs -lmysqlclient -lssl -lcrypto"
```

- mysqlclient install 
```
pip3 install mysqlclient
```
