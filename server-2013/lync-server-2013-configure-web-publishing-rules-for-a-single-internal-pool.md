﻿---
title: Lync Server 2013：为单个内部池配置 Web 发布规则
TOCTitle: 为单个内部池配置 Web 发布规则
ms:assetid: 86ff4b2a-1ba9-46a2-a175-8b19e00a49dd
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg429712(v=OCS.15)
ms:contentKeyID: 49313479
ms.date: 12/10/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中为单个内部池配置 Web 发布规则

 

_**上一次修改主题：** 2016-12-08_

Microsoft Forefront Threat Management Gateway 2010 和 Internet Information Server 应用程序请求路由 (IIS ARR) 使用 Web 发布规则向 Internet 上的用户发布内部资源，如会议 URL。

除了虚拟目录的 Web 服务 URL 外，还必须为简单 URL、LyncDiscover URL 和 Office Web Apps Server 创建发布规则。对于每个简单 URL，必须在指向该简单 URL 的反向代理上创建单个规则。

如果您部署移动功能并使用自动发现，则需要为外部自动发现服务 URL 创建发布规则。自动发现也需要针对 控制器池和 前端池的外部 Lync Server Web 服务 URL 的发布规则。有关为自动发现创建 Web 发布规则的详细信息，请参阅 [在 Lync Server 2013 中配置反向代理以实现移动功能](lync-server-2013-configuring-the-reverse-proxy-for-mobility.md)。

使用以下过程创建 Web 发布规则。

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>这些过程假定您已安装 Forefront Threat Management Gateway (TMG) 2010 Standard Edition 或者配置了 Internet Information Server 应用程序请求路由 (IIS ARR) 扩展。您可以使用 TMG 或 IIS ARR。</td>
</tr>
</tbody>
</table>


## 在运行 TMG 2010 的计算机上创建 Web 服务器发布规则

1.  单击“开始”，依次选择“程序”、“Microsoft Forefront TMG”，然后单击“Forefront TMG 管理”。

2.  在左窗格中展开“服务器名称”，右键单击“防火墙策略”，选择“新建”，然后单击“网站发布规则”。

3.  在“欢迎使用新建 Web 发布规则向导”页上，为发布规则键入一个显示名称（例如，LyncServerWebDownloadsRule）。

4.  在“选择规则操作”页上，选择“允许”。

5.  在“发布类型”页上，选择“发布单个网站或负载平衡器”。

6.  在“服务器连接安全性”页上，选择“使用 SSL 连接到发布的 Web 服务器或服务器场”。

7.  在“内部发布详细信息”页的“内部站点名称”框中，键入承载会议内容和通讯簿内容的内部 Web 场的完全限定域名 (FQDN)。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>如果内部服务器是 Standard Edition Server，则此 FQDN 为 Standard Edition Server 的 FQDN。如果内部服务器是前端池，则此 FQDN 是用于平衡内部 Web 场服务器负载的硬件负载平衡器虚拟 IP (VIP)。TMG 服务器必须能够将 FQDN 解析为内部 Web 服务器的 IP 地址。如果 TMG 服务器不能将 FQDN 解析为正确的 IP 地址，可以选择“使用计算机名称或 IP 地址连接到发布的服务器”，然后在“计算机名称或 IP 地址”框中键入内部 Web 服务器的 IP 地址。如果这样做，则必须确保在 TMG 服务器上打开端口 53，且 TMG 服务器可以访问位于外围网络中的 DNS 服务器。还可以使用本地主机文件中的条目提供名称解析。</td>
    </tr>
    </tbody>
    </table>


8.  在“内部发布详细信息”页的“路径(可选)”框中键入 **/\*** 作为要发布的文件夹的路径。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>在网站发布向导中，只能指定一个路径。可以通过修改规则的属性来添加其他路径。</td>
    </tr>
    </tbody>
    </table>


9.  在“发布名称详细信息”页上，确认在“接受请求”下选择“此域名”，在“公共名称”框中键入外部 Web 服务的 FQDN。

10. 在“选择 Web 侦听器”页上单击“新建”，以打开“新建 Web 侦听器定义向导”。

11. 在“欢迎使用新建 Web 侦听器向导”页上的“Web 侦听器名称”框中为 Web 侦听器键入一个名称（例如 LyncServerWebServers）。

