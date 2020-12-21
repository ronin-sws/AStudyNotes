> 说明：当系统中安装有多个版本的python时，想要设置python3命令为最常用的python版本时，只需要修改python在系统环境中的软连接即可。

- 查看ubuntu系统中已安装的python的版本及软连接指向
```python
ls -al | grep python
```
![file](http://sswss5.cn/wp-content/uploads/2020/06/5eec25a50d247.png)

- 需求:将python3的软连接指向python3.6
 - 修改前：命令行输入python3
![file](http://sswss5.cn/wp-content/uploads/2020/06/5eec25fbe6f67.png)

- 修改：
①：找到python3.6的安装路径
![file](http://sswss5.cn/wp-content/uploads/2020/06/5eec270a61635.png)

②：移除原有的python3软连接
```python
sudo rm /usr/bin/python3
```

③：设置为新的指向
```python
sudo ln -s /usr/bin/python3.6/bin/python3 /usr/bin/python3
```
![file](http://sswss5.cn/wp-content/uploads/2020/06/5eec284b8d54f.png)
