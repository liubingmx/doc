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

### pyenv (python virtual env)

>https://github.com/pyenv/pyenv
>https://github.com/pyenv/pyenv-virtualenv

- Installing  for macOS.
```
$ brew install pyenv
```
- pyenv-virtualenv, Installing with Homebrew (for macOS users)
This is the recommended method of installation if you installed pyenv with Homebrew.
```
$ brew install pyenv-virtualenv
```
Or, if you would like to install the latest development release:
```
$ brew install --HEAD pyenv-virtualenv

```
After installation, you'll still need to add to your profile (as stated in the caveats). You'll only ever have to do this once.
```
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```
### pyenv commands
- 查询pyenv版本：`pyenv -v`
- 查询python可安装版本：`pyenv install -l or  pyenv install --list`
- 安装python：`pyenv install <version> or pyenv install -v <version> 如：python install 3.7.1`
- 查询已安装Python所有版本：`pyenv versions`
- 查询当前使用的Python版本：`pyenv version`
- 设置全局python版本：`pyenv global <version>`
- 设置当前所在目录的python版本：`pyenv local <version>`
- 设置当前所在目标的python shell版本：`pyenv shell <version>`
### virtual-env
- 创建虚拟环境：
 `pyenv virtualenv 3.7.0 virtualenv-name`
- 切换虚拟环境：`pyenv activate <folder_name>`
- 退出虚拟环境：`pyenv deactivate`
- 列出当前所有的虚拟环境：`pyenv virtualenvs`
- 删除虚拟环境：`pyenv virtualenv-delete <folder_name>`


## Django
- version
```
$ python -m django --version
```
- run server
```
$ python manage.py runserver
```

## Celery
 celery not only schedule
- install
```
$ pip install -U Celery
$ pip install celery[librabbitmq,redis,auth,msgpack]

```

