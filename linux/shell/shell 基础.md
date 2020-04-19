- 管道和重定向

  - 管道与管道符

    - 进程间通信的方式之一，和信号一样，如 kill -9（信号） pid 

    - cat | ps -f：两个命令都会 fork 一个子进程

      <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416112705711.png" alt="image-20200416112705711" style="zoom:40%;" align=left />

  - 子进程与子 shell

    - 管道使用build-in 命令会创建子进程和子shell

  - 重定向符号

    <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416113538522.png" alt="image-20200416113538522" style="zoom:30%;" align=left />

    - wc -l < /etc/passwd

- **变量: 赋值不能有空格，不然shell 会当成命令执行**

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416130821340.png" alt="image-20200416130821340" style="zoom:30%;" align=left />

  - 作用范围：

    - 只在当前shell(终端) 中

      <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416131920316.png" alt="image-20200416131920316" style="zoom:35%;" align=left />

    - ```shell
      > demo_var1="hello bash"  # export var=1
      > vim 4.sh
      	> echo $demo_var1
      > bash 4.sh
      > source 4.sh
      > . 4.sh
      ```

    - export demo_var1：子进程获得父进程的变量

      ```shell
      echo "export PATH=$PATH:/root" >> /etc/profile
      ```

    - unset demo_vae1：删除变量

    - 读取用户输入变量

      ```shell
      > read -p "your name:" first last; echo $first; echo $last
      ```

  - 环境变量：每个shell 打开都可以获取的变量

    - set、env命令

    - $?：运行状态   $$：进程pid   $0：进程名称

    - $PATH

      ```shell
      > vim 5.sh
      > PATH=$PATH:/root # 对子shell 也生效
      > 5.sh
      ```

    - $PS1：终端显示信息

  - 位置变量

    - $1 $2 ... $n

      ```shell
      echo $1
      echo ${2}_: 为空时，显示_
      echo ${2-_}: 为空时，显示_
      ```

  - 环境变量配置文件

    - 编辑配置文件后，重新打开终端后生效，要么 source 配置文件

      <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416134859761.png" alt="image-20200416134859761" style="zoom:25%;" align=left />

    - su - (login)：加载 /etc/profile

      <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416135807511.png" alt="image-20200416135807511" style="zoom:30%;" align=left />

      <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416140109443.png" alt="image-20200416140109443" style="zoom:33%;" align=left />



- 数组：

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416140700481.png" alt="image-20200416140700481" style="zoom:25%;" align=left />

  

- 转义和引用

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416141132584.png" alt="image-20200416141132584" style="zoom:25%;" align=left />

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416141457468.png" alt="image-20200416141457468" style="zoom:25%;" align=left />

  - 引用符号：“双引号-不完全引用  ‘单引号-完全引用  `反引号

    <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416141839434.png" alt="image-20200416141839434" style="zoom:33%;" align=left />

    

- 运算符

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416143454398.png" alt="image-20200416143454398" style="zoom:25%;" align=left />

  ```shell
  expr 4 + 5 # 只支持整数
  a=`expr 4 + 5` # a=$(expr 4 + 5)
  a=(( 4 + 5 ))
  (( a++ ))
  echo $[ 4 * 4 ]
  ```

- 特殊字符

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416150928175.png" alt="image-20200416150928175" style="zoom:33%;" align=left />

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416151748080.png" alt="image-20200416151748080" style="zoom:50%;" align=left />

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416151825136.png" alt="image-20200416151825136" style="zoom:33%;" align=left />

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416152150254.png" alt="image-20200416152150254" style="zoom:33%;" align=left />

  - ifdown etc0; ifup etc0

    

- 测试与判断

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416153031416.png" alt="image-20200416153031416" style="zoom:33%;" align=left />

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416153345750.png" alt="image-20200416153345750" style="zoom:38%;" align=left />

  ```shell
  if [ true ] / exit = 0; then  ;fi
  if []; then
  	echo
  else
  	echo
  fi
  
  case "$1" in
  	"start"|"START")
  	echo $0 start...
  	;;
  	"stop"|"STOP")
  	echo $0 stop...
  	;;
  	"restart")
  	echo $0 restart...
  	;;
  	*)
  	echo "usage: $0 {star|sop|restart}"
  esac
  ```



- 循环

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416160255845.png" alt="image-20200416160255845" style="zoom:33%;" align=left />

  ```shell
  #!/bin/bash
  for filename in `ls *.mp3`
  do
  	mv $filename $(basename $filename .mp3).mp4
  done
  ```

  ```shell
  #!/bin/bash
  # c 语言风格
  for(( i=1; i<=10; i++ ))
  do
  	echo $i
  done
  ```

  ```shell
  #!/bin/bash
  # while 循环
  a=1
  while [ $a -lt 10 ]   # (:/true/[])
  do
  	(( a++ ))
  	echo
  done
  
  while :
  do
  	echo always
  done
  
  # until 循环，为true 退出
  until :
  do
  	echo always
  done
  ```

  ```shell
  # break、continue
  for filename in /etc/profile.d/*.sh
  do
  	if [ -x $filename ]; then
  		. $filename
  	fi
  done
  
  for i in {1..9}
  do
  	if [ $i -eq 5 ]
  		break;
  	fi
  	echo $i
  done
  ```



- 循环处理命令行参数

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416164720524.png" alt="image-20200416164720524" style="zoom:33%;" align=left />

  ```shell
  for pos in $*
  do
  	echo $pos
  done
  ------
  while [ $# -ge 1 ]
  do
  	echo $#
  	# 参数左移一个，参数 -1
  	shift 
  done
  ```

- vim /etc/vimrc

  ```shell
  # 设置 tab 缩进
  :set tabstop=4
  ```

  