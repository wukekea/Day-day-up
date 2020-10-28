* CentOS系统没有窗口最小化最大化功能，需要下载软件。方法如下：
    * 下载安装gnome-tweaks ：yum install gnome-tweaks
    * 执行 gnome-tweaks命令
    * 选择windows菜单，打开Maximize，Minimize选项
    * 此时最小化窗口时无法像Windows一样在底部显示。可以再选择extension菜单，打开windows list即可。

* centos8设置、查看、删除环境变量的方法 
    * 1. 显示环境变量HOME(红色部分代表要输入的命令，不要把一个#号输进去了，#代表用户名) <br>
    ```bash
    # echo $HOME 
    ```

    * 2. 设置一个新的环境变量hello <br>
    ```bash
    # export HELLO="Hello!"
    # echo &HELLO 
    Hello!
    ```

    * 3. 使用env命令显示所有的环境变量 <br>
    ```bash
    # env 
    HOSTNAME=redbooks.safe.org
    PVM_RSH=/usr/bin/rsh
    Shell=/bin/bash
    TERM=xterm
    HISTSIZE=1000
    ... 
    ```

    * 4. 使用set命令显示所有本地定义的Shell变量 <br>
    ```bash
    # set 
    BASH=/bin/bash 
    BASH_VERSINFO=([0]="2"[1]="05b"[2]="0"[3]="1"[4]="release"[5]="i386-redhat-linux-gnu") 
    BASH_VERSION='2.05b.0(1)-release'
    COLORS=/etc/DIR_COLORS.xterm
    COLUMNS=80 
    DIRSTACK=()
    DISPLAY=:0.0 
    ... 
    ```

    * 5. 使用unset命令来清除环境变量 <br>
    set可以设置某个环境变量的值。清除环境变量的值用unset命令。如果未指定值，则该变量值将被设为NULL。示例如下： <br>
    ```bash
    # export TEST="Test..."             #增加一个环境变量TEST 
    # env|grep TEST                     #此命令有输入，证明环境变量TEST已经存在了
    TEST=Test... 
    # unset #TEST                       #删除环境变量TEST 
    # env|grep TEST                     #此命令没有输出，证明环境变量TEST已经存在了
    ```

    * 6. 使用readonly命令设置只读变量 <br>
    如果使用了readonly命令的话，变量就不可以被修改或清除了。示例如下： 
    ```bash
    # export TEST="Test..."             #增加一个环境变量TEST 
    # readonly TEST                     #将环境变量TEST设为只读 
    # unset TEST                        #会发现此变量不能被删除 
    -bash: unset: TEST: cannot unset: readonly variable 
    # TEST="New"                        #会发现此也变量不能被修改 
    -bash: TEST: readonly variable 
    ```


    * 环境变量的设置位于/etc/profile文件 
    * 如果需要增加新的环境变量可以添加下属行 
    * export path=$path:/path1:/path2:/pahtN 

* 安装好的centos系统默认是没有中文输入法的，只能输入英文字符。需要手动添加。方法如下<br>
设置 -> 语言 -> （添加失败）

* linux系统常用的shell命令<br>
1，复制文件：cp -a app-package-manager /root/Documents/app-package-manager<br>
2，新建文件夹：mkdir file<br>
3，新建文件：touch a.txt<br>
4，删除文件：rm -i filename
5，删除文件夹：rm -ir filedir
6，直接删除文件夹（不询问）：rm -rf filedir
