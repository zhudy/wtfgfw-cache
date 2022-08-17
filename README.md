# wtfgfw-cache
local cache to fuck GFW

原理：
0. 对于某些源可以简单改为采用国内csp的源
   比如 /etc/apt/sources.list 改用清华的
1. 不改源而改ip地址的办法：本地启动www服务，缓存并提供经常受墙影响的URL网址文件
2. 修改/etc/resolv.conf将特定网址指向本地ip，达到实际访问的是本地的本地缓存来绕开防火墙


问题：
   域名证书怎么办? 比如访问的是https://github.com
	思路1：改造本机客户端下载软件wget, curl, 改https 为http?
	思路2：配置客户端软件走proxy访问, 而proxy拦截改成访问本地http。如果走这个途径就无需修改/etc/resolv.conf了。

实操：
1. 创建缓存目录，并将文件拷贝进去
    mkdir cache

2. www缓存服务器 Squid proxy server: https://hub.docker.com/r/datadog/squid#introduction https://github.com/sameersbn/docker-squid
   //docker pull datadog/squid
   //docker run --name squid -d -p 3128:3128 datadog/squid
   //docker run --name squid -p 3128:3128 datadog/squid
   docker run --rm --name squid -p 3128:3128 --volume $PWD/cache:/var/spool/squid datadog/squid
   在另外的窗口可以sudo su 然后看 find cache
   浏览器设置SwitchyOmega默认: 代理协议 HTTP, 代理服务器 localhost, 代理端口3128, 高级设置里头选择同默认
   wget命令行： 设置 export https_proxy=http://localhost:3128

2do: 
1. 验证以上设置搬迁有效
2. 如果无法访问的文件通过别的方法下载后，怎么变成cache里的可以使用的数据？

3. goproxy服务器: https://goproxy.cn
添加到Dockerfile中的Run
RUN go env -w GO111MODULE=on && go env -w GOPROXY=https://goproxy.cn,direct && \
