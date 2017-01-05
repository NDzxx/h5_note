# nginx-rtmp使用
##nginx-rtmp
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
##ffmpeg
```bash
linux上ppa安装ffmpeg（ubuntu 14.04测试通过）
sudo add-apt-repository ppa:kirillshkrogalev/ffmpeg-next
sudo apt-get update
sudo apt-get install ffmpeg
```
##使用命令
```bash
关闭
sudo /usr/local/nginx/sbin/nginx -s stop  
开启
sudo /usr/local/nginx/sbin/nginx
重新载入配置（重启）
sudo /usr/local/nginx/sbin/nginx -s reload
```
##nginx-rtmp hls配置
参考链接：

[Mac直播服务器Nginx配置对HLS的支持](http://www.cnblogs.com/jys509/p/5653720.html)

[mac上搭建nginx-rtmp服务](http://www.cnblogs.com/jys509/p/5649066.html)

[使用Nginx+FFMPEG搭建HLS直播转码服务器 ](http://blog.csdn.net/wutong_login/article/details/42292787)

```
worker_processes  1;

error_log  logs/error.log debug;

events {
    worker_connections  1024;
}

rtmp {
    server {
        listen 1935;
		application zbcs {
               live on;
               #注释内容直线于mac和linux，在win下目前不可用
		#exec_static ffmpeg -i rtmp://192.168.233.130:1935/zbcs/$name 
		#-c:a aac -b:a 32k -c:v libx264 -b:a 32k -c:v libx264 -b:v 128K -f
		#rtmp://192.168.233.130:1935/hls/$name_low;
		}
				
        application hls {
            live on;
		    hls on;  
		    hls_path temp/zxx;  
		    hls_fragment 5s;
			hls_nested on;
        }
       	
		application small {
            live on;
		}
   }

}

http {
    server {
        listen      8082;
		
        location / {
            root www;
        }
		
		location /stat {     #第二处添加的location字段。
            rtmp_stat all;
			rtmp_stat_stylesheet stat.xsl;
			#访问http://ip:8082/stat可以查看状态
		}

         location /stat.xsl { #第二处添加的location字段。
			root www;
		}
		
		location /control {    
            rtmp_control all;    
        }    
  
				
		location /hls {  
           #server hls fragments  
			types{  
				application/vnd.apple.mpegurl m3u8;  
				video/mp2t ts;  
			}  
			alias temp/zxx;
			add_header Cache-Control no-cache;
			
        }  

    }
}


```
原因：[详细原因地址](https://helping-squad.com/transcoding-your-video-with-nginx/)

As mentioned earlier under Windows you cannot add the commandline directly to the config. 

For Windows just setup a batch file which you can start with a desktop shortcut.

 You can then use the shortcut when necessary to start the transcoding.
 
 Win的做法：
 把以下代码保存成bat,手动启动即可
 ```bash
 ffmpeg -i rtmp://192.168.233.130:1935/zbcs/113  
      -c:a aac -b:a 32k -c:v libx264 -b:v 128K
      -f flv rtmp://192.168.233.130:1935/hls/113_low 

 ```
 
 [ffmpeg常用基本命令](http://www.cnblogs.com/wainiwann/p/4128154.html)
 