12. 在“客户端连接安全性”页上，选择“需要与客户端建立 SSL 安全连接”。

13. 在“Web 侦听器 IP 地址”页上，选择“外部”，然后单击“选择 IP 地址”。

14. 在“外部侦听器 IP 选择”页上，选择“所选网络中 Forefront TMG 计算机上指定的 IP 地址”，选择适当的 IP 地址，然后单击“添加”。

15. 在“侦听器 SSL 证书”页上，选择“为每个 IP 地址分配一个证书”，选择与外部 Web FQDN 关联的 IP 地址，然后单击“选择证书”。

16. 在“选择证书”页上，选择与在步骤 9 中指定的公共名称相匹配的证书，然后单击“选择”。

17. 在“身份验证设置”页上，选择“无身份验证”。

18. 在“单一登录设置”页上，单击“下一步”。

19. 在“正在完成 Web 侦听器向导”页上，确认“Web 侦听器”设置正确，然后单击“完成”。

20. 在“身份验证委派”页上，选择“无委派，但是客户端可以直接进行身份验证”。

21. 在“用户集”页上，单击“下一步”。

22. 在“正在完成新建 Web 发布规则向导”页上，确认 Web 发布规则设置正确，然后单击“完成”。

23. 在详细信息窗格中，单击“应用”，以保存所做的更改并更新配置。

## 在运行 IIS ARR 的计算机上创建 Web 服务器发布规则

1.  将您将用于反向代理的证书绑定到 HTTPS 协议。单击“开始”，依次选择“程序”、“管理工具”，然后单击“Internet 信息服务 (IIS) 管理器”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>有关部署和配置 IIS ARR 的其他帮助、屏幕截图和指导可以在 NextHop 文章<a href="http://go.microsoft.com/fwlink/?linkid=293391">使用 IIS ARR 作为 Lync Server 2013 的反向代理</a>中找到。</td>
    </tr>
    </tbody>
    </table>


2.  如果您尚未这样做，请导入将用于反向代理的证书。在“Internet 信息服务 (IIS) 管理器”中，在控制台左侧单击反向代理服务器名称。在控制台中心的“IIS”下方，找到“服务器证书”。右键单击“服务器证书”，并选择“打开功能”。

3.  在控制台右侧，单击“导入...”。键入证书的路径和文件名以及扩展名，或者单击“…”以浏览查找证书。选择证书，然后单击“打开”。提供用于保护私钥的密码（如果您在导出证书和私钥时分配了密码）。单击“确定”。如果证书导入成功，证书将在控制台中间的“服务器证书”中显示为一个条目。

4.  分配供 HTTPS 使用的证书。在控制台左侧，选择 IIS 服务器的“默认网站”。在右侧，单击“绑定...”。在“网站绑定”对话框中，单击“添加…”。在“添加网站绑定”对话框中的“类型：”下方，单击“https”。选择 https 将允许您选择用于 https 的证书。在“SSL 证书：”下方，选择已为反向代理导入的证书。单击“确定”。然后单击“关闭”。证书现在已绑定到安全套接字层 (SSL) 和传输层安全性 (TLS) 的反向代理。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>如果您在关闭“绑定”对话框时收到一个警告，表明缺少中间证书，则您需要找到并导入公共 CA 根证书颁发机构证书和任何中间 CA 证书。请参阅您向其请求您的证书的公共 CA 处的说明，并按照说明请求和导入证书链。如果您从 边缘服务器 导出了证书，则可以导入根 CA 证书和与 边缘服务器 相关联的任何中间 CA 证书。将根 CA 证书导入到计算机的（不要与用户存储混淆）受信任的根证书颁发机构存储中，而将中间证书导入到计算机的中间证书颁发机构存储中。</td>
    </tr>
    </tbody>
    </table>


5.  在控制台左侧的 IIS 服务器名称下面，右键单击“服务器场”，然后单击“创建服务器场…”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>如果看不到“服务器场”节点，则您需要安装应用程序请求路由。有关详细信息，请参阅<a href="lync-server-2013-setting-up-reverse-proxy-servers.md">为 Lync Server 2013 设置反向代理服务器</a>。</td>
    </tr>
    </tbody>
    </table>
    
    在“创建服务器场”对话框中，在“服务器场名称”中键入第一个 URL 的名称（这可以是用于识别目的的友好名称）。单击“下一步”。

