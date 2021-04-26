我们这个课程基于CentOS 7版本的学习

Linux一切皆文件：文件就 读、写、（权限）

## 学习方式

1. 认识Linux
2. 基本的命令（重点：Git讲了一些基本的命令（文件操作、目录管理、文件属性、Vim编辑器、账号管理、磁盘管理...））
3. 软件的安装和部署！（java、tomact、docker）

## 走进Linux系统

1. 开机登录

   > ​	开机会启动很多程序。他们在Windows叫做“服务”（service），在Linux就叫做“守护进程”（daemon）。
   >
   > ​	开机成功后，它会显示一个文本登录界面，这个界面就是我们经常看到的登录界面，在这个登录界面中会提示用户输入用户名，而用户输入的用户将作为参数传给login程序来验证用户的身份，密码是不显示的，输完回车即可！
   >
   > ​	一般来说，用户的登录方式有三种：
   >
   > 	* 命令行登录
   > 	* ssh登录
   > 	* 图形界面登录
   >
   > ​	最高权限账户为root，可以操作一切！

2. 关机

   > ​	在linux领域内大多用在服务器上，很少遇到关机的操作。毕竟服务器上跑一个服务是永无止境的，除非特殊情况下，不得已才会关机。
   >
   > ​	关机指令为：shutdown；
   >
   > ```bash
   > sync # 将数据由内存同步到硬盘中。
   > 
   > shutdown # 关机指令，你可以man shutdown 来看一下帮助文档。例如你可以运行如下命令关机：
   > 
   > shutdown –h 10 # 这个命令告诉大家，计算机将在10分钟后关机
   > 
   > shutdown –h now # 立马关机
   > 
   > shutdown –h 20:25 # 系统会在今天20:25关机
   > 
   > shutdown –h +10 # 十分钟后关机
   > 
   > shutdown –r now # 系统立马重启
   > 
   > shutdown –r +10 # 系统十分钟后重启
   > 
   > reboot # 就是重启，等同于 shutdown –r now
   > 
   > halt # 关闭系统，等同于shutdown –h now 和 poweroff
   > ```
   >
   > ​	最后总结一下，不管是重启系统还是关闭系统，首先要运行sync命令，把内存中的数据写到磁盘中。

3. 系统目录结构

   1. 一切皆文件
   2. 根目录/ ，所有的文件都挂载在这个节点下

   > 登录系统后，在当前命令窗口下输入命令：
   >
   > ```bash
   > ls /
   > ```
   >
   > 你会看到如下图所示：
   >
   > ![1616575355228](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616575355228.png)
   >
   > 以下是对这些目录的解释：
   >
   > *  **/bin**：bin是Binary的缩写, 这个目录存放着最经常使用的命令。 
   > *  **/boot：** 这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。(不要动)
   > *  **/dev ：** dev是Device(设备)的缩写, 存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。 
   > *  **/etc： 这个目录用来存放所有的系统管理所需要的配置文件和子目录。** 
   > *  **/home：用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。** 
   > *  **/lib**：这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。(不要动)
   > *  **/lost+found**：这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。（存放突然关机的一些文件）
   > *  **/media**：linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。 
   > *  **/mnt**：系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。（我们稍后会把一些本地文件挂载在这个目录下）
   > *  **/opt：这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。** 
   > *  **/proc**：这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。（不用管）
   > *  **/root：该目录为系统管理员，也称作超级权限者的用户主目录。** 
   > *  **/sbin**：s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。 
   > *  **/srv**：该目录存放一些服务启动之后需要提取的数据。 
   > *  **/sys**：这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sysfs 
   > *  **/tmp：这个目录是用来存放一些临时文件的。 用完即丢的文件，可以放在这个目录下，比如：安装包！**
   > *  **/usr：这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录。** 
   > *  **/usr/bin：** 系统用户使用的应用程序。 
   > *  **/usr/sbin：** 超级用户使用的比较高级的管理程序和系统守护程序。 Super
   > *  **/usr/src：** 内核源代码默认的放置目录。 
   > *  **/var：这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。** 
   > *  **/run**：是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。 
   > * **/www :  存放服务器网站相关的资源，环境，网站的项目**

## 常用的基本命令（必掌握）

