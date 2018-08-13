## Ubuntu 上安装 python3.6

```
# 下载源码包
wget https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tar.xz
# cd 到m源码包所在目录下
tar -zxvf Python-3.6.6.tar.xz
cd Python3.6.6
./configure
make
sudo make intall
```
实际安装完成后发现一个问题，pip3 不能正常使用, ssl module不能正常工作。
重新执行如下命令后
```
sudo apt-get install openssl
sudo apt-get install libssl-dev
```
仍然不可以！

### 解决方法如下：
修改Moudles/Setup  (该目录在python的解压目录下)
```
vim Modules/Setup
```
改成如下：
```
# Socket module helper for socket(2)
_socket socketmodule.c timemodule.c
 
# Socket module helper for SSL support; you must comment out the other
# socket line above, and possibly edit the SSL variable:
#SSL=/usr/local/ssl
_ssl _ssl.c \
-DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
-L$(SSL)/lib -lssl -lcrypto
```
### 重新安装一次
```
./configure
make
sudo -H make install
```
