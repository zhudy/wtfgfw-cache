# wtfgfw-cache
local cache to fuck GFW

原理：
0. 对于某些源可以简单改为采用国内csp的源
   比如 /etc/apt/sources.list 改用清华的
1. 不改源而改ip地址的办法：本地启动www服务，缓存并提供经常受墙影响的URL网址文件
2. 修改/etc/resolv.conf将特定网址指向本地ip，达到实际访问的是本地的本地缓存来绕开防火墙