1. 目录管理

   1. 绝地路径和相对路径

      > 我们知道Linux的目录结构为树状结构，最顶级的目录为根目录 / 。
      >
      > 其他目录通过挂载可以将它们添加到树中，通过解除挂载可以移除它们。
      >
      > 在开始本教程前我们需要先知道什么是绝对路径与相对路径。
      >
      > 
      >
      > **绝对路径：**
      >
      > 路径的写法，由根目录 / 写起，例如：/user/share/doc这个目录。
      >
      > 
      >
      > **相对路径：**
      >
      >  路径的写法，不是由 / 写起，例如由 /usr/share/doc 要到 /usr/share/man 底下时，可以写成：cd ../man 这就是相对路径的写法！ 

   2. 处理目录的常用命令

      > 常用命令：
      >
      > * ls：列出目录
      > * cd：切换目录
      > * pwd：显示目前的目录
      > * mkdir：创建一个新的目录
      > * rmdir：删除一个空的目录
      > * cp：复制文件或目录
      > * rm：移除文件或目录
      > * mv：移动文件与目录，或修改文件与目录的名称
      >
      > 可以使用 *man [命令]* 来查看各个命令的使用文档，如：man cp。

   3. ls（列出目录）

      > 在Linux系统当中，ls命令可能是最常被运行的。
      >
      > 语法：
      >
      > ```bash
      > [root@www ~]# ls [-aAdfFhilnrRSt] 目录名称
      > ```
      >
      > 选项与参数：
      >
      > * -a ：全部的文件，连同隐藏文件（开头为 . 的文件）一起列出来（常用）
      > * -l ：长数据串列出，包含文件的属性与权限等等数据，但无法列出吟唱数据；（常用）
      >
      > 所有的Linux命令可以组合使用，例如：将目录下的所有文件列出来（含属性与隐藏档）
      >
      > ```bash
      > [root@www ~]# ls -al ~
      > ```
      >
      > ![1616578529037](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616578529037.png)

   4. cd （切换目录）

      > cd是Change Directory的缩写，这是用来变换工作目录的命令。
      >
      > 
      >
      > 语法：
      >
      > ```bash
      > cd [相对路径或绝对路径]
      > ```
      >
      > 
      >
      > 测试：
      >
      > ```bash
      > # 切换到用户目录下
      > [root@kuangshen /]# cd home  
      > 
      > # 使用 mkdir 命令创建 kuangstudy 目录
      > [root@kuangshen home]# mkdir kuangstudy
      > 
      > # 进入 kuangstudy 目录
      > [root@kuangshen home]# cd kuangstudy
      > 
      > # 回到上一级
      > [root@kuangshen kuangstudy]# cd ..
      > 
      > # 回到根目录
      > [root@kuangshen kuangstudy]# cd /
      > 
      > # 表示回到自己的家目录，亦即是 /root 这个目录
      > [root@kuangshen kuangstudy]# cd ~
      > ```
      >
      > ![1616579099903](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616579099903.png)

   5. pwd（显示目前所在的目录）

      > pwd是Print Working Directory 的缩写，也就是显示目前所辖目录的命令。
      >
      > ```bash
      > [root@kuangshen kuangstudy]#pwd [-P]
      > ```
      >
      > 选项与参数：-P：显示出确实的路径，而并非连接（link）路径
      >
      > 测试：
      >
      > ```bash
      > # 单纯显示出目前的工作目录
      > [root@kuangshen ~]# pwd
      > /root
      > 
      > # 如果是链接，要显示真实地址，可以使用 -P参数
      > [root@kuangshen /]# cd bin
      > [root@kuangshen bin]# pwd -P
      > /usr/bin
      > ```
      >
      > ![1616580005616](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616580005616.png)
      >
      > 注意：区分大小写

   6. mkdir （创建新目录）

      > 如果想要创建新的目录的话，那么就是用mkdir （make directory）
      >
      > ```bash
      > mkdir [-mp] 目录名称
      > ```
      >
      > 选项与参数：
      >
      > * -m：配置文件的权限，直接配置，不需要看默认权限（umask）的脸色
      > * -p：帮助你直接将所需要的目录（包含上一级目录）递归创建起来！
      >
      > 测试：
      >
      > ```bash
      > # 进入我们用户目录下
      > [root@kuangshen /]# cd /home
      > 
      > # 创建一个 test 文件夹
      > [root@kuangshen home]# mkdir test
      > 
      > # 创建多层级目录
      > [root@kuangshen home]# mkdir test1/test2/test3/test4
      > mkdir: cannot create directory ‘test1/test2/test3/test4’:
      > No such file or directory  # <== 没办法直接创建此目录啊！
      > 
      > # 加了这个 -p 的选项，可以自行帮你创建多层目录！
      > [root@kuangshen home]# mkdir -p test1/test2/test3/test4
      > 
      > # 创建权限为 rwx--x--x 的目录。
      > [root@kuangshen home]# mkdir -m 711 test2
      > [root@kuangshen home]# ls -l
      > drwxr-xr-x 2 root root  4096 Mar 12 21:55 test
      > drwxr-xr-x 3 root root  4096 Mar 12 21:56 test1
      > drwx--x--x 2 root root  4096 Mar 12 21:58 test2
      > ```
      >
      > ![1616580540861](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616580540861.png)

   7. rmdir （删除空的目录）

      > 语法：
      >
      > ```bash
      > rmdir [-p] 目录名称
      > ```
      >
      > 选项与参数：-p ：连同上一级【空的】目录也一起删除
      >
      > 
      >
      > 测试：
      >
      > ```bash
      > # 看看有多少目录存在？
      > [root@kuangshen home]# ls -l
      > drwxr-xr-x 2 root root  4096 Mar 12 21:55 test
      > drwxr-xr-x 3 root root  4096 Mar 12 21:56 test1
      > drwx--x--x 2 root root  4096 Mar 12 21:58 test2
      > 
      > # 可直接删除掉，没问题
      > [root@kuangshen home]# rmdir test
      > 
      > # 因为尚有内容，所以无法删除！
      > [root@kuangshen home]# rmdir test1
      > rmdir: failed to remove ‘test1’: Directory not empty
      > 
      > # 利用 -p 这个选项，立刻就可以将 test1/test2/test3/test4 依次删除。
      > [root@kuangshen home]# rmdir -p test1/test2/test3/test4
      > ```
      >
      > ![1616580881619](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616580881619.png)
      >
      > 注意：这个rmdir仅能删除空的目录，你可以使用rm命令来删除非空目录，后面有提及。

   8. cp（复制文件或目录）

      > 语法：
      >
      > ```bash
      > [root@www ~]# cp [-adfilprsu] 来源档(source) 目标档(destination)
      > [root@www ~]# cp [options] source1 source2 source3 .... directory
      > ```
      >
      > 选项与参数：
      >
      > * **-a：**相当于 -pdr的意思；（常用）
      > * **-p：**连同文件的属性一起复制过去，而非使用默认属性（备份常用）
      > * **-d：**若来源档为连结档的属性（link file），则复制连结档属性而非文件本身；
      > * **-r：**递归持续复制，用于目录的复制行为；（常用）
      > *  **-f：**为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次； 
      > *  **-i：**若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用) 
      > *  **-l：**进行硬式连结(hard link)的连结档创建，而非复制文件本身。 
      > *  **-s：**复制成为符号连结档 (symbolic link)，亦即『捷径』文件； 
      > *  **-u：**若 destination 比 source 旧才升级 destination ！ 
      >
      > 测试：
      >
      > ```bash
      > # 找一个有文件的目录，我这里找到 root目录
      > [root@kuangshen home]# cd /root
      > [root@kuangshen ~]# ls
      > install.sh
      > [root@kuangshen ~]# cd /home
      > 
      > # 复制 root目录下的install.sh 到 home目录下
      > [root@kuangshen home]# cp /root/install.sh /home
      > [root@kuangshen home]# ls
      > install.sh
      > 
      > # 再次复制，加上-i参数，增加覆盖询问？
      > [root@kuangshen home]# cp -i /root/install.sh /home
      > cp: overwrite ‘/home/install.sh’? y # n不覆盖，y为覆盖
      > ```

   9. rm（移除文件或目录）

      > 语法：
      >
      > ```bash
      > rm [-fir] 文件或目录
      > ```
      >
      > 选项与参数：
      >
      > * -f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息； 
      > *  -i ：互动模式，在删除前会询问使用者是否动作 
      > *  -r ：递归删除啊！最常用在目录的删除了！这是非常危险的选项！！！ 
      >
      > 测试：
      >
      > ```bash
      > # 将刚刚在 cp 的实例中创建的 install.sh删除掉！
      > [root@kuangshen home]# rm -i install.sh
      > rm: remove regular file ‘install.sh’? y
      > # 如果加上 -i 的选项就会主动询问喔，避免你删除到错误的档名！
      > 
      > # 尽量不要在服务器上使用 rm -rf /      删库跑路
      > ```

   10. mv（移动文件与目录，或修改名称）

       > 语法：
       >
       > ```bash
       > [root@www ~]# mv [-fiu] source destination
       > [root@www ~]# mv [options] source1 source2 source3 .... directory
       > ```
       >
       > 选项与参数：
       >
       > *  -f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖； 
       > *  -i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！ 
       > *  -u ：若目标文件已经存在，且 source 比较新，才会升级 (update) 
       >
       > 测试：
       >
       > ```bash
       > # 复制一个文件到当前目录
       > [root@kuangshen home]# cp /root/install.sh /home
       > 
       > # 创建一个文件夹 test
       > [root@kuangshen home]# mkdir test
       > 
       > # 将复制过来的文件移动到我们创建的目录，并查看
       > [root@kuangshen home]# mv install.sh test
       > [root@kuangshen home]# ls
       > test
       > [root@kuangshen home]# cd test
       > [root@kuangshen test]# ls
       > install.sh
       > 
       > # 将文件夹重命名，然后再次查看！
       > [root@kuangshen test]# cd ..
       > [root@kuangshen home]# mv test mvtest
       > [root@kuangshen home]# ls
       > mvtest
       > ```

