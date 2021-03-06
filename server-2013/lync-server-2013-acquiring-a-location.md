﻿---
title: Lync Server 2013：获取位置
TOCTitle: 获取位置
ms:assetid: 9bf069aa-d240-4d95-be3a-e795537f8e70
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/JJ205110(v=OCS.15)
ms:contentKeyID: 49313731
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中获取位置

 

_**上一次修改主题：** 2012-06-06_

在 Lync Server 2013 E9-1-1 部署中，每个内部连接的 Lync 或 Lync Phone Edition 客户端将主动获取其自己的位置。SIP 注册后，客户端会将知道的有关自己的所有网络连接信息通过位置请求提供给 位置信息服务，其是复制的 SQL Server 数据库支持的 Web 服务。每个中央站点池具有一个 位置信息服务，其使用网络信息在其记录中查询匹配的位置。如果有匹配项，则 位置信息服务会将位置返回给客户端。如果没有匹配项，则可能提示用户手动输入位置（取决于位置策略设置）。位置数据将以 Internet 工程任务组 (IETF) 标准化 XML 格式传输回客户端，称为“状态信息数据格式位置对象 (PIDF-LO)”。

Lync Server 客户端包含作为紧急呼叫一部分的 PIDF-LO 数据，E9-1-1 服务提供商使用此数据确定相应的 PSAP 以及将呼叫路由到该 PSAP 和正确的 ESQK，其允许 PSAP 调度程序获取呼叫者的位置。

下图演示 Lync Server 客户端如何获取位置（基于 MAC 地址的位置方法的第三方客户端除外）：

![客户端获取位置的方式的图示](images/JJ205110.4438f5fc-f1b2-444b-8565-09035363ed43(OCS.15).jpg "客户端获取位置的方式的图示")

对于获取位置的客户端，必须执行下列步骤：

1.  管理员使用网络线路（将各种类型的网络地址映射到对应紧急响应位置 (ERL) 的表）填充 位置信息服务数据库。

2.  如果使用 SIP 中继 E9-1-1 服务提供商，则管理员将针对 E9-1-1 服务提供商维护的主街道地址指南 (MSAG) 数据库验证 ERL 的市政地址部分。如果使用 ELIN 网关，则管理员将确保 PSTN 运营商会将 ELIN 上载到自动位置标识 (ALI) 数据库。

3.  在注册期间或网络更改出现时，内部连接的客户端将向 位置信息服务发送包含客户端的已发现网络地址的位置请求。

4.  位置信息服务在其已发布的记录中查询位置，并且如果发现匹配项，则将以 PIDF-LO 格式将 ERL 返回到客户端。

