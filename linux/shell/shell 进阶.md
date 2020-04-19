#### 正则表达式和文本搜索、文本替换

- 元字符

  - . * [] ^ $ \

- 扩展元字符：+ ？｜

- 文件的查找命令 find

  ```shell
  find *.mp5 -exec rm {} \;
  find xxx -exec command ; # -exec执行不用提示
  find xxx -ok command ; # -ok每次执行command 都要提示用户
  -exec 表示执行xxx命令
  {}表示把查到的文件发送到这里来
  \; 转义后的; 表示结束
  find /etc -type f -regex .*wd
  
  # 显示文件的状态信息
  stat filea
  LANG=C stat filea
  ```

  

- 文本内容过滤（查找）grep

  ```shell
  grep pass /root/anaconda-ks.cfg | cut -d " " -f 1/2/3...
  cut -d ":" -f7 /etc/passwd | sort | uniq -c
  ```



#### sed 和 awk：行编辑器

- sed 的基本工作方式

  - 将文件以行为单位读取到内存（模式空间）
  - 使用 sed 的每个脚本对该文件进行操作
  - 处理完成后输出该行

- sed 的替换命令 s：

  - sed 's/old/new/' filename

  - sed -e 's/old/new' -e 's/old/new/' filename

  - sed -i 's/old/new' 's/old/new/' filename

  - sed 's/正则/new/' filename: . * ^ $ [] \

  - sed -r 's/扩展正则/new/' filename：+ ? |

    ```shell
    sed 's!/!aa!' afile	
    head -5 /etc/passwd | sed 's/s*bin//'
    grep root /etc/passwd | sed 's/^root//'
    sed -r 's/ab+/!/' bfile
    # \1 代表匹配的模式块 打印两次，
    sed -r 's/(a.*b)/\1:\1/' cfile
    sed -r 's/(aa)|(bb)/!/' bfile
    ```

- sed 的替换命令加强版

  - 全局替换

    - s/old/new/g

  - 标志位

    - s/old/new/标志位

  - 寻址

    - /正则/s/old/new/g
    - 行号s/old/new/g

  - 分组

    - /regular/{s/old/new/; s/old/new/}

  - sed 脚本文件

    - sed -f sedscript filename

    ```shell
    head -5 /etc/passwd | sed 's/root/!!!/2'
    head -5 /etc/passwd | sed 's/root/!!!/g'
    # 打印匹配上的行
    head -5 /etc/passwd | sed 's/root/!!!/p'
    head -5 /etc/passwd | sed -n 's/root/!!!/p'
    # 匹配上的行写入到文件
    head -5 /etc/passwd | sed -n 's/root/!!!/w /tmp/tmp.txt' 
    # 替换以bin 开头的行到末尾行
    head -6 /etc/passwd | sed '/^bin/,$s/adm/!!/g'
    ```

- sed 的其他命令

  - 删除、追加、插入、更改、打印、下一行 - n、读写文件、退出命令

    ```shell
    # 匹配上删除后，不会执行后面的命令
    sed '/ab/d; s/a/!/' bfile
    # = 打印行号
    sed '/ab/d; =' bfile
    # i上一行插入
    sed '/ab/i hello' bfile
    # a下一行插入
    sed '/ab/a hello' bfile
    # c匹配行直接改写为hello
    sed '/ab/c hello' bfile
    # r file，读取文件所有行内容到匹配行后面：多文件合并
    sed '/ab/r afile' bfile > cfile
    # w file
    sed '/ab/w afile' bfile
    # 匹配行打印
    sed -n '/ab/p' bfile
    
    seq 1 1000000 > line.txt
    # 读取1-10后打印，不退出，还会把后面的读入内存，效率低
    time sed -n '1,10p' line.txt 
    # 退出命令,读取10行就退出
    time sed -n '10q' line.txt
    ```