## 基本属性

1. 看懂文件属性

   > Linux系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。为了保护系统的安全性，Linux系统对不同的用户访问投以文件（包括目录文件）的权限做了不同的规定。
   >
   > 在Linux中我们可以使用ll或者ls-ll命令显示一个文件的属性以及文件所属的用户和组，如：
   >
   > ![1616586356067](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616586356067.png)
   >
   > 实例中，boot文件的第一个属性“d”表示。“d”在Linux中代表该文件是一个目录文件。
   >
   > 在Linux中第一个字符代表这个文件是目录、文件或链接文件等等：
   >
   > *  当为[ **d** ]则是目录 
   > *  当为[ **-** ]则是文件 
   > *  若是[ **l** ]则表示为链接文档 ( link file )
   > *  若是[ **b** ]则表示为装置文件里面的可供储存的接口设备 ( 可随机存取装置 ) 
   > *  若是[ **c** ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标 ( 一次性读取装置 ) 
   >
   > 接下来的字符中，以三个为一组，且均为 『rwx』 的三个参数的组合。
   >
   > 其中，[ r ]代表可读(read)、[ w ]代表可写(write)、[ x ]代表可执行(execute)。
   >
   > 要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号[ - ]。
   >
   > 每个文件的属性由左边第一部分的10个字符来确定（如下图）：
   >
   > ![1616587080343](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616587080343.png)
   >
   > 从左至右用0-9这些数字来表示。
   >
   > 第0位确定文件类型，第1-3位确定属主（该文件的所有者）拥有该文件的权限。第4-6位确定属组（所有者的同组用户）拥有该文件的权限，第7-9位确定其他用户拥有该文件的权限。
   >
   > 其中：
   >
   >  第1、4、7位表示读权限，如果用"r"字符表示，则有读权限，如果用"-"字符表示，则没有读权限； 
   >
   >  第2、5、8位表示写权限，如果用"w"字符表示，则有写权限，如果用"-"字符表示没有写权限； 
   >
   >  第3、6、9位表示可执行权限，如果用"x"字符表示，则有执行权限，如果用"-"字符表示，则没有执行权限。 
   >
   >  对于文件来说，它都有一个特定的所有者，也就是对该文件具有所有权的用户。 
   >
   >  同时，在Linux系统中，用户是按组分类的，一个用户属于一个或多个组。 
   >
   >  文件所有者以外的用户又可以分为文件所有者的同组用户和其他用户。 
   >
   >  因此，Linux系统按文件所有者、文件所有者同组用户和其他用户来规定了不同的文件访问权限。 
   >
   >  在以上实例中，boot 文件是一个目录文件，属主和属组都为 root。 

2. 修改文件属性

   1.  **chgrp：更改文件属组** 

      > ```bash
      > chgrp [-R] 属组名 文件名
      > ```
      >
      > -R：递归更改文件属性，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属组都会更改。

   2.  **chown：更改文件属主，也可以同时更改文件属组** 

      > ```bash
      > chown [–R] 属主名 文件名
      > chown [-R] 属主名：属组名 文件名
      > ```

   3.  **chmod：更改文件9个属性（必须掌握）**

      >  ```bash
      > chmod [-R] xyz 文件或目录
      >  ```
      >
      >  Linux文件属性有两种设置方法，一种是数字（常用的是数字），一种是符号。 
      >
      >  Linux文件的基本权限就有九个，分别是owner/group/others三种身份各有自己的  read/write/execute权限。 
      >
      >  先复习一下刚刚上面提到的数据：文件的权限字符为：『-rwxrwxrwx』， 这九个权限是三个三个一组的！其中，我们可以使用数字来代表各个权限，各权限的分数对照表如下： 
      >
      > ```
      > r:4    w:2    x:1
      > ```
      >
      > 每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为： 
      >
      > [rwxrwx---]分数则是：
      >
      > * owner = rwx = 4+2+1 = 7
      > *  group = rwx = 4+2+1 = 7 
      > *  others= --- = 0+0+0 = 0 
      >
      > ```bash
      > chmod 770 filename
      > ```
      >
      > ![1616588259465](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616588259465.png)

## 文件内容查看

1. 概述

> Linux系统中使用以下命令来查看文件的内容：
>
> * cat 由第一行开始显示文件内容，用来读文章或者用来读取配置文件都使用 cat 命令
>
> * tac 从最后一行开始显示，可以看出 tac 是 cat 的倒着写。
>
>   ![1616589392815](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616589392815.png)
>
> * nl 显示的时候，顺道输出行号，看代码的时候，希望显示行号（常用）
>
>   ![1616589616004](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616589616004.png)
>
> * more 一页一页的显示文件内容（空格代表翻页，enter代表向下看一行，:f可以看到当前行号设置）
>
>   ![1616589774826](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616589774826.png)
>
> * less 与 more 类似，但是比more更好的是，他可以往前翻页。（空格翻页，上下键代表翻动页面。退出使用q命令，查找字符串/要查询的字符向下查询，向上查询使用？要查询的字符串，n代表next（继续查询下一个））
>
>   ![1616590345233](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616590345233.png)
>
> * head 只看头几行（通过-n参数来显示几行）
>
> * tail 只看尾巴几行（通过-n参数来显示几行）
>
> 可以使用 *man[命令]* 来查看各个命令的使用文档，如：man cp。
>
> 
>
> 网络配置目录：cd /etc/sysconfig/network-scripts/
>
> ![1616588941718](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616588941718.png)
>
> ```bash
> ifconfig  # 查看网络配置
> ```

2. 拓展：Linux链接概念（了解即可）

   > Linux链接分两种，一种被称为硬链接（Hard Link），另一种被称为符号链接（Symbolic Link  软连接）。
   >
   > ln命令产生硬链接。
   >
   > 
   >
   > **硬连接**
   >
   > 硬链接指通过索引节点来进行连接。在Linux的文件系统中，保存在磁盘分区中的文件不管是什么类型都给它分配一个编号，称为索引节点号（Inode Index）。在Linux中，多个文件名指向同一索引节点是存在的。比如：A 是 B 的硬链接（A 和 B 都是文件名），则 A 的目录项中的 indoe 节点号与 B 的目录项中的 inode 节点号相同，即一个 inode 节点对应两个不同的文件名，两个文件名只想投一个文件，A 和 B对文件系统来说是完全平等的。删除其中任何一个都不会影响另外一个的访问。
   >
   > 硬连接的作用是允许一个文件拥有多个有效路径名，这样用户就可以建立硬链接到重要文件，以防止“误删”的功能。其原因如上所述，因为对应该目录的索引节点有一个以上的连接。只删除一个连接并不影响索引节点本身和其它的连接，只有当最后一个连接被删除后，文件的数据块及目录的连接才会被释放。也就是说，文件真正删除的条件是与之相关的所有硬链接文件均被删除。
   >
   > 
   >
   > **软连接**
   >
   > 另外一种连接称之为符号链接（Symbolic Link），也叫软连接。软链接文件有类似于Windows的快捷方式。它实际上是一个特殊的文件。在符号连接中，文件实际上是一个文本文件，其中包含的由另一文件的位置信息。比如：A 是 B 的软连接（A 和 B 都是文件名），A 的目录项中的 inode 节点号与 B 的目录项中的 inode 节点号不相同，A 和 B 指向的是两个不同的 inode ，继而指向两块不同的数据块。但是 A 的数据块中存放的只是 B 的路径名（可以根据这个找到 B 的目录项）。A 和 B 之间是“主从”关系，如果B被删除了，A仍然存在（因为两个是不同的文件），但指向的是一个无效的连接。
   >
   > 
   >
   > 测试：
   >
   > ```bash
   > [root@kuangshen /]# cd /home
   > [root@kuangshen home]# touch f1 # 创建一个测试文件f1
   > [root@kuangshen home]# ls
   > f1
   > [root@kuangshen home]# ln f1 f2     # 创建f1的一个硬连接文件f2
   > [root@kuangshen home]# ln -s f1 f3   # 创建f1的一个符号连接文件f3
   > [root@kuangshen home]# ls -li       # -i参数显示文件的inode节点信息
   > 397247 -rw-r--r-- 2 root root     0 Mar 13 00:50 f1
   > 397247 -rw-r--r-- 2 root root     0 Mar 13 00:50 f2
   > 397248 lrwxrwxrwx 1 root root     2 Mar 13 00:50 f3 -> f1
   > ```
   >
   > ![1616592655908](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616592655908.png)
   >
   >  从上面的结果中可以看出，硬连接文件 f2 与原文件 f1 的 inode 节点相同，均为 267087，然而符号连接文件的 inode 节点不同。 
   >
   > ```bash
   > # echo 字符串输出 >> f1 输出到 f1文件
   > [root@kuangshen home]# echo "I am f1 file" >>f1
   > [root@kuangshen home]# cat f1
   > I am f1 file
   > [root@kuangshen home]# cat f2
   > I am f1 file
   > [root@kuangshen home]# cat f3
   > I am f1 file
   > [root@kuangshen home]# rm -f f1
   > [root@kuangshen home]# cat f2
   > I am f1 file
   > [root@kuangshen home]# cat f3
   > cat: f3: No such file or directory
   > ```
   >
   > ![1616593122496](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616593122496.png)
   >
   > 通过上面的测试可以看出：当删除原始文件 f1 后，硬连接 f2 不受影响，但是符号连接 f1 文件无效；
   >
   > 依次您可以做一些相关的测试，可以得到以下全部结论：
   >
   > *  删除符号连接f3,对f1,f2无影响； 
   > *  删除硬连接f2，对f1,f3也无影响； 
   > *  删除原文件f1，对硬连接f2没有影响，导致符号连接f3失效； 
   > *  同时删除原文件f1,硬连接f2，整个文件会真正的被删除。 

## Vim编辑器

1. 什么是Vim编辑器

   > vim编译器通过一些插件可以实现和IDE一样的功能！
   >
   > Vim是从vi发展过来的一个文本编辑器。代码补全、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。尤其是Linux中，必须要会使用Vim（查看内容，编辑内容，保存内容）
   >
   > 简单的来说， vi 是老式的字处理器，不过功能已经很齐全了，但是还是有可以进步的地方。
   >
   > vim 则可以说是程序开发者的一项很好用的工具。
   >
   > 所有的 Unix Like 系统都会内建 vi 文书编辑器，其他的文书编辑器则不一定会存在。
   >
   > 连 vim 的官方网站 (http://www.vim.org) 自己也说 vim 是一个程序开发工具而不是文字处理软件。
   >
   > vim 键盘图：
   >
   > ![1616631046020](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616631046020.png)
   
2. 三种使用模式

   > 基本上vi/vim共分为三种模式，分别是命令模式（Command mode），输入模式（Insert mode）和底线命令模式（Last line mode）。这三种模式的作用分别是：
   >
   > **命令模式**
   >
   > 用户刚刚启动 vi/vim，便进入了命令模式。 
   >
   > 此状态下敲击键盘动作会被Vim识别为命令，而非输入字符。比如我们此时按下i，并不会输入一个字符，i被当作了一个命令。 
   >
   > 以下是常用的几个命令： 
   >
   > *  **i** 切换到输入模式，以输入字符。 
   >
   >   ![1616631623036](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616631623036.png)
   >
   > *  **x** 删除当前光标所在处的字符。 
   >
   > *  **:** 切换到底线命令模式，以在最底一行输入命令。 如果是编辑模式，需要先退出编辑模式
   >
   >   ![1616631805761](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616631805761.png)
   >
   >  若想要编辑文本：启动Vim，进入了命令模式，按下i，切换到输入模式。 
   >
   >  命令模式只有一些最基本的命令，因此仍要依靠底线命令模式输入更多命令。 
   >
   > **输入模式**
   >
   >  在命令模式下按下i就进入了输入模式 。
   >
   >  在输入模式中，可以使用以下按键： 
   >
   > *  **字符按键以及Shift组合**，输入字符 
   > *  **ENTER**，回车键，换行 
   > *  **BACK SPACE**，退格键，删除光标前一个字符 
   > *  **DEL**，删除键，删除光标后一个字符 
   > *  **方向键**，在文本中移动光标 
   > *  **HOME**/**END**，移动光标到行首/行尾 
   > *  **Page Up**/**Page Down**，上/下翻页 
   > *  **Insert**，切换光标为输入/替换模式，光标将变成竖线/下划线 
   > *  **ESC**，退出输入模式，切换到命令模式 
   >
   > **底线命令模式**
   >
   >  在命令模式下按下:（英文冒号）就进入了底线命令模式。 
   >
   >  底线命令模式可以输入单个或多个字符的命令，可用的命令非常多。 
   >
   >  在底线命令模式中，基本的命令有（已经省略了冒号）： 
   >
   > *  q 退出程序 
   > *  w 保存文件 
   >
   > 可以使用 wq 配合保存并退出
   >
   >  按ESC键可随时退出底线命令模式。 
   >
   >  简单的说，我们可以将这三个模式想成底下的图标来表示： 
   >
   > ![1616632079841](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616632079841.png)

3. 上手体验一下，在home目录下测试

   > 如果你想要使用vi来建立一个名为kuangstudy.txt的文件时，你可以这样做：
   >
   > ```bash
   > [root@kuangshen home]# vim kuangstudy.txt
   > ```
   >
   > 然后就会进入文件
   >
   > ![1616632356707](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616632356707.png)
   >
   >  ![1616632745640](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616632745640.png)
   >
   > **按下 i 进入输入模式(也称为编辑模式)，开始编辑文字** 
   >
   >  在一般模式之中，只要按下 i, o, a 等字符就可以进入输入模式了！ 
   >
   >  在编辑模式当中，你可以发现在左下角状态栏中会出现 –INSERT- 的字样，那就是可以输入任意字符的提示。 
   >
   >  这个时候，键盘上除了 **Esc** 这个按键之外，其他的按键都可以视作为一般的输入按钮了，所以你可以进行任何的编辑。 
   >
   > ![1616632422970](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616632422970.png)
   >
   >  **按下 ESC 按钮回到一般模式** 
   >
   >  好了，假设我已经按照上面的样式给他编辑完毕了，那么应该要如何退出呢？是的！没错！就是给他按下 **Esc** 这个按钮即可！马上你就会发现画面左下角的 – INSERT – 不见了！ 
   >
   >  在一般模式中按下 **:wq** 储存后离开 vim！ 

4. Vim按键说明

   > 除了上面简易范例的 i, Esc, :wq 之外，其实 vim 还有非常多的按键可以使用。 
   >
   > 
   >
   > **第一部分：一般模式可用的光标移动、复制粘贴、搜索替换等** 
   >
   > | **移动光标的方法** |                                                              |
   > | ------------------ | ------------------------------------------------------------ |
   > | h 或 向左箭头键(←) | 光标向左移动一个字符                                         |
   > | j 或 向下箭头键(↓) | 光标向下移动一个字符                                         |
   > | k 或 向上箭头键(↑) | 光标向上移动一个字符                                         |
   > | l 或 向右箭头键(→) | 光标向右移动一个字符                                         |
   > | [Ctrl] + [f]       | 屏幕『向下』移动一页，相当于 [Page Down]按键 (常用)          |
   > | [Ctrl] + [b]       | 屏幕『向上』移动一页，相当于 [Page Up] 按键 (常用)           |
   > | [Ctrl] + [d]       | 屏幕『向下』移动半页                                         |
   > | [Ctrl] + [u]       | 屏幕『向上』移动半页                                         |
   > | \+                 | 光标移动到非空格符的下一行                                   |
   > | \-                 | 光标移动到非空格符的上一行                                   |
   > | n< space>          | 那个 n 表示『数字』，例如 20 。按下数字后再按空格键，光标会向右移动这一行的 n 个字符。 |
   > | 0 或功能键[Home]   | 这是数字『 0 』：移动到这一行的最前面字符处 (常用)           |
   > | $ 或功能键[End]    | 移动到这一行的最后面字符处(常用)                             |
   > | H                  | 光标移动到这个屏幕的最上方那一行的第一个字符                 |
   > | M                  | 光标移动到这个屏幕的中央那一行的第一个字符                   |
   > | L                  | 光标移动到这个屏幕的最下方那一行的第一个字符                 |
   > | G                  | 移动到这个档案的最后一行(常用)                               |
   > | nG                 | n 为数字。移动到这个档案的第 n 行。例如 20G 则会移动到这个档案的第 20 行(可配合 :set nu) |
   > | gg                 | 移动到这个档案的第一行，相当于 1G 啊！(常用)                 |
   > | n< Enter>          | n 为数字。光标向下移动 n 行(常用)                            |
   >
   > | **搜索替换** |                                                              |
   > | ------------ | ------------------------------------------------------------ |
   > | /word        | 向光标之下寻找一个名称为 word 的字符串。例如要在档案内搜寻 vbird 这个字符串，就输入 /vbird 即可！(常用) |
   > | ?word        | 向光标之上寻找一个字符串名称为 word 的字符串。               |
   > | n            | 这个 n 是英文按键。代表重复前一个搜寻的动作。举例来说， 如果刚刚我们执行 /vbird 去向下搜寻 vbird 这个字符串，则按下 n 后，会向下继续搜寻下一个名称为 vbird 的字符串。如果是执行 ?vbird 的话，那么按下 n 则会向上继续搜寻名称为 vbird 的字符串！ |
   > | N            | 这个 N 是英文按键。与 n 刚好相反，为『反向』进行前一个搜寻动作。例如 /vbird 后，按下 N 则表示『向上』搜寻 vbird 。 |
   >
   > | **删除、复制与粘贴** |                                                              |
   > | -------------------- | ------------------------------------------------------------ |
   > | x, X                 | 在一行字当中，x 为向后删除一个字符 (相当于 [del] 按键)， X 为向前删除一个字符(相当于 [backspace] 亦即是退格键) (常用) |
   > | nx                   | n 为数字，连续向后删除 n 个字符。举例来说，我要连续删除 10 个字符， 『10x』。 |
   > | dd                   | 删除游标所在的那一整行(常用)                                 |
   > | ndd                  | n 为数字。删除光标所在的向下 n 行，例如 20dd 则是删除 20 行 (常用) |
   > | d1G                  | 删除光标所在到第一行的所有数据                               |
   > | dG                   | 删除光标所在到最后一行的所有数据                             |
   > | d$                   | 删除游标所在处，到该行的最后一个字符                         |
   > | d0                   | 那个是数字的 0 ，删除游标所在处，到该行的最前面一个字符      |
   > | yy                   | 复制游标所在的那一行(常用)                                   |
   > | nyy                  | n 为数字。复制光标所在的向下 n 行，例如 20yy 则是复制 20 行(常用) |
   > | y1G                  | 复制游标所在行到第一行的所有数据                             |
   > | yG                   | 复制游标所在行到最后一行的所有数据                           |
   > | y0                   | 复制光标所在的那个字符到该行行首的所有数据                   |
   > | y$                   | 复制光标所在的那个字符到该行行尾的所有数据                   |
   > | p, P                 | p 为将已复制的数据在光标下一行贴上，P 则为贴在游标上一行！举例来说，我目前光标在第 20 行，且已经复制了 10 行数据。则按下 p 后， 那 10 行数据会贴在原本的 20 行之后，亦即由 21 行开始贴。但如果是按下 P 呢？那么原本的第 20 行会被推到变成 30 行。(常用) |
   > | J                    | 将光标所在行与下一行的数据结合成同一行                       |
   > | c                    | 重复删除多个数据，例如向下删除 10 行，[ 10cj ]               |
   > | u                    | 复原前一个动作。(常用)                                       |
   > | [Ctrl]+r             | 重做上一个动作。(常用)                                       |
   >
   > 
   >
   >  **第二部分：一般模式切换到编辑模式的可用的按钮说明** 
   >
   > | **进入输入或取代的编辑模式** |                                                              |
   > | ---------------------------- | ------------------------------------------------------------ |
   > | i, I                         | 进入输入模式(Insert mode)：i 为『从目前光标所在处输入』， I 为『在目前所在行的第一个非空格符处开始输入』。(常用) |
   > | a, A                         | 进入输入模式(Insert mode)：a 为『从目前光标所在的下一个字符处开始输入』， A 为『从光标所在行的最后一个字符处开始输入』。(常用) |
   > | o, O                         | 进入输入模式(Insert mode)：这是英文字母 o 的大小写。o 为『在目前光标所在的下一行处输入新的一行』；O 为在目前光标所在处的上一行输入新的一行！(常用) |
   > | r, R                         | 进入取代模式(Replace mode)：r 只会取代光标所在的那一个字符一次；R会一直取代光标所在的文字，直到按下 ESC 为止；(常用) |
   > | [Esc]                        | 退出编辑模式，回到一般模式中(常用)                           |
   >
   > 
   >
   >  **第三部分：一般模式切换到指令行模式的可用的按钮说明** 
   >
   > | **指令行的储存、离开等指令**                                 |                                                              |
   > | ------------------------------------------------------------ | ------------------------------------------------------------ |
   > | :w                                                           | 将编辑的数据写入硬盘档案中(常用)                             |
   > | :w!                                                          | 若文件属性为『只读』时，强制写入该档案。不过，到底能不能写入， 还是跟你对该档案的档案权限有关啊！ |
   > | :q                                                           | 离开 vi (常用)                                               |
   > | :q!                                                          | 若曾修改过档案，又不想储存，使用 ! 为强制离开不储存档案。    |
   > | 注意一下啊，那个惊叹号 (!) 在 vi 当中，常常具有『强制』的意思～ |                                                              |
   > | :wq                                                          | 储存后离开，若为 :wq! 则为强制储存后离开 (常用)              |
   > | ZZ                                                           | 这是大写的 Z 喔！若档案没有更动，则不储存离开，若档案已经被更动过，则储存后离开！ |
   > | :w [filename]                                                | 将编辑的数据储存成另一个档案（类似另存新档）                 |
   > | :r [filename]                                                | 在编辑的数据中，读入另一个档案的数据。亦即将 『filename』 这个档案内容加到游标所在行后面 |
   > | :n1,n2 w [filename]                                          | 将 n1 到 n2 的内容储存成 filename 这个档案。                 |
   > | :! command                                                   | 暂时离开 vi 到指令行模式下执行 command 的显示结果！例如 『:! ls /home』即可在 vi 当中看 /home 底下以 ls 输出的档案信息！ |
   > | :set nu （设置行号，代码中经常会使用）                       | 显示行号，设定之后，会在每一行的前缀显示该行的行号           |
   > | :set nonu                                                    | 与 set nu 相反，为取消行号！                                 |

## 账号管理

1. 简介

   > Linux系统是一个多用户，多任务的分时操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。
   >
   > 用户的账号一方面可以帮助系统管理员对使用系统的用户进行跟踪，并控制它们对系统资源的访问；另一方面也可以帮助用户组织文件，并为用户提供安全性保护。
   >
   > 每个用户账号都拥有一个唯一的用户名和各自的口令。
   >
   > 用户在登录时键入正确的用户名和口令后，就能够进入系统和自己的主目录。
   >
   > 实现用户账号的管理，要完成的工作主要有如下几个方面：
   >
   > * 用户账号的添加、删除与修改
   > * 用户口令的管理。
   > * 用户组的管理。

2. 用户账号的管理

   > 用户账号的管理工作主要涉及到用户账号的添加、修改和删除。
   >
   > 添加用户账号就是在系统中创建一个新的账号，然后为账号分配用户号、用户组、主目录和登录Shell等资源。

3. 添加账号 useradd

   > ```bash
   > useradd 选项 用户名
   > ```
   >
   > 参数说明：
   >
   > * 选项：
   >   *  -c comment 指定一段注释性描述 
   >   *  -d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。 
   >   *  -g 用户组 指定用户所属的用户组。 
   >   *  -G 用户组，用户组 指定用户所属的附加组。 
   >   *  -m　使用者目录如不存在则自动建立。 
   >   *  -s Shell文件 指定用户的登录Shell。 
   >   *  -u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。 
   > * 用户名：
   >   *  指定新账号的登录名。 
   >
   > 测试：
   >
   > ```bash
   > # 此命令创建了一个用户kuangshen，其中-m选项用来为登录名kuangshen产生一个主目录 /home/kuangshen
   > [root@kuangshen home]# useradd -m kuangshen
   > ```
   >
   >  增加用户账号就是在/etc/passwd文件中为新用户增加一条记录，同时更新其他系统文件如/etc/shadow, /etc/group等。 
   
4. Linux下如何切换用户

   > 1. 切换用户的命令为： su username 【username是你的用户名哦】 
   > 2.  从普通用户切换到root用户，还可以使用命令：sudo su 
   > 3.  在终端输入exit或logout或使用快捷方式ctrl+d，可以退回到原来用户，其实ctrl+d也是执行的exit命令 
   > 4.  在切换用户时，如果想在切换用户之后使用新用户的工作环境，可以在su和username之间加-，例如：【su - root】 
   >
   >  $表示普通用户 
   >
   >  \#表示超级用户，也就是root用户 
   >
   > ![1616671268469](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616671268469.png)
   >
   > ![1616671442972](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616671442972.png)

5. 删除账号

   >  如果一个用户的账号不再使用，可以从系统中删除。 
   >
   >  删除用户账号就是要将/etc/passwd等系统文件中的该用户记录删除，必要时还删除用户的主目录。
   >
   >  删除一个已有的用户账号使用userdel命令，其格式如下： 
   >
   > ```bash
   > userdel 选项 用户名
   > ```
   >
   >  常用的选项是 **-r**，它的作用是把用户的主目录一起删除。 
   >
   > ```bash
   > [root@kuangshen home]# userdel -r kuangshen
   > ```
   >
   >  此命令删除用户kuangshen在系统文件中（主要是/etc/passwd, /etc/shadow, /etc/group等）的记录，同时删除用户的主目录。 

6. 修改账号

   > 修改用户账号就是根据实际情况更改用户的有关属性，如用户号、主目录、用户组、登录Shell等。 
   >
   > 修改已有用户的信息使用usermod命令，其格式如下： 
   >
   > ```bash
   > usermod 选项 用户名
   > ```
   >
   > 常用的选项包括-c, -d, -m, -g, -G, -s, -u以及-o等，这些选项的意义与useradd命令中的选项一样，可以为用户指定新的资源值。 
   >
   > 例如：
   >
   > ```bash
   > # usermod -s /bin/ksh -d /home/z –g developer kuangshen
   > ```
   >
   >  此命令将用户kuangshen的登录Shell修改为ksh，主目录改为/home/z，用户组改为developer。 

7. 用户口令管理

   > 用户管理的一项重要内容是用户口令的管理。用户账号刚创建时没有口令，但是被系统锁定，无法使用，必须为其指定口令后才可以使用，即使是指定空口令。 
   >
   > 指定和修改用户口令的Shell命令是passwd。超级用户可以为自己和其他用户指定口令，普通用户只能用它修改自己的口令。 
   >
   > 命令的格式为：
   >
   > ```bash
   > passwd 选项 用户名
   > ```
   >
   > 可使用的选项： 
   >
   > *  -l 锁定口令，即禁用账号。 
   > *  -u 口令解锁。 
   > *  -d 使账号无口令。 
   > *  -f 强迫用户下次登录时修改口令。 
   >
   > 如果默认用户名，则修改当前用户的口令。 
   >
   > 例如，假设当前用户是kuangshen，则下面的命令修改该用户自己的口令： 
   >
   > ```bash
   > $ passwd 
   > Old password:******
   > New password:*******
   > Re-enter new password:*******
   > ```
   >
   > 如果是超级用户，可以用下列形式指定任何用户的口令： 
   >
   > ```bash
   > # passwd kuangshen
   > New password:*******
   > Re-enter new password:*******
   > ```
   >
   > 普通用户修改自己的口令时，passwd命令会先询问原口令，验证后再要求用户输入两遍新口令，如果两次输入的口令一致，则将这个口令指定给用户；而超级用户为用户指定口令时，就不需要知道原口令。
   >
   > 为了系统安全起见，用户应该选择比较复杂的口令，例如最好使用8位长的口令，口令中包含有大写、小写字母和数字，并且应该与姓名、生日等不相同。 
   >
   > 为用户指定空口令时，执行下列形式的命令： 
   >
   > ```bash
   > # passwd -d kuangshen
   > ```
   >
   > 此命令将用户 kuangshen的口令删除，这样用户 kuangshen下一次登录时，系统就不再允许该用户登录了。 
   >
   > passwd 命令还可以用 -l(lock) 选项锁定某一用户，使其不能登录，例如： 
   >
   > ```bash
   > # passwd -l kuangshen
   > ```

## 用户组管理

> 每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。不同Linux 系统对用户组的规定有所不同，如Linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。 
>
> 用户组的管理涉及用户组的添加、删除和修改。组的增加、删除和修改实际上就是对/etc/group文件的更新。 

1. 增加一个新的用户组使用groupadd命令

   > ```bash
   > groupadd 选项 用户组
   > ```
   >
   > 可以使用的选项有：
   >
   > -  -g GID 指定新用户组的组标识号（GID）。 
   > -   -o 一般与-g选项同时使用，表示新用户组的GID可以与系统已有用户组的GID相同。 
   >
   > 实例1：
   >
   > ```bash
   > # groupadd group1
   > ```
   >
   > 此命令向系统中增加了一个新组group1，新组的组标识号是在当前已有的最大组标识号的基础上加1。 
   >
   > 实例2：
   >
   > ```bash
   > # groupadd -g 101 group2
   > ```
   >
   > 此命令向系统中增加了一个新组group2，同时指定新组的组标识号是101。 

2. 删除用户组

   > ```bash
   > groupdel 用户组
   > ```
   >
   > 例如：
   >
   > ```bash
   > # groupdel group1
   > ```
   >
   > 此命令从系统中删除组group1。 

3. 修改用户组的属性使用groupmod命令

   > ```bash
   > groupmod 选项 用户组
   > ```
   >
   > 常用的选项有：
   >
   > *  -g GID 为用户组指定新的组标识号。 
   > *  -o 与-g选项同时使用，用户组的新GID可以与系统已有用户组的GID相同。 
   > *  -n新用户组 将用户组的名字改为新名字 
   >
   > ```bash
   > # 此命令将组group2的组标识号修改为102。
   > groupmod -g 102 group2
   > 
   > # 将组group2的标识号改为10000，组名修改为group3。
   > groupmod –g 10000 -n group3 group2
   > ```

4. 切换组

   > 如果一个用户同时属于多个用户组，那么用户可以在用户组之间切换，以便具有其他用户组的权限。
   >
   > 用户可以在登录后，使用命令newgrp切换到其他用户组，这个命令的参数就是目的用户组。 
   >
   > 例如：
   >
   > ```bash
   > $ newgrp root
   > ```
   >
   > 这条命令将当前用户切换到root用户组，前提条件是root用户组确实是该用户的主组或附加组。 

5. /etc/passwd

   > 完成用户管理的工作有许多种方法，但是每一种方法实际上都是对有关的系统文件进行修改。 
   >
   > 与用户和用户组相关的信息都存放在一些系统文件中，这些文件包括/etc/passwd, /etc/shadow, /etc/group等。 
   >
   > 下面分别介绍这些文件的内容。 
   >
   > **/etc/passwd文件是用户管理工作涉及的最重要的一个文件。** 
   >
   > Linux系统中的每个用户都在/etc/passwd文件中有一个对应的记录行，它记录了这个用户的一些基本属性。 
   >
   > 这个文件对所有用户都是可读的。它的内容类似下面的例子： 
   >
   > ```bash
   > ＃ cat /etc/passwd
   > 
   > root:x:0:0:Superuser:/:
   > daemon:x:1:1:System daemons:/etc:
   > bin:x:2:2:Owner of system commands:/bin:
   > sys:x:3:3:Owner of system files:/usr/sys:
   > adm:x:4:4:System accounting:/usr/adm:
   > uucp:x:5:5:UUCP administrator:/usr/lib/uucp:
   > auth:x:7:21:Authentication administrator:/tcb/files/auth:
   > cron:x:9:16:Cron daemon:/usr/spool/cron:
   > listen:x:37:4:Network daemon:/usr/net/nls:
   > lp:x:71:18:Printer administrator:/usr/spool/lp:
   > ```
   >
   > 从上面的例子我们可以看到，/etc/passwd中一行记录对应着一个用户，每行记录又被冒号(:)分隔为7个字段，其格式和具体含义如下： 
   >
   > ```bash
   > 用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell
   > ```
   >
   > 1）"用户名"是代表用户账号的字符串。
   >
   > 通常长度不超过8个字符，并且由大小写字母和/或数字组成。登录名中不能有冒号(:)，因为冒号在这里是分隔符。
   >
   > 为了兼容起见，登录名中最好不要包含点字符(.)，并且不使用连字符(-)和加号(+)打头。
   >
   > 2）“口令”一些系统中，存放着加密后的用户口令字。
   >
   > 虽然这个字段存放的只是用户口令的加密串，不是明文，但是由于/etc/passwd文件对所有用户都可读，所以这仍是一个安全隐患。因此，现在许多Linux 系统（如SVR4）都使用了shadow技术，把真正的加密后的用户口令字存放到/etc/shadow文件中，而在/etc/passwd文件的口令字段中只存放一个特殊的字符，例如“x”或者“*”。
   >
   > 3）“用户标识号”是一个整数，系统内部用它来标识用户。
   >
   > 一般情况下它与用户名是一一对应的。如果几个用户名对应的用户标识号是一样的，系统内部将把它们视为同一个用户，但是它们可以有不同的口令、不同的主目录以及不同的登录Shell等。
   >
   > 通常用户标识号的取值范围是0～65 535。0是超级用户root的标识号，1～99由系统保留，作为管理账号，普通用户的标识号从100开始。在Linux系统中，这个界限是500。
   >
   > 4）“组标识号”字段记录的是用户所属的用户组。
   >
   > 它对应着/etc/group文件中的一条记录。
   >
   > 5)“注释性描述”字段记录着用户的一些个人情况。
   >
   > 例如用户的真实姓名、电话、地址等，这个字段并没有什么实际的用途。在不同的Linux 系统中，这个字段的格式并没有统一。在许多Linux系统中，这个字段存放的是一段任意的注释性描述文字，用作finger命令的输出。
   >
   > 6)“主目录”，也就是用户的起始工作目录。
   >
   > 它是用户在登录到系统之后所处的目录。在大多数系统中，各用户的主目录都被组织在同一个特定的目录下，而用户主目录的名称就是该用户的登录名。各用户对自己的主目录有读、写、执行（搜索）权限，其他用户对此目录的访问权限则根据具体情况设置。
   >
   > 7)用户登录后，要启动一个进程，负责将用户的操作传给内核，这个进程是用户登录到系统后运行的命令解释器或某个特定的程序，即Shell。
   >
   > Shell是用户与Linux系统之间的接口。Linux的Shell有许多种，每种都有不同的特点。常用的有sh(Bourne Shell), csh(C Shell), ksh(Korn Shell), tcsh(TENEX/TOPS-20 type C Shell), bash(Bourne Again Shell)等。
   >
   > 系统管理员可以根据系统情况和用户习惯为用户指定某个Shell。如果不指定Shell，那么系统使用sh为默认的登录Shell，即这个字段的值为/bin/sh。
   >
   > 用户的登录Shell也可以指定为某个特定的程序（此程序不是一个命令解释器）。
   >
   > 利用这一特点，我们可以限制用户只能运行指定的应用程序，在该应用程序运行结束后，用户就自动退出了系统。有些Linux 系统要求只有那些在系统中登记了的程序才能出现在这个字段中。
   >
   > 8)系统中有一类用户称为伪用户（pseudo users）。
   >
   > 这些用户在/etc/passwd文件中也占有一条记录，但是不能登录，因为它们的登录Shell为空。它们的存在主要是方便系统管理，满足相应的系统进程对文件属主的要求。

