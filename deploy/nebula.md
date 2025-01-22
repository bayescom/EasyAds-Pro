# Nebula
EasyAds Pro 数据报表任务 & Report API三方数据拉取任务

## 依赖
* python 3.8.10及以上
* pandas 1.1.5
* PyMySQL 1.1.0
  <br><br><br>
## 部署
任务输入为聚合SDK请求、上报日志文件，任务结果输出到MySQL数据库。
<br><br>
### 您需要：
### 1. 给任务提供聚合SDK日志输入
见小时运行脚本 `/nebula/crontab/hour_report.sh`第161行，**在小时脚本运行时，您需要确保两种聚合SDK日志文件分别存在于两个指定的路径。**

将整个小时的请求日志文件（action为req）放入`temp_nebula_pv_log_path`，其他上报日志文件（action为loaded、succeed、win、click）放入`temp_nebula_deal_log_path`。 推荐您编写一个自动拉取并分割日志的脚本，并在小时运行脚本`hour_report.sh`第161行处调用。

* 请务必将原始日志按行分割成小文件后再放入路径，本任务启用了python的多线程处理，分割为小文件可以显著加快运行速度。

* 请确保聚合SDK日志文件的临时路径包含对应的时间，例如：`/您的目录/20250101_00/pv`和`/您的目录/20250101_00/deal`，以确保多个小时的任务同时运行时临时文件不会混淆。
  <br><br>
### 2. 给任务提供MySQL数据库输出
见数据库更新python脚本 `/nebula/utils/db_utils.py`第50行，**脚本运行完成后，您的结果将写出到给定的MySQL数据库中。**

MySQL数据库配置位于 `/nebula/db.config`，请根据您的实际配置填写。

本任务涉及到三个MySQL数据表：report_hourly（小时报表）、report_daily（天报表）、sdk_report_api_params（report api任务上游渠道账号表）。项目提供了建表的示例语句，位于 `/nebula/create_table_template`，您可以直接使用或者按需求修改。

* 如果您需要修改数据库更新相关代码，请修改报表更新逻辑脚本 `/nebula/script/join_and_update_db.py` 和数据库更新脚本 `/nebula/utils/db_utils.py`。
  <br><br>
### 3. 给数据库提供Report API账号（可选）
如果您有上游渠道的Report API账号，您可以将账号信息插入到sdk_report_api_params表中来为天报表拉取第三方数据。

* 本项目提供了穿山甲、快手、优量汇的Report API模版。如果需要拉取其他的上游渠道三方数据，请参照 `/nebula/report_api`下已有的脚本开发。

* 请确保您的账号开通了对应上游渠道的Report API功能。各个上游渠道的操作各不相同，请参考各个上游渠道的官方文档。
  <br><br><br>
## 运行
### 1. 运行小时报表任务
`cd /您的目录/nebula/crontab && bash hour_report.sh [-t yyyymmdd_HH]`
* 默认运行上一小时的日志任务，可选 -t 选项指定运行小时，格式为yyyymmdd_HH。例如：`bash hour_report.sh -t 20250101_00`
  <br><br>
### 2. 运行天报表任务
`cd /您的目录/nebula/crontab && bash day_report.sh [-t yyyymmdd_HH]`
* 默认运行上一天的日志任务，可选 -t 选项指定运行日期，格式为yyyymmdd。例如：`bash day_report.sh -t 20250101`
  <br><br>
### 3. 运行上游渠道Report API三方数据拉取（可选）
`cd /您的目录/nebula/crontab && bash report_api.sh [yyyy-mm-dd]`
* 默认运行上一天的Report API，可选指定运行日期，格式为yyyy-mm-dd。例如：`bash report_api.sh 2025-01-01`

**建议通过Linux crontab定时执行任务。每个小时执行上一小时数据报表任务；每天凌晨在上一天小时报表任务全部结束，后执行上一天的天数据报表任务。考虑到上游渠道出数据可能存在延迟，每天早上执行Report API三方数据拉取任务。**
<br><br><br>
## 运行日志
本任务启动后，会自动生成log日志目录和对应小时/天的日志文件，路径为 `/nebula/log`。请根据运行日志来排查问题。
<br><br><br>
## 可能的常见问题
1. map阶段报错，提示找不到文件。
* 解决方法：参见部署1.，确保聚合SDK请求日志文件和上报日志文件已经分别存在于正确的路径。

2. 运行过程中sort阶段报错，提示进程终止。造成此报错的原因在任务执行中有一个需要大量内存去排序的阶段，机器预留的内存比例不足。在小时运行脚本 `/nebula/crontab/hour_report.sh`中， 66和72行：sort命令默认指定了参数 `-S 70%`，即最高使用整台机器70%的内存来排序。如果在该台机器上还运行有其他的服务占用内存，可能会导致本任务的内存不足。
* 解决方法：降低本任务允许使用整台机器的最高内存比例，例如`-S 30%`。降低该比例可能会导致任务中间执行的速度变慢。

3. join_and_update_db.py或db_utils.py报错，更新失败。
* 解决方法：参见部署2.，确保MySQL数据库配置与代码一致。