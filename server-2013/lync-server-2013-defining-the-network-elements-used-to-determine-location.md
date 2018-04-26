﻿---
title: Lync Server 2013：定义用于确定位置的网络元素
TOCTitle: 定义用于确定位置的网络元素
ms:assetid: 7538779d-055d-44ed-8dd7-11c45fc1b9f5
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg398567(v=OCS.15)
ms:contentKeyID: 49313276
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中定义用于确定位置的网络元素

 

_**上一次修改主题：** 2012-10-29_

如果要将 Lync Server 基础结构设置为支持自动客户端位置检测，则首先需要确定用来将呼叫者映射到位置的网络元素。在 Lync Server 2013 中，您可以将下面的第 2 层和第 3 层网络元素与位置关联：

  - 无线访问点 (WAP) 基本服务集标识 (BSSID) 地址（第 2 层）

  - LLDP 交换机端口（第 2 层）

  - LLDP 交换机机架 ID（第 2 层）

  - IP 子网（第 3 层）

  - 客户端 MAC 地址（第 2 层）

以上网络元素是按优先顺序列出的。如果可以使用多个网络元素查找客户端，则 Lync Server 将按优先顺序确定要使用的机制。

以下几节提供有关使用每个网络元素的更多详细信息。

<table>
<thead>
<tr class="header">
<th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>在使用网络元素将呼叫者映射到各个位置时，使 位置信息服务数据库保持最新最为重要。例如，如果添加或更改网络元素（例如，添加 WAP），您必须删除旧条目并在位置数据库中添加新条目。</td>
</tr>
</tbody>
</table>


## 无线访问点

当客户端以无线方式连接到网络时，位置请求使用 WAP 的 BSSID 地址来确定位置。如果客户端处于漫游状态，则指明的 WAP 可能不是最近的 WAP，甚至可能选取在建筑的其他层的 WAP。若要指示该位置是近似的，您可以将 Near 或 Close to 描述符附加到位置值的前面。

此位置方法假定每个 WAP 的 BSSID 都是静态的。但如果 WAP 供应商使用动态分配的 BSSID，则从 WAP 获取的 BSSID 可能会发生更改（此情况会在 WAP 配置发生更改之后出现），并且无线客户端可以处于不接收位置的情况。为了消除这种可能性，需要用每个 WAP 使用的所有可能 BSSID 地址的 ERL 填充 位置信息服务数据库。

## LLDP 端口和交换机

支持 Link Layer Discovery Protocol-Media Endpoint Discover (LLDP-MED) 的托管以太网交换机可向与 LLDP-MED 兼容的客户端播发其身份和端口信息，然后可向位置数据库查询该信息，以提供设备的位置。可以只在交换机机架 ID 上关联 ERL，或将其向下映射到端口级别。

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Lync Server 2013 仅支持使用 LLDP-MED 来确定 Lync Phone Edition 设备和在 Windows 8 上运行的 Lync 2013 的位置。如果需要使用交换机级别第 2 层数据来确定其他基于 PC 的有线 Lync 客户端的位置，则需使用客户端 MAC 地址方法。</td>
</tr>
</tbody>
</table>


## 子网

第 3 层子网提供一种受所有 Lync Server 客户端支持的机制，该机制可用于自动检测客户端位置。使用 IP 子网是配置和管理有线客户端的最简单定位方法。但在决定使用子网之前，请使用以下问题来帮助确定子网的位置特性是否精细到足以准确找到客户端：

  - 一个或多个客户端子网是否覆盖多个楼层？

  - 一个或多个子网是否覆盖多座建筑？

  - 每个客户端子网覆盖多大楼面空间？

如果子网覆盖的区域过于广泛，则可能需要使用其他机制来查找客户端。但作为最可行的方法，我们建议客户重新组织其 IP 子网，以满足 ERL 位置特性要求而不是引发基于 SNMP 的第三方解决方案的成本和复杂性。

## 客户端 MAC 地址

若要使用客户端计算机的 MAC 地址定位呼叫者，则需要托管的以太网交换机，并且必须部署第三方 SNMP 解决方案，该解决方案可以发现连接至（或通过）这些交换机的 Lync 客户端的 MAC 地址。SNMP 解决方案会不断轮询托管交换机，以获取连接到每个端口的终结点 MAC 地址的当前映射并获取对应的端口 ID。在 Lync 客户端向 位置信息服务发出请求期间， 位置信息服务将使用客户端的 MAC 地址查询第三方应用程序，然后返回任何匹配的交换机 IP 地址和端口 ID。 位置信息服务使用此信息在其已发布的第 2 层线路映射中查询匹配记录，并将该位置返回给客户端。如果使用此选项，请确保 SNMP 应用程序与已发布的位置数据库记录之间的交换机端口标识符是一致的。

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>有些第三方 SNMP 解决方案可以支持非托管访问交换机；如果为 Lync 客户端提供服务的交换机是非托管的，但具有到托管通讯交换机的上行链路，则托管交换机可将连接至访问交换机的客户端的 MAC 地址报告回 SNMP 应用程序。 位置信息服务可利用此信息来确定用户的位置。但是，只能向非托管交换机上的所有端口分配单个 ERL，因此位置特性仅在访问交换机的机架级别而非端口级别可用。</td>
</tr>
</tbody>
</table>
