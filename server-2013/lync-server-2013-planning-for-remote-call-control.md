﻿---
title: Lync Server 2013：规划远程呼叫控制
TOCTitle: 规划远程呼叫控制
ms:assetid: 688a0328-1aa7-449f-b5f7-98c876112ed2
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg558658(v=OCS.15)
ms:contentKeyID: 49313131
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中规划远程呼叫控制

 

_**上一次修改主题：** 2012-09-05_

在 Lync Server 2013 中，支持远程呼叫控制方案可允许用户通过使用台式计算机上的 Lync 2013 控制专用交换机 (PBX) 电话。本节介绍远程呼叫控制功能以及部署远程呼叫控制的要求。

PBX 与 Lync Server 2013 之间的集成使启用了远程呼叫控制的用户可通过以下方式使用 Lync 2013 用户界面 (UI) 来控制 PBX 电话上的呼叫：

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>最终，承载用户 PBX 电话的 PBX 的功能将决定可供该用户使用的远程呼叫控制功能。</td>
</tr>
</tbody>
</table>


  - 发出传出呼叫

  - 应答传入呼叫

  - 使用即时消息应答传入呼叫
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>也就是说，呼叫者的电话号码可以与即时消息地址相关联，该地址可以来自组织的全局地址列表 (GAL)、被叫方的 Lync 联系人列表，或者联盟伙伴组织。</td>
    </tr>
    </tbody>
    </table>


  - 转接呼叫

  - 转接传入呼叫

  - 将呼叫置于保持状态

  - 交替处理多个并发呼叫

  - 在呼叫过程中应答第二个呼叫（即，呼叫等待）

  - 双音多频 (DTMF) 数字拨号

  - 在“对话”窗口中，键入 Microsoft Office OneNote 的做笔记程序的注释

此外，如果用户启用了远程呼叫控制，则 Lync 2013 将为用户提供以下呼叫信息：

  - 通过名称识别呼叫者，前提是呼叫者的电话号码存在于启用了远程呼叫控制的用户的 Microsoft Office Outlook 消息和协作客户端的联系人列表、 Lync 联系人列表或组织的 GAL 中。

  - 以前的传入和传出呼叫，保存在 Outlook 的“对话历史记录”文件夹中。

  - 未接来电通知，会发送到用户的 Outlook“收件箱”文件夹中，但是仅当接收传入呼叫时 Lync 正在运行，才会生成此通知。

## 远程呼叫控制和企业语音

虽然远程呼叫控制功能独立于 企业语音功能且无法为用户同时启用这两组功能， 企业语音提供了对启用了远程呼叫控制的用户也可用的功能子集。如果部署了 企业语音，则启用了远程呼叫控制的用户可以使用 Lync 访问以下 企业语音功能：

  - 向另一个 Lync 客户端发起音频呼叫和接收音频呼叫

  - 加入由启用了 企业语音的用户创建的会议的音频部分

## 本节内容

  - [Lync Server 2013 中远程呼叫控制的部署任务](lync-server-2013-deployment-tasks-for-remote-call-control.md)

