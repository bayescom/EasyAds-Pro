# 运行环境
## 环境说明
本wiki只针对ubuntu(24.04 LTS)进行验证，其他系统请自行测试。
## openresty
- openresty: latest(current is 1.27.1.1)
  参考[官方文档](https://openresty.org/cn/linux-packages.html#ubuntu)
```bash
# 导入官方GPG密钥
sudo apt-get -y install --no-install-recommends wget gnupg ca-certificates lsb-release
wget -O - https://openresty.org/package/pubkey.gpg | sudo gpg --dearmor -o /usr/share/keyrings/openresty.gpg
# 添加APT仓库
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/openresty.gpg] http://openresty.org/package/ubuntu $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/openresty.list > /dev/null
# 安装openresty
sudo apt-get update
sudo apt-get -y install openresty
```
## 依赖
- 应用
```bash
sudo apt-get install luajit
sudo apt-get install liblua5.1-0-dev
# 用于组件安装
sudo apt-get install luarocks
```
- 组件
```bash
luarocks install lua-resty-jit-uuid
luarocks install lua-resty-redis
luarocks install lua-resty-http
luarocks install md5
luarocks install lua-ffi-zlib
luarocks install lua-resty-logger-socket
luarocks install net-url
opm get anjia0532/lua-resty-redis-util
```
# 部署
## 1.配置更新
|配置项|修改说明|
|------|------|
|redis|`conf/conf.lua`文件中 `_M.redis, _M.frequency_redis`，其中媒体广告位配置的地址要与Luna项目中配置的信息一致|
|上报服务域名|`conf/conf.lua`文件中`_M.track.url` 为Stella的服务域名|
| 密钥|`conf/conf.lua`文件中`_M.encryption_keys`，需与SDK侧的密钥一致 |
| 日志存储| `conf/conf.lua中的_M.log_type`。其中 `local`是本地存储模式，将会写文件到本地磁盘；`server`是发送到远端（比如`syslog-ng`），`conf/conf.lua`中`_M.center_syslog_ng`配置|
## 2.应用启停
```bash
# start
sh bin/start.sh
# stop
sh bin/stop.sh
# restart
sh bin/restart.sh 
```
## 3. IP解析(可选)
因为SDK策略管理中使用地域定向，本项目提供`maxmind`的解析方法，用户请根据需求来酌情选择。

- 第三方`maxmind`
```bash
-- 安装maxmind的相关包
apt-get install libmaxminddb0 libmaxminddb-dev
-- 安装openresty依赖组件
luarocks install lua-resty-maxminddb
```
- 自定义IP解析

可修改`lua/processor/ip_region_analyzer.lua`文件替换为自己的IP解析，并根据自己的体系`id`与数据库中`system_code`表中`code_type_id=4`的地域信息进行`id`关系映射实现地域定向功能，映射功能和方法可参考现有使用了`maxmind`的映射关系代码。