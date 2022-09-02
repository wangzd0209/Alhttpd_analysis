1. **Althttpd**为每个连接提供了一个单独的proc  -->fork
2. Althttpd 接听80/443端口
2. 没有配置文件，通过很久简单的命令行配置  --> config



1. 开启命令行 -> `althttpd -logfile logfile -root /home/www -user nobody`

2. 在远程ssh连接的服务器使用`-popup`变成一个独立的服务接听8080-8100
3. `-port` 的参数可以是 `80` / `80..81`