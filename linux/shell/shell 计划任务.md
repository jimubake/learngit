- 一次性计划任务 at

  ```shell
  > at 18:32
  at> echo hello > /tmp/hello.txt
  ctrl + d 创建任务
  如果创建失败，使用 systemctl start  atd.service
  ```

- 周期性计划任务

  - cron

    - 配置方式
      - crontab -e
    - 查看现有的计划任务
      - crontab -l
    - 配置格式
      - 分钟 小时 日期 月份 星期 执行的命令

    ```shell
    crontab -e
    => * * * * * /usr/bin/date >> /tmp/date.txt
    /var/log/cron
    tail -f cron
    /var/spool/cron
    
    * * * * 1,5: 周一或者周五
    * * 7 7 1-5: 七月七日，如果这一天时周一到周五的一天，执行
    30 3 * * 1: 工作常用
    ```

- 计划任务加锁 flock

  - 如果计算机不能按时执行任务

    - anacontab 延时计划任务
    - flock 锁文件

    ```shell
    /etc/cron.d/0hourly
    /etc/anacrontab
    /etc/cron.daily/logrotate
    
    ```

    <img src="/Users/zl/Library/Application Support/typora-user-images/image-20200416185824732.png" alt="image-20200416185824732" style="zoom:33%;" align=left />