- sed 多行模式

  - N 将下一行读入模式空间、P 打印模式空间的第一个到最后一个字符、D 删除模式空间第一个到最后一个字符

    ```shell
    # 两行一组处理数据，12 23 34，P，D执行后，不会退出，重新回头 N执行
    sed 'N;s/\n//;s/hello bash/hello sed\n/;P;D' b.txt
    # 123 234 345
    sed 'N;N;s/\n//;s/hello bash/hello sed\n/;P;D' b.txt
    ```

- sed 的保持空间

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200417175149532.png" alt="image-20200417175149532" style="zoom:30%;" align=left />

  - h - 复制到保持空间，覆盖保持空间的内容，H-追加
  - g - 取出到模式空间，覆盖模式空间的内容，G-追加
  - x 交换保持空间和模式空间

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200417175346121.png" alt="image-20200417175346121" style="zoom:33%;" align=left />

  ```shell
  # 倒叙打印：6 5 4 3 2 1
  # 1!G,第一行不取保持空间的内容，$p读取最后一行的时候才打印
  cat -n /etc/passwd | head -6 | sed -n '1!G;h;$p'
  cat -n /etc/passwd | head -6 | sed '1!G;h;$!d'
  cat -n /etc/passwd | head -6 | sed -n '1h;1!G;$!x;$p'
  ```



- AWK字段的引用分离

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200417183936609.png" alt="image-20200417183936609" style="zoom:33%;" align=left />

- AWK 表达式：系统变量

  <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200417185333467.png" alt="image-20200417185333467" style="zoom:33%;" align=left />

  ```shell
  head -5 /etc/passwd | awk -F ":" '{print $1}'
  head -5 /etc/passwd | awk 'BEGIN{FS=":"}{print $1,$2}'
  head -5 /etc/passwd | awk 'BEGIN{FS=":";OFS="-"}{print $1,$2}'
  head -5 /etc/passwd | awk 'print NR,$0'
  head -5 /etc/passwd | awk '{print FNR,$0}'
  awk '{print FNR,$0}' /etc/hosts /etc/hosts
  head -5 /etc/passwd | awk 'BEGIN{FS=":"}{print NF}'
  head -5 /etc/passwd | awk 'BEGIN{FS=":"}{print $NF}'
  
  # if
  awk '{if($2>=80){print $1;print$2}}' afile
  # 循环
  awk '{sum=0; for(c=2;c<=NF;c++) sum+=$c; print sum/(NF-1)' afile
  # 数组
  
  # kip.txt
  user1 70 72 74 76 74 72
  user2 80 82 74 82 80 78
  user3 60 61 62 63 64 65
  user4 90 89 88 87 86 85
  user5 45 60 63 63 61 60
  
  # result.awk 脚本
  {
  sum=0
  for(column=2; column<=NF; column++)
      sum+=$column
  average[$1] = sum/(NF-1)
  print $1,average[$1]
  if(average[$1] >=80)
      letter="S"
  else if (average[$1] >= 70)
      letter="A"
  else if (average[$1] >=60)
      letter="B"
  else
      letter="C"
  print $1,average[$1],letter
  letter_all[letter]++
  }
  END{
  for(user in average)
      sum_all += average[user]
  
  avg_all = sum_all/NR
  print "average all:",avg_all
  
  for(user in average)
      if(average[user] > avg_all)
          above++
      else
          below++
  print "above", above
  print "below", below
  print "S", letter_all["S"]
  print "A", letter_all["A"]
  print "B", letter_all["B"]
  print "C", letter_all["C"]
  }
   
  awk  -f result.awk kpi.txt
  ```

- AWK 函数

  ```shell
  # 算数函数：sin cos int rand srand
  awk 'BEGIN{pi=3.14; print int(pi)}'
  awk 'BEGIN{srand(); print rand()*100}'
  # 字符串函数：man awk：sub，split...
  
  # 自定义函数
  function a(str) {
  	return str str
  }
  BEGIN{
  print a("hello awk ")
  }
  ```

  



