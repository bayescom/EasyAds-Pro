# EasyAds Pro

![image](bayescom.png)

**智能聚合 极速变现 专业私有 卓越升级**

## 项目简介

欢迎大家使用**EasyAds Pro**

EasyAds Pro是[倍业科技](http://www.bayescom.com/)技术团队研发的一款开源聚合SDK管理项目，项目包括了聚合SDK、聚合管理平台、SDK策略服务和数据服务四大模块；旨在帮助APP开发者快速搭建属于自己的聚合SDK管理系统，赋能开发者流量变现能力。

## 六大核心能力

![image](core_func.png)

## 项目优势

### 1. 流量管理方便快捷

支持实时竞价；提供多个维度对用户进行分组，开发者可根据需要选择分组的维度，并针对不同流量分组配置不同的瀑布流；可设置最多10个分组进行A/B测试，帮助开发者寻求流量变现最优解。

### 2. 集成简单

双端均可通过选择性的配置装载不同广告渠道SDK快速集成。开发者无须进行二次开发，直接调用EasyAds Pro SDK封装的上层接口即可；前台系统可直接复用EasyAds Pro前台系统。目前已集成主流的[穿山甲(字节)](https://www.csjplatform.com/union/media/union/download)、[优量汇(腾讯)](https://adnet.qq.com/resource/sdk)、
[百青藤(百度)](https://union.baidu.com/bqt/#/)、[快手](https://u.kuaishou.com/)，开发者也可以通过自定义SDK渠道来实现其他广告SDK的对接管理。

### 3. 开源，免费，安全

项目完全开源，不进行任何用户相关信息的获取及上报，开发者可根据自己业务需求进行二次开发。

## 核心模块介绍

### 1. 整体架构

![arch.jpg](arch.jpg)


### 2. 模块介绍

| 模块名称      | 说明                                                                                                                                                                                                                                                                                                                 |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 聚合SDK     | **Advance项目** <br> 集成在手机端的SDK软件，与后端服务引擎Stella交互获取SDK管理分发策略，根据策略启动不同的广告SDK并获取对应广告，目前包括[Android](https://github.com/bayescom/Android_AdvanceSDK)和[iOS](https://github.com/bayescom/iOS_AdvanceSDK)两个项目，集成主流[穿山甲(字节)](https://www.csjplatform.com/union/media/union/download)、[优量汇(腾讯)](https://adnet.qq.com/resource/sdk)、[百青藤(百度)](https://union.baidu.com/bqt/#/)、[快手](https://u.kuaishou.com/)以及[倍业](https://www.bayescom.com/docsify/docs/#/bayescom/)广告SDK。 |
| 聚合管理平台    | **Apollo项目**+**Luna项目**+**Mysql数据库** <br> 用于管理聚合SDK的分发策略，包括媒体的广告位管理、SDK分发策略管理、用户管理、版本管理等，[Apollo](https://github.com/bayescom/EasyAds-Pro_Apollo)项目是管理操作的前端项目基于React开发，[Luna](https://github.com/bayescom/EasyAds-Pro_Luna)项目是管理操作的后端服务项目基于SpringBoot框架开发。                                                         |
| SDK策略服务   | **Stella项目**+**Redis服务** <br> [Stella](https://github.com/bayescom/EasyAds-Pro_Stella)是SDK策略引擎服务，基于OpenResty框架开发，具有较高的HTTP并发处理能力。策略引擎读取聚合管理平台(Luna)推送到Redis的配置信息获取对应广告位的流量分发管理信息，与SDK交互根据请求返回不同的SDK启动策略配置；同时策略服务还是SDK上报打点的收集服务。                                                                                    |
| 数据服务      | **Nebula项目** <br> [Nebula](https://github.com/bayescom/EasyAds-Pro_Nebula)是数据任务项目，该项目根据SDK策略服务的日志信息统计报表数据信息，也支持通过配置的Report API信息，自动拉取各方SDK数据，目前可支持穿山甲、优量汇的Report API数据拉取功能。                                                                                                                                        | 

## 代码快速接入指引

| 系统     | 代码指引                                                                                                                                                                                                                                                                    |
|-------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| iOS     | [![iOS-github](https://img.shields.io/badge/Github-EasyAds_iOS_v1.0-red.svg)](https://github.com/bayescom/iOS_AdvanceSDK)   [![iOS-gitee](https://img.shields.io/badge/Gitee-EasyAds_iOS_v1.0-orange.svg)](https://gitee.com/bayescom/iOS_AdvanceSDK)                         |
| Android | [![Android-github](https://img.shields.io/badge/Github-EasyAds_Android_v1.0-green.svg)](https://github.com/bayescom/Android_AdvanceSDK)   [![Android-gitee](https://img.shields.io/badge/Gitee-EasyAds_Android_v1.0-blue.svg)](https://gitee.com/bayescom/Android_AdvanceSDK  ) |
| Apollo | [![Apollo-github](https://img.shields.io/badge/Github-EasyAds_Pro_Apollo_v1.0-yellow.svg)](https://github.com/bayescom/EasyAds-Pro_Apollo) [![Apollo-gitee](https://img.shields.io/badge/Github-EasyAds_Pro_Apollo_v1.0-yellow.svg)](https://gitee.com/bayescom/EasyAds-Pro_Apollo)|
| Luna | [![Luna-github](https://img.shields.io/badge/Github-EasyAds_Pro_Luna_v1.0-blue.svg)](https://github.com/bayescom/EasyAds-Pro_Luna) [![Luna-gitee](https://img.shields.io/badge/Github-EasyAds_Pro_Luna_v1.0-blue.svg)](https://gitee.com/bayescom/EasyAds-Pro_Luna) |
| Stella | [![Stella-github](https://img.shields.io/badge/Github-EasyAds_Pro_Stella_v1.0-green.svg)](https://github.com/bayescom/EasyAds-Pro_Stella) [![Stella-gitee](https://img.shields.io/badge/Github-EasyAds_Pro_Stella_v1.0-green.svg)](https://gitee.com/bayescom/EasyAds-Pro_Stella)|
| Nebula | [![Nebula-github](https://img.shields.io/badge/Github-EasyAds_Pro_Nebula_v1.0-red.svg)](https://github.com/bayescom/EasyAds-Pro_Nebula) [![Nebula-gitee](https://img.shields.io/badge/Github-EasyAds_Pro_Nebula_v1.0-red.svg)](https://gitee.com/bayescom/EasyAds-Pro_Nebula)|

## 部署指引

### 1. 自由部署

用户根据自己需要，依次部署各个模块，各模块部署指引如下：

- [Apollo部署指引](deploy/apollo.md)
- [Luna部署指引](deploy/luna.md)
- [Stella部署指引](deploy/stella.md)
- [Nebula部署指引](deploy/nebula.md)

如在部署中遇到问题，请联系我们的技术支持人员或发送邮件至easyads@bayescom.com，我们将尽快为您解决问题。

### 2. Docker部署

Docker部署准备中...


## 合规指南
[合规指南参考](compliance.md)


## 技术支持

QQ群：
<a target="_blank" href="https://qm.qq.com/cgi-bin/qm/qr?k=E_IUfzy5PqOteuekOryWlfjZL6AQZuCE&jump_from=webapi"><img border="0" src="https://pub.idqqimg.com/wpa/images/group.png" alt="EasyAds开源社区群" title="EasyAds开源社区群"></a>

QQ群二维码：

![image](http://www.bayescom.com/uploads/20211220/43af3f34fc5a7bb50d84f94e374b3e98.png)

邮件技术支持：<easyads@bayescom.com>

## 更多
**倍业科技**，是一家赋能各领域媒体商业化的SaaS服务型企业，通过智能高效的流量管理和运营优化工具，为媒体充分挖掘每个流量的价值。
如果你有更高级的变现需求，欢迎与我们联系。

商务合作：<partner@bayescom.com>