## 磁盘管理

1. 概述

   > Linux磁盘管理好坏直接关系到整个系统的性能问题。 
   >
   > Linux磁盘管理常用命令为 df、du。 
   >
   > * df ：列出文件系统的整体磁盘使用量 
   > * du：检查磁盘空间使用量 
   >
   > ![1616675476644](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616675476644.png)

## 进程管理

对于我们开发人员来说，其实Linux更偏向于使用即可！

1. 什么是进程

   > 1. 在Linux中，每一个程序都是有自己的一个进程，每一个进程都有一个id号！
   > 2. 每一个进程都会有一个父进程！
   > 3. 进程可以有两种存在方式：前台！后台运行！
   > 4. 一般的话服务都是后台运行的，基本得程序都是前台运行的！

2. 命令

   > ps  查看当前系统中正在执行的各种进程的信息！
   >
   > ps -xx：
   >
   > * -a 显示当前终端运行的所有的进程信息（当前的进程）
   > * -u 以用户的信息显示进程
   > * -x显示后台运行进程的参数！
   >
   > ```bash
   > # ps -aux|查看所有的进程
   > ps -aux|grep mysql
   > 
   > # | 在Linux这个叫做管道符    A|B
   > # grep 查找文件中符合条件的字符串！
   > ```
   >
   > 对于我们来说，这里目前只需要记住一个命令即可 ps -xx|grep 进程名字          ！ 过滤进程信息！
   >
   > **ps -ef：可以查看到父进程的信息**
   >
   > ```bash
   > ps -ef|grep mysql  # 看父进程我们一般可以通过目录树结构来查看！
   > 
   > 
   > # 进程树
   > pstree
   > 	-p 显示父id
   > 	-u 显示用户组
   > ```
   >
   > ![1616677128278](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616677128278.png)
   >
   > 结束进程：杀死进程，等价于windows结束任务！
   >
   > kill -9 进程的id
   >
   > 但是，我们平常写的一个java代码死循环了，可以选择结束进程！杀进程
   >
   > ```bash
   > kill -9 进程的id
   > ```
   >
   > 表示强制结束该进程。

