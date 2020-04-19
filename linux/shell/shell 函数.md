#### 函数

```shell
function cdls() {
	cd $1
	local var=1
	echo $var
	ls
}
##########

unset cdls

##########
#!/bin/bash

checkpid() {
    local i
    for i in $* ; do
        [ -d "/proc/$i" ] && return 0
    done
    return 1
}

> source x.sh
> checkpid 12
> echo $?
```



- 系统自建了函数库，可以在脚本中引用

  - /etc/init.d/functions

- 自建函数库

  - 使用 source 函数脚本文件“导入”函数

- 脚本控制

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416174754355.png" alt="image-20200416174754355" style="zoom:33%;" align=left />

- 子进程栈溢出：

  ```shell
  # 后台开启两个进程
  : | :&
  
  # 递归开启无数子进程
  func() {
  	func | func&
  }
  # func 替换为 .  root 用户 fork 炸弹
  .(){.|.&};.
  
  # 普通用户有限制，不会造成fork 炸弹；ulimit -a 查看
  ```

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416181526597.png" alt="image-20200416181526597" style="zoom:50%;" align=left />



- 信号

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416181609188.png" alt="image-20200416181609188" style="zoom:38%;" align=left />

  ```shell
  # 捕获15号信号
  trap "echo signal 15" 15
  # 捕获2号信号：ctrl+c, 按了也不会结束程序
  trap "echo signal 2" 2
  echo $$
  while :
  do
  	:
  done
  运行脚本后，另起终端，> kill -15 pid; kill -9 pid
  ```

  

