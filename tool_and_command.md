# Tool And Command
<h2 align="left">目录</h3>
[toc]

## Windows Shortcuts
```
win + E # 打开此电脑
win + I # 打开设置
win + D # 快速回到桌面
WIN+L   # 锁屏
WIN+S   # 打开收索
WIN+R   # 打开运行
CTRL+ALT+Q # 打开截图工具（反应慢不好用）
Win+Shift+S # 截图快捷键
Win + 左右键  # 分屏
```
## Linux Command
### zip and unzip
1. tar command
```
# 压缩文件 file1 和目录 dir2 到 test.tar.gz
tar -zcvf test.tar.gz file1 dir2
# 解压 test.tar.gz（将 c 换成 x 即可）
tar -zxvf test.tar.gz
# 列出压缩文件的内容
tar -ztvf test.tar.gz
    
## 注释
# -z : 使用 gzip 来压缩和解压文件
# -v : --verbose 详细的列出处理的文件
# -f : --file=ARCHIVE 使用档案文件或设备，这个选项通常是必选的
# -c : --create 创建一个新的归档（压缩包）
# -x : 从压缩包中解出文件
```

```
tar -cf archive.tar foo bar  # Create archive.tar from files foo and bar.
tar -tvf archive.tar         # List all files in archive.tar verbosely.
tar -xf archive.tar          # Extract all files from archive.tar.
```
2. rar command
```
# 压缩文件
rar a -r test.rar file
# 解压文件
unrar x test.rar
	
## 注释
# a : 添加到压缩文件
# -r : 递归处理
# x : 以绝对路径解压文件
```
3. zip command
```
# 压缩文件
zip -r test.zip file
# 解压文件
unzip test.zip
	
## 注释
# -r : 递归处理
```

### 文件和目录操作



## Vim Command



## Git Command
### Push
Git提交是每次提交本地缓存区内容，同时保证远程和本地缓存区内容一致(远程旧内容会被删除)
```bash
git push <远程主机名> <本地分支名>:<远程分支名>

# 将本地的 main 分支推送到 origin 主机的 main 分支
# 如果本地分支名与远程分支名相同，则冒号后面的部分可以省略
git push origin main : main
```
Push代码的一般流程，首先进入main文件夹下：

1. ```git init``` ：在此文件夹生成一个.git隐藏文件；
2. git config user.name  "gitee/gitHub的用户名称":
   ```bash
   git config user.name  "CJuncheng"
   ```
3. git config user.email  "你的邮箱"
   ```bash
   git config user.email  "j.c.chen.cn9@gmail.com"
   ```
4. ``git add .`` : 将文件添加到缓存区( 注意这个"."，是有空格的，"."代表这个文件夹下的目录全部都提交，也可以通过git add 文件名 提交指定的文件)；
5. ``git commit -m "up"``：提交添加到缓存区的文件 
6. ``git branch -M main``
7. ``git remote add origin https://github.com/CJuncheng/CodeCraft2022.git `` ： 添加新的git方式的origin, github上创建好的仓库和本地仓库进行关联 
8. ``git config --global http.sslVerify "false"`` : 连接超时执行
9. ``git push -u origin master`` : 把本地库的所有内容推送到远程仓库（github）上，即上传本地文件

### Fetch, Merge and Pull 
git fetch 用法
```bash
git fetch origin main # 取回origin 主机的 main 分支
```
git merge 用法
```bash
# 开发分支（dev）上的代码达到上线的标准后，要合并到 master 分支
git checkout dev
git pull
git checkout master
git merge dev
git push -u origin master
```
git pull 用法
```bash
git pull <远程主机名> <远程分支名>:<本地分支名>

# 从远程主机 origin 的 main 分支拉取内容，并合并到 main 分支，
# 如果远程分支是与当前分支合并，则冒号后面的部分可以省略
git pull origin main : main 
```
> git pull 则是将远程主机的最新内容拉下来后直接合并，即：git pull = git fetch + git merge，这样可能会产生冲突，需要手动解决。

