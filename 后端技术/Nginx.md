* nginx官网：http://nginx.org/en/docs/

### 什么是nginx？
* Nginx同Apache一样都是一种WEB服务器。基于REST架构风格，以统一资源描述符(Uniform Resources Identifier)URI或者统一资源定位符(Uniform Resources Locator)URL作为沟通依据，通过HTTP协议提供各种网络服务。

* Nginx (engine x) 是一个高性能的HTTP和反向代理web服务器，同时也提供了IMAP/POP3/SMTP服务。Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。

* Nginx 可以在大多数 Unix Linux OS 上编译运行，并有 Windows 移植版。Nginx作为负载均衡服务：Nginx 既可以在内部直接支持 Rails 和 PHP 程序对外进行服务，也可以支持作为 HTTP代理服务对外进行服务。Nginx采用C进行编写，不论是系统资源开销还是CPU使用效率都比 Perlbal 要好很多。<br>
处理静态文件，索引文件以及自动索引;打开文件描述符缓冲。<br>
无缓存的反向代理加速，简单的负载均衡和容错。<br>
FastCGI，简单的负载均衡和容错。<br>
模块化的结构。包括 gzipping, byte ranges, chunked responses,以及 SSI-filter 等 filter。如果由 FastCG或其它代理服务器处理单页中存在的多个 SSI，则这项处理可以并行运行，而不需要相互等待。

* Nginx 是一个安装非常的简单、配置文件非常简洁（还能够支持perl语法）、Bug非常少的服务。Nginx 启动特别容易，并且几乎可以做到7*24不间断运行，即使运行数个月也不需要重新启动。你还能够不间断服务的情况下进行软件版本的升级。

* 在连接高并发的情况下，Nginx是Apache服务器不错的替代品。

* 对于Nginx和Tomcat，Nginx 常作为负载均衡服务器和静态资源服务器放在最前端，后面是Tomcat组成的集群。如果用户请求的是静态资源，Nginx直接搞定，不用麻烦后面的tomcat了。如果是动态资源（如xxx.jsp）, Nginix 就会按照一定的算法转发到某个Tomcat上，达到负载均衡的目的。

### 什么是反向代理？
* 关于代理：所谓代理就是一个代表、一个渠道。<br>
此时就涉及到两个角色，一个是被代理角色，一个是目标角色，被代理角色通过这个代理访问目标角色完成一些任务的过程称为代理操作过程；如同生活中的专卖店~客人到adidas专卖店买了一双鞋，这个专卖店就是代理，被代理角色就是adidas厂家，目标角色就是用户。

* 说反向代理之前，我们先看看正向代理，正向代理也是大家最常接触的到的代理模式，我们会从两个方面来说关于正向代理的处理模式，分别从软件方面和生活方面来解释一下什么叫正向代理。<br>
在如今的网络环境下，我们如果由于技术需要要去访问国外的某些网站，此时你会发现位于国外的某网站我们通过浏览器是没有办法访问的，此时大家可能都会用一个操作FQ进行访问，FQ的方式主要是找到一个可以访问国外网站的代理服务器，我们将请求发送给代理服务器，代理服务器去访问国外的网站，然后将访问到的数据传递给我们！<br>
上述这样的代理模式称为正向代理，正向代理最大的特点是客户端非常明确要访问的服务器地址；服务器只清楚请求来自哪个代理服务器，而不清楚来自哪个具体的客户端；正向代理模式屏蔽或者隐藏了真实客户端信息。<br>
总结来说：正向代理，"它代理的是客户端，代客户端发出请求"，是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端必须要进行一些特别的设置才能使用正向代理。<br>
正向代理的用途：<br>
（1）访问原来无法访问的资源，如Google<br>
（2）可以做缓存，加速访问资源<br>
（3）对客户端访问授权，上网进行认证<br>
（4）代理可以记录用户访问记录（上网行为管理），对外隐藏用户信息<br>

