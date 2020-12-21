> ubuntu 12.04默认安装python2.7和python3.2，现因python3.2不满足新项目运行环境，固安装python3.6.5,以下是完成的安装流程（在跨过很多的坑后总结的方法）

## 安装python 

①：[python3.6.5下载链接](https://www.python.org/downloads/release/python-365/ "python3.6.5下载链接")
![file](http://sswss5.cn/wp-content/uploads/2020/06/5eee1fc395fe6.png)

②：上传源码到ubuntu系统（任意位置都可，只要自己能找的到）
```python
scp -r Python-3.6.5.tgz 用户名@公网IP:/home/baohe
```

③：解压
```python
tar xfz Python-3.6.5.tgz
```
![file](http://sswss5.cn/wp-content/uploads/2020/06/5eee21455cde3.png)

④：进入解压后的Python3.6.5的目录中,执行以下命令
```python
./configure  --prefix=/usr/local/python3  # --prefix后面是python3的安装路径

sudo make

sudo make install
```

⑤：等待安装完成后，查看python 安装位置，发现并没有python3，原因是没有建立python3的软连接
```python
whereis python
```
![file](http://sswss5.cn/wp-content/uploads/2020/06/5eee22ee8ba9f.png)

- 查看系统以及存在的关于python的软连接
```python
cd /usr/bin/
ls -il | grep python
```
![file](http://sswss5.cn/wp-content/uploads/2020/08/5f2a5fd1e5d6b.png)


⑥：建立python3.6的软连接(需要先删除旧的软链接)
```python
sudo ln -s /usr/local/python3/bin/python3 /usr/bin/python3
# 删除软连接的命令：sudo rm /usr/bin/python3
```

⑦：建立pip3的软连接(需要先删除旧的软链接)
```python
sudo ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
# 备注：默认安装的pip版本比较低，需要升级，升级的命令为：
sudo pip2 install --upgrade pip
sudo pip3 install --upgrade pip
```

⑧：查看软连接是否建立成功
![file](http://sswss5.cn/wp-content/uploads/2020/06/5eee2450ed0c7.png)

⑨：如果想实现命令python2执行的python2.7,命令python3执行的是python3.6，将python2.7的软连接重新设置（方法如设置python3软连接），测试：
![file](http://sswss5.cn/wp-content/uploads/2020/06/5eee25ca5ca68.png)

## 安装虚拟环境
①：安装virtualenv 和 virtualenvwrapper

- 将默认的pip源更换为国内的镜像源，常用的国内的镜像源如下
	- 阿里云 http://mirrors.aliyun.com/pypi/simple/ 
	- 中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/ 
	- 豆瓣(douban) http://pypi.douban.com/simple/ 
	-  清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/ 
	- 中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/

- 【永久更改】创建~/.pip/pip.conf 文件，并编辑如下内容保存
```python
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

- 【临时更改】
```python
pip install scrapy -i https://pypi.tuna.tsinghua.edu.cn/simple
```

- 安装virtualenv 和virtualenvwrapper
```python
sudo pip3 install virtualenv
sudo pip3 install virtualenvwrapper
```

②：创建虚拟环境管理目录
```python
mkdir ~/.virtualenvs
```
![file](http://sswss5.cn/wp-content/uploads/2020/06/5eee293940c19.png)

③：编辑配置文件：vim ~/.bashrc,添加如下内容：
```python
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export WORKON_HOME=$HOME/.virtualenvs  # 所有虚拟环境存储的目录
source /usr/local/bin/virtualenvwrapper.sh
```

④：启动配置文件
```python
source ~/.bashrc
```

⑤：创建python3 和python2的虚拟环境
```python
mkvirtualenv -p python3 py3env
mkvirtualenv -p python2 py2env
```

⑥：常用命令
```python
workon  # 查看 或者进入环境（+环境名称）
lsvirtualenv # 列出所有环境
deactivate # 退出虚拟环境
```
![file](http://sswss5.cn/wp-content/uploads/2020/06/5eee2ae1653a7.png)