### 同步操作
```bash
#将远程分支内容同步到本地
git pull origin main 
# 将本地分支通过覆盖方式 push 到远程分支
git push -f origin main
```

### 缓冲区
添加文件到缓存区
```bash
git add 文件名/目录名
git add .
```
查看缓存区内容
```bash
git ls-files
```
删除缓存区
```bash
# 删除已经添加缓存区的某一个文件，不删除物理文件，
git rm --cached +文件名 

# 删除已经添加缓存的某一个目录下所有文件，不删除物理文件
git rm -r --cached 文件夹名
git rm -r --cached . # 清空缓存区（包含所有文件和文件夹）

# 不仅将文件从缓存中删除，还会将物理文件删除，所以使用这个命令要谨慎。
git rm --f +文件名 
git rm -r --f +文件夹名    
```

### 分支操作

```bash
# 查看分支
git branch    # 查看本地分支
git branch -r # 查看远程分支

# 新建分支
git branch (branchname)

# 切换分支
git checkout (branchname)

# 删除分支
git branch -d localBranchName # 删除本地分支
git push origin --delete remoteBranchName # 删除远程分支

# 合并分支
git merge newtest # 将原来分支合并到新分支



```

### 网络代理
只对Github代理（推荐）
```bash
#使用socks5代理（推荐）
git config --global http.https://github.com.proxy socks5://127.0.0.1:7890
#使用http代理（不推荐）
git config --global http.https://github.com.proxy http://127.0.0.1:7890
```
取消代理
```bash
git config --global --unset http.proxy git config --global --unset https.proxy
```






## GCC Command

需要的编译选项
-g (生成调试信息)
-static (静态链接) 此选项将禁止使用动态库，所以，编译出来的东西，一般都很大，也不需要什么动态连接库，就可以运行。


## Makefile

## CMake

Linux CMake 编译流程
```
mkdir build 
cd build
cmake ..
make
```

## GDB Debug
1. 开始调试： gdb a.out

2. GDB 启动三种方式
   - run -- Start debugged program.
开始执行程序，如果没有设置断点，不会停下。 
   - start -- Start the debugged program stopping at the beginning of the main procedure.
   开始执行程序，在main 函数处会停下来
   - starti -- Start the debugged program stopping at the first instruction.
   开始执行程序，在第一条指令处会停下来

1. attach: 用gdb调试一个正在运行中的进程
gdb <program> PID

2. br: 设置断点
br filename:line_num

br namespace::classname::func_name

3. n: 单步跳过   s: 单步进入

4. finish：执行到函数retun返回

5. list: 列出当前位置之后的10行代码；list line_number: 列出line_number之后的十行代码

6. bt（backtrace）：列出调用栈

7. info locals：列出当前函数的局部变量

8. p var_：打印变量值

9. info breakpoints：列出所有断点

10. delete breakpoints：删除所有断点；delete breakpoints id：删除编号为id的断点；disable/enable breakpoints id：禁用/启用断点

11. break ... if ... 条件中断

## Binutils  Tool
二进制文件工具集
### ld
link
### as
assembler
### objdump
### objcopy
### readelf

## MySQL

SSH远程连接MySQL DBSM:
1. 找到 bind-address 修改值为 0.0.0.0(或注释掉)
```shell
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
2. 设置权限
```shell
sudo mysql -uroot -p    #进入MySQL
mysql>use mysql;        #切换数据库
mysql> UPDATE user SET host = '%' WHERE user = 'root'; #允许远程访问
mysql>flush privileges; #刷新权限
mysql>quit;             #退出
```
再看看能否连接，如果仍然报错，可以不用root，自己新建一个用户再连接
3. 新建一个用户
```shell
#创建用户
mysql> CREATE USER 'name'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
#赋权
mysql> GRANT ALL PRIVILEGES ON *.* TO 'name'@'%' WITH GRANT OPTION;
```


## Golang Command

初始化mod
```go
go mod init ProjectName
```

go运行程序和编译程序
```go
go run hello.go 
go build hello.go // generate hello execuctable file
```