* 明白了什么是正向代理，我们继续看关于反向代理的处理方式，举例如我大天朝的某宝网站，每天同时连接到网站的访问人数已经爆表，单个服务器远远不能满足人民日益增长的购买欲望了，此时就出现了一个大家耳熟能详的名词：分布式部署；也就是通过部署多台服务器来解决访问人数限制的问题；某宝网站中大部分功能也是直接使用Nginx进行反向代理实现的，并且通过封装Nginx和其他的组件之后起了个高大上的名字：Tengine，有兴趣的童鞋可以访问Tengine的官网查看具体的信息：http://tengine.taobao.org/。<br>
反向代理是相对于通常的代理服务器来说的，普通的Web代理服务器不支持外部对内部网络的访问请求，当一个代理服务器能够代理外部网络上的主机，访问内部网络时，这种代理服务的方式称为反向代理服务。反向代理方式是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。<br>
多个客户端给服务器发送的请求，Nginx服务器接收到之后，按照一定的规则分发给了后端的业务处理服务器进行处理了。此时~请求的来源也就是客户端是明确的，但是请求具体由哪台服务器处理的并不明确了，Nginx扮演的就是一个反向代理角色。<br>
客户端是无感知代理的存在的，反向代理对外都是透明的，访问者并不知道自己访问的是一个代理。因为客户端不需要任何配置就可以访问。<br>
反向代理，"它代理的是服务端，代服务端接收请求"，主要用于服务器集群分布式部署的情况下，反向代理隐藏了服务器的信息。<br>
反向代理的作用：<br>
（1）保证内网的安全，通常将反向代理作为公网访问地址，Web服务器是内网<br>
（2）负载均衡，通过反向代理服务器来优化网站的负载<br>


### nginx的安装
* 安装编译工具及库文件
```bash
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
```

* 首先要安装 PCRE。PCRE 作用是让 Nginx 支持 Rewrite 功能。`未完成`<br>
下载地址：http://www.pcre.org/<br>
https://www.cnblogs.com/crazylqy/p/6891929.html<br>
    ```bash
    # 解压安装包
    tar zxvf pcre-8.35.tar.gz
    # 进入安装目录
    cd pcre-8.35
    # 编译安装
    ./configure
    make && make install
    # 查看版本
    pcre-config --version
    ```

* 安装nginx（失败）
    ```bash
    # 下载nginx
    wget http://nginx.org/download/nginx-1.6.2.tar.gz
    # 解压缩安装包
    tar zxvf nginx-1.6.2.tar.gz
    # 进入安装目录
    cd nginx-1.6.2
    # 编译安装
    ./configure --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.35
    ```

* 安装nginx（centos）<br>
nginx官网：http://nginx.org/en/docs/

* 第一次在电脑上安装nginx时，需要先安装nginx的包仓库（nginx packages repository）。之后我们可以通过这个包仓库安装和更新nginx。

    1， 安装先决条件<br>
    sudo yum install yum-utils<br>
    2， 为了设置yun仓库，先创建文件：<br>
    touch /etc/yum.repos.d/nginx.repo<br>
    3，粘贴文件内容<br>
    ```
    [nginx-stable]
    name=nginx stable repo
    baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
    gpgcheck=1
    enabled=1
    gpgkey=https://nginx.org/keys/nginx_signing.key
    module_hotfixes=true

    [nginx-mainline]
    name=nginx mainline repo
    baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
    gpgcheck=1
    enabled=0
    gpgkey=https://nginx.org/keys/nginx_signing.key
    module_hotfixes=true
    ```
    4，By default, the repository for stable nginx packages is used. If you would like to use mainline nginx packages, run the following command:<br>
    sudo yum-config-manager --enable nginx-mainline<br>
    5，运行下列命令来安装nginx<br>
    sudo yum install nginx<br>
    当提示需要接受 GPG key时，先确定它是不是`573B FD6B 3D8F BC64 1079 A6AB ABF5 BD82 7BD9 BF62`。如果是，就接受<br>
    6，运行nginx<br>
    nginx<br>
    7，访问nginx：浏览器中输入：http://localhost:80 <br>
    8，查看运行中的nginx进程：<br>
    ps -ax | grep nginx<br>
    9，查看nginx的启动用户：


