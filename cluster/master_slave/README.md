# mysql主从同步架构
方案一：一主一从
主：开启server_id,binlog日志
vim /etc/my.cnf
  server_id=10
  log_bin=master10
  binlog_format="mixed"
systemctl restart mysqld
mysql -uroot -p123456 -e 'show master status'
  master10.000001   154
从：开启server_id
vim /etc/my.cnf
  server_id=11
systemctl restart mysqld
mysql -uroot -p123456 -e "change master to master_host='mster10',master_user='repluser',master_password='123qqq...A',master_log_file='master10.000001',master_pos=154"
mysql -uroot -p123456 -e "start slave;"
mysql -uroot -p123456 -e "show slave status\G" | grep -E 'YES'
IO 和SQL两个YES即可
