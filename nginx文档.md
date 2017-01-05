# nginx-rtmp使用

```bash
linux上ppa安装ffmpeg（ubuntu 14.04测试通过）
sudo add-apt-repository ppa:kirillshkrogalev/ffmpeg-next
sudo apt-get update
sudo apt-get install ffmpeg
```
```bash
安装编译 nginx 所需要的库
sudo apt-get install build-essential libpcre3 libpcre3-dev libssl-dev
下载 nginx-1.9.15.tar.gz
wget http://nginx.org/download/nginx-1.9.15.tar.gz
下载 nginx-rtmp-module
wget https://github.com/arut/nginx-rtmp-module/archive/master.zip
解压
tar -zxvf nginx-1.9.15.tar.gz
unzip master.zip
cd nginx-1.9.15
编译
./configure --with-http_ssl_module --add-module=../nginx-rtmp-module-master
make
sudo make install
```