6.  在“添加服务器”对话框中，在“服务器地址”中键入您的 前端服务器 上外部 Web 服务的完全限定的域名 (FQDN)。这里用作举例说明的名称与反向代理[Lync Server 2013 中的证书摘要 - 反向代理](lync-server-2013-certificate-summary-reverse-proxy.md)的“规划”部分使用的名称相同。参阅反向代理规划，键入 FQDN `webext.contoso.com`。确认选中“联机”旁边的复选框。单击“添加”以向此配置的 Web 服务器池添加服务器。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/JJ656815.warning(OCS.15).gif" title="warning" alt="warning" />警告：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Lync Server 使用硬件负载平衡器池化控制器和前端服务器的 HTTP 和 HTTPS 流量。在向 IIS ARR 服务器场添加服务器时，您只能提供一个 FQDN。FQDN 将是非池化服务器配置中的前端服务器或控制器，或者为服务器池配置的硬件负载平衡器的 FQDN。负载平衡 HTTP 和 HTTPS 流量唯一支持的方法是使用硬件负载平衡器。</td>
    </tr>
    </tbody>
    </table>


7.  在“添加服务器”对话框中，单击“高级设置...”。这将打开一个对话框，以为配置的 FQDN 请求定义应用程序请求路由。目的是重新定义在 IIS ARR 处理请求时使用哪个端口。
    
    默认情况下，目标 HTTP 端口必须定义为 8080。单击当前的“httpPort 80”旁边，将值设置为“8080”。单击当前的“httpsPort 443”旁边，将值设置为“4443”。将 **weight** 参数保留为 100。如果需要，在具有比较基准统计信息之后，您可以重新为给定规则定义权重。单击“完成”以完成规则配置的这一部分。

8.  您可能会看到一个重写规则对话框，通知您 IIS 管理员可以创建 URL 重写规则来自动将所有传入请求路由到服务器场。单击“是”。您将手动调整规则，但是选择“是”将设置初始配置。

9.  单击您刚刚创建的服务器场的名称。在 IIS 管理器功能视图中的“服务器场”下方，双击“缓存”。取消选择“启用磁盘缓存”。在右侧，单击“应用”。

10. 单击服务器场的名称。在 IIS 管理器功能视图中的“服务器场”下方，双击“代理”。在“代理设置”页面上，将“超时（秒）”的值更改为适合您的部署的值。单击“应用”以保存更改。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>代理超时值是一个数字，因部署而异。您应该监控您的部署并修改值以便获得最佳客户端体验。您或许能够设置较低值，如 200。如果在您的环境中支持 Lync 移动客户端，您应该将值设置为 960，以便允许来自 Office 365 的推送通知超时，其超时值为 900。很可能您需要增加超时值以避免在值太低时客户端断开连接，或者如果通过代理进行的连接未断开，请降低该数字，并在客户端断开连接之后清除。只有监控您的环境的正常情况并设定比较基准，才能准确地确定此值的恰当设置。</td>
    </tr>
    </tbody>
    </table>


11. 单击服务器场的名称。在 IIS 管理器功能视图中的“服务器场”下方，双击“路由规则”。在“路由规则”对话框的“路由”下方，清除“启用 SSL 卸载”旁边的复选框。如果无法清除该复选框，请选择“使用 URL 重写检查传入请求”的复选框。单击“应用”以保存更改。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/JJ205186.Caution(OCS.15).gif" title="Caution" alt="Caution" />警告：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>不支持反向代理的 SSL 卸载。</td>
    </tr>
    </tbody>
    </table>


12. 为必须穿过反向代理的每个 URL 重复步骤 5-11。常见列表将为以下内容：
    
      - 外部前端服务器 Web 服务：webext.contoso.com（已经通过初始演练配置）
    
      - 控制器的外部控制器 Web 服务：webdirext.contoso.com（如果部署了控制器则为可选）
    
      - 会议简单 URL：meet.contoso.com
    
      - 拨入简单 URL：dialin.contoso.com
    
      - Lync 自动发现 URL：lyncdiscover.contoso.com
    
      - Office Web Apps 服务器 URL：officewebapps01.contoso.com
        
        <table>
        <thead>
        <tr class="header">
        <th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
        </tr>
        </thead>
        <tbody>
        <tr class="odd">
        <td>Office Web Apps Server 的 URL 将使用不同的 httpsPort 地址。在步骤 7 中，您将“httpsPort”定义为“443”，并将“httpPort”定义为端口“80”。所有其他配置设置相同。</td>
        </tr>
        </tbody>
        </table>