## 环境安装

**jdk安装（rmp安装）**

1.  rpm下载地址http://www.oracle.com/technetwork/java/javase/downloads/index.html 

2. 安装java环境

   如果存在则卸载

   ```bash
   [root@kuangshen ~]# java -version
   java version "1.8.0_121"
   Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
   Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)
   # 检查
   [root@kuangshen ~]# rpm -qa|grep jdk
   jdk1.8.0_121-1.8.0_121-fcs.x86_64
   # 卸载 -e --nodeps 强制删除
   [root@kuangshen ~]# rpm -e --nodeps jdk1.8.0_121-1.8.0_121-fcs.x86_64
   [root@kuangshen ~]# java -version
   -bash: /usr/bin/java: No such file or directory  # OK
   ```

   没有就安装

   ![1616678893972](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616678893972.png)

   ```bash
   # 检测当前系统是否存在java环境！  java -version
   # 如果有的话就直接卸载
   # rmp -qa|grep jdk   # 检测版本信息
   # rmp -e --nodeps jdk_
   
   # 卸载完毕后即可安装jdk
   # rpm -ivh rpm包
   
   # 配置环境变量！cd /etc/profile
   
   ```

## Tomcat安装（解压缩安装）

1.  安装好了Java环境后我们可以测试下Tomcat！准备好Tomcat的安装包！ 