* nginx包括一个主进程和多个工作进程。主进程用来读取和分析配置文件，并维护工作进程。工作进程处理实际的请求。nginx采用时间驱动模型。工作进程的数量定义在配置文件中并且会根据可用的cpu数量自动调整。nginx的工作方式定义在配置文件中。默认情况下，配置文件位于：/usr/local/nginx/conf, `/etc/nginx`（here）, 或者 /usr/local/etc/nginx.

* nginx命令如下：<br>
nginx -s signal      `signal可以为：`
    * stop — fast shutdown
    * quit — graceful shutdown(它会等待工作进程先完成当前的请求)
    * reload — reloading the configuration file
    * reopen — reopening the log files

* nginx配置文件的结构<br>
nginx consists of modules which are controlled by `directives` specified in the configuration file. Directives are divided into `simple directives` and `block directives`. A `simple directive` consists of the name and parameters separated by spaces and ends with a semicolon (;). A `block directive` has the same structure as a simple directive, but instead of the semicolon it ends with a set of additional instructions surrounded by braces ({ and }). If a block directive can have other directives inside braces, it is called a `context` (examples: events, http, server, and location).<br>
Directives placed in the configuration file outside of any contexts are considered to be in the main context. The events and http directives reside in the main context, server in http, and location in server.<br>
The rest of a line after the # sign is considered a comment.

* 使用nginx提供静态内容<br>
An important web server task is serving out files (such as images or static HTML pages). You will implement an example where, depending on the request, files will be served from different local directories: /data/www (which may contain HTML files) and /data/images (containing images). This will require editing of the configuration file and setting up of a server block inside the http block with two location blocks.<br>
    * First, create the /data/www directory and put an index.html file with any text content into it and create the /data/images directory and place some images in it.
    * Next, open the configuration file. The default configuration file already includes several examples of the server block, mostly commented out. For now comment out all such blocks and start a new server block: <br>
        ```
        http {
            server {
            }
        }
        ```
        Generally, the configuration file may include several server blocks distinguished by ports on which they listen to and by server names. Once nginx decides which server processes a request, it tests the URI specified in the request’s header against the parameters of the location directives defined inside the server block.

        Add the following location block to the server block:
        ```
        location / {
            root /data/www;
        }
        ```
        This location block specifies the “/” prefix compared with the URI from the request. For matching requests, the URI will be added to the path specified in the root directive, that is, to /data/www, to form the path to the requested file on the local file system. If there are several matching location blocks nginx selects the one with the longest prefix. The location block above provides the shortest prefix, of length one, and so only if all other location blocks fail to provide a match, this block will be used.<br>
        Next, add the second location block:

        ```
        location /images/ {
            root /data;
        }
        ```
        It will be a match for requests starting with /images/ (location / also matches such requests, but has shorter prefix).

        The resulting configuration of the server block should look like this:
        ```
        server {
            location / {
                root /data/www;
            }

            location /images/ {
                root /data;
            }
        }
        ```

        This is already a working configuration of a server that listens on the standard port 80 and is accessible on the local machine at http://localhost/. In response to requests with URIs starting with /images/, the server will send files from the /data/images directory. For example, in response to the http://localhost/images/example.png request nginx will send the /data/images/example.png file. If such file does not exist, nginx will send a response indicating the 404 error. Requests with URIs not starting with /images/ will be mapped onto the /data/www directory. For example, in response to the http://localhost/some/example.html request nginx will send the /data/www/some/example.html file.

        To apply the new configuration, start nginx if it is not yet started or send the reload signal to the nginx’s master process, by executing:
        ```
        nginx -s reload
        ```

        注意：In case something does not work as expected, you may try to find out the reason in access.log and error.log files in the directory /usr/local/nginx/logs or /var/log/nginx.

        访问 http://localhost/images/example.png 可能出现的问题：403forbiden；<br>
        解决方法：<br>
        可能是nginx的启动用户和系统用户不一样。将nginx.conf里面的user改为root（当前用户）即可
        