13. 在控制台左侧，单击 IIS 服务器名称。在控制台中心，在“IIS”下方找到“URL 重写”。双击“URL 重写”以打开 URL 重写规则配置。您应会看到您在前面的步骤中创建的每个服务器场的规则。如果看不到，请确认您已在 Internet Information Server 管理器控制台中“起始页”节点的正下方单击 **IIS 服务器**名称。

14. 在“URL 重写”对话框中，以 webext.contoso.com 为例，显示的规则完整名称为 **ARR\_webext.contoso.com\_loadbalance\_SSL**。
    
      - 双击规则以打开“编辑入站规则”对话框。
    
      - 单击“条件”对话框上的“添加…”。
    
      - 在“添加条件”上，在“条件输入：”中键入 **{HTTP\_HOST}**。（在您键入时，将显示一个对话框，允许您选择条件。）在“检查输入字符串是否：”下方，选择“匹配该模式”。在“模式输入”中，键入 **\***。应选择“忽略大小写”。单击“确定”。
    
      - 在“编辑入站规则”对话框中向下滚动以找到“操作”对话框。“操作类型：”应设置为“路由至服务器场”，“方案：”应设置为“https://”，“服务器场：”应设置应用此规则的 URL。例如，这应设置为“webext.contoso.com”。“路径：”应设置为“/{R:0}”。
    
      - 单击“应用”以保存更改。单击“返回到规则”以返回到 URL 重写规则。

15. 为您已经定义的每个 SSL 重写规则（每个服务器场 URL 一个）重复在步骤 14 中定义的过程。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/JJ656815.warning(OCS.15).gif" title="warning" alt="warning" />警告：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>默认情况下，将创建 HTTP 规则并通过与 SSL 规则类似的命名表示。对于我们当前的示例，HTTP 规则将命名为 <strong>ARR_webext.contoso.com_loadbalance</strong>。无需对这些规则做任何修改，可以安全地忽略它们。</td>
    </tr>
    </tbody>
    </table>


## 在 TMG 2010 中修改 Web 发布规则的属性

1.  单击“开始”，指向“程序”，选择“Microsoft Forefront TMG”，然后单击“Forefront TMG 管理”。

2.  在左窗格中，展开“服务器名称”，然后单击“防火墙策略”。

3.  在详细信息窗格中，右键单击在前面的过程中创建的 Web 服务器发布规则（例如，LyncServerExternalRule），然后单击“属性”。

4.  在“属性”页上的“从”选项卡上，执行下列操作：
    
      - 在“此规则应用于来自这些源的通讯”列表中，单击“任意位置”，然后单击“删除”。
    
      - 单击“添加”。
    
      - 在“添加网络实体”中，展开“网络”，依次单击“外部”、“添加”，然后单击“关闭”。

5.  在“到”选项卡上，确保“转发原始主机头，而不是实际主机头”复选框处于选中状态。

6.  在“桥接”选项卡上，选中“将请求重定向到 SSL 端口”复选框，然后指定端口 **4443**。

7.  在“公共名称”选项卡上，添加简单 URL（例如，meet.contoso.com 和 dialin.contoso.com）。

8.  单击“应用”保存更改，然后单击“确定”。

9.  在详细信息窗格中，单击“应用”，以保存所做的更改并更新配置。

## 在 IIS ARR 中修改 Web 发布规则的属性

1.  单击“开始”，依次选择“程序”、“管理工具”，然后单击“Internet 信息服务 (IIS) 管理器”。

2.  在控制台左侧，单击 IIS 服务器名称。

3.  在控制台中心，在“IIS”下方找到“URL 重写”。双击“URL 重写”以打开 URL 重写规则配置。

4.  双击需要修改的规则。在“匹配 URL”、“条件”、“服务器变量”或“操作”中根据需要进行修改。

5.  单击“应用”以提交更改。单击“返回到规则”以修改其他规则，或者如果修改完毕，请关闭“IIS 管理器”控制台。

