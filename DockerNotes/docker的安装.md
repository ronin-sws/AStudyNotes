## CentOS7 环境
- 卸载old版本（若是首次安装则跳过）
```python
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 安装 Docker Engine-Community
> 使用 Docker 仓库进行安装,首次安装 Docker Engine-Community 之前，需要设置 Docker 仓库。之后，您可以从仓库安装和更新 Docker。

#### 设置仓库
	- 安装所需的软件包。yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2。

```python
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

- 设置稳定的仓库。
```python
sudo yum-config-manager \
    --add-repo \ https://download.docker.com/linux/centos/docker-ce.repo
```

- 上面的安装地址可换成别的源
	```python
	sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
	```

### 安装 Docker Engine-Community
```python 
sudo yum install docker-ce docker-ce-cli containerd.io
```
- 安装过程中接受GPG秘钥

- 列出并排序您存储库中可用的版本。
	```python
	yum list docker-ce --showduplicates | sort -r
	```

- 安装特定版本
	```python
sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
	```

- 启动docker
```python
sudo systemctl start docker
```

- 验证是否安装成功
```python
sudo docker run hello-world
```