2.  将文件移动到/usr/tomcat/下，并解压！ 

   ```bash
   [root@kuangshen kuangshen]# mv apache-tomcat-9.0.22.tar.gz /usr
   [root@kuangshen kuangshen]# cd /usr
   [root@kuangshen usr]# ls
   apache-tomcat-9.0.22.tar.gz
   [root@kuangshen usr]# tar -zxvf apache-tomcat-9.0.22.tar.gz   # 解压
   ```

   **![1616728417537](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616728417537.png)**

3. 运行Tomcat，进入bin目录，和我们以前在Windows下看的都是一样的 

   ```bash
   # 执行：startup.sh -->启动tomcat
   # 执行：shutdown.sh -->关闭tomcat
   ./startup.sh
   ./shutdown.sh
   ```

   ![1616728600324](C:\Users\28943\AppData\Roaming\Typora\typora-user-images\1616728600324.png)

   如果防火墙8080端口开了，并且阿里云安全组也开放了这个时候就可以访问远程了！

4. 确保Linux的防火墙端口是开启的，如果是阿里云，需要保证阿里云的安全组策略是开放的！ 

   ```bash
   # 查看firewall服务状态
   systemctl status firewalld
   
   # 开启、重启、关闭、firewalld.service服务
   # 开启
   service firewalld start
   # 重启
   service firewalld restart
   # 关闭
   service firewalld stop
   
   # 查看防火墙规则
   firewall-cmd --list-all    # 查看全部信息
   firewall-cmd --list-ports  # 只看端口信息
   
   # 开启端口
   开端口命令：firewall-cmd --zone=public --add-port=8080/tcp --permanent
   重启防火墙：systemctl restart firewalld.service
   
   命令含义：
   --zone #作用域
   --add-port=80/tcp  #添加端口，格式为：端口/通讯协议
   --permanent   #永久生效，没有此参数重启后失效
   ```

   