---
lab:
  title: 01 - 管理 Azure Active Directory 标识
  module: Administer Identity
---

# <a name="lab-01---manage-azure-active-directory-identities"></a>实验室 01 - 管理 Azure Active Directory 标识

# <a name="student-lab-manual"></a>学生实验室手册

## <a name="lab-scenario"></a>实验室方案

In order to allow Contoso users to authenticate by using Azure AD, you have been tasked with provisioning users and group accounts. Membership of the groups should be updated automatically based on the user job titles. You also need to create a test Azure AD tenant with a test user account and grant that account limited permissions to resources in the Contoso Azure subscription.

<bpt id="p1">**</bpt>Note:<ept id="p1">**</ept> An <bpt id="p2">**</bpt><bpt id="p3">[</bpt>interactive lab simulation<ept id="p3">](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)</ept><ept id="p2">**</ept> is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same.

## <a name="objectives"></a>目标

在此实验中，将执行以下操作：

+ 任务 1：创建和配置 Azure AD 用户
+ 任务 2：创建已分配动态成员身份的 Azure AD 组
+ 任务 3：创建 Azure Active Directory (AD) 租户（可选 - 实验室环境问题）
+ 任务 4：管理 Azure AD 来宾用户（可选 - 实验室环境问题）

## <a name="estimated-timing-30-minutes"></a>预计用时：30 分钟

## <a name="architecture-diagram"></a>体系结构关系图
![image](../media/lab01.png)

## <a name="instructions"></a>说明

### <a name="exercise-1"></a>练习 1

#### <a name="task-1-create-and-configure-azure-ad-users"></a>任务 1：创建和配置 Azure AD 用户

在此任务中，我们将创建和配置 Azure AD 用户。

>**注意**：如果你之前在此 Azure AD 租户上使用过 Azure AD Premium 的试用版许可证，则需要一个新的 Azure AD 租户，并在该新 Azure AD 租户中执行任务 3 后执行任务 2。

1. 登录 [Azure 门户](https://portal.azure.com)。

1. 在 Azure 门户中，搜索并选择“Azure Active Directory”。

1. 在 Azure Active Directory 边栏选项卡上，向下滚动到“管理”部分，单击“用户设置”，然后查看可用的配置选项。

1. 在 Azure Active Directory 边栏选项卡上的“管理”部分，单击“用户”，然后单击你的用户帐户以显示其“个人资料”设置。 

1. 在“设置”部分单击“编辑”，将“使用位置”设置为“美国”，然后单击“保存”以应用更改    。

    >**注意**：必须执行此操作，以便稍后在本实验室中将 Azure AD Premium P2 许可分配给你的用户帐户。

1. 导航回“用户 - 所有用户”边栏选项卡，然后单击“+ 新增用户” 。

1. 使用以下设置创建新用户（将其他设置保留为默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 用户名 | az104-01a-aaduser1 |
    | 名称 | az104-01a-aaduser1 |
    | 让我创建密码 | enabled |
    | 初始密码 | 提供安全密码 |
    | 使用位置 | **美国** |
    | 职务 | 云管理员 |
    | 部门 | **IT** |

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: <bpt id="p2">**</bpt>Copy to clipboard<ept id="p2">**</ept> the full <bpt id="p3">**</bpt>User Principal Name<ept id="p3">**</ept> (user name plus domain). You will need it later in this task.

1. 在用户列表中，单击新创建的用户帐户以显示其边栏选项卡。

1. 查看“管理”部分的可用选项并注意，你可以标识分配给该用户帐户的 Azure AD 角色以及该用户帐户对 Azure 资源的权限。

1. 在“管理”部分，单击“分配的角色”，然后单击“添加分配”按钮并分配“用户管理员”角色至“az104-01a-aaduser1”。

    >**注意**：预配新用户时，还可以选择分配 Azure AD 角色。

1. Open an <bpt id="p1">**</bpt>InPrivate<ept id="p1">**</ept> browser window and sign in to the <bpt id="p2">[</bpt>Azure portal<ept id="p2">](https://portal.azure.com)</ept> using the newly created user account. When prompted to update the password, change the password to a secure password of your choosing. 

    >**注意**：你可以粘贴剪贴板的内容，而不必输入用户名（包括域名）。

1. 在“InPrivate”浏览器窗口，在 Azure 门户中，搜索并选择“Azure Active Directory” 。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: While this user account can access the Azure Active Directory tenant, it does not have any access to Azure resources. This is expected, since such access would need to be granted explicitly by using Azure Role-Based Access Control. 

1. 在“InPrivate”浏览器窗口，在 Azure AD 边栏选项卡上，向下滚动到“管理”部分，单击“用户设置”，请注意，你无权修改任何配置选项。

1. 在 InPrivate 浏览器窗口，在 Azure AD 边栏选项卡上的“管理”部分，单击“用户”，然后单击“+ 新用户”。

1. 使用以下设置创建新用户（将其他设置保留为默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 用户名 | az104-01a-aaduser2 |
    | 名称 | az104-01a-aaduser2 |
    | 让我创建密码 | enabled |
    | 初始密码 | 提供安全密码 |
    | 使用位置 | **美国** |
    | 职务 | **系统管理员** |
    | 部门 | **IT** |

1. 以 az104-01a-aaduser1 用户身份从 Azure 门户注销，并关闭 InPrivate 浏览器窗口。

#### <a name="task-2-create-azure-ad-groups-with-assigned-and-dynamic-membership"></a>任务 2：创建已分配动态成员身份的 Azure AD 组

在此任务中，你将创建具有已分配动态成员身份的 Azure Active Directory 组。

1. 返回使用你的用户帐户登录的 Azure 门户，导航回到 Azure AD 租户的“概述”边栏选项卡，在“管理”部分单击“许可证”   。

    >**注意**：需要 Azure AD Premium P1 或 P2 许可证才能实现动态组。

1. 在“管理”部分，单击“所有产品”。

1. 单击“+ 试用/购买”并激活 Azure AD Premium P2 免费试用版。

1. 刷新浏览器窗口，验证是否激活成功。 

 >为了让 Contoso 用户能够使用 Azure AD 进行身份验证，你的任务是预配用户和组账户。

1. 在“许可证 - 所有产品”边栏选项卡，选择“Azure Active Directory Premium P2”条目，然后将 Azure AD Premium P2 的所有许可证选项分配给你的用户帐户和两个新创建的用户帐户。

1. 在 Azure 门户中，导航回“Azure AD 租户”边栏选项卡，然后单击“组”。

1. 使用“+ 新建组”按钮创建一个新组，设置如下：

    | 设置 | 值 |
    | --- | --- |
    | 组类型 | **安全性** |
    | 组名称 | IT 云管理员 |
    | 组说明 | Contoso IT 云管理员 |
    | 成员身份类型 | 动态用户 |

    >**注意**：如果“成员身份类型”下拉列表显示为灰色，请等待几分钟，然后刷新浏览器页面。

1. 单击“添加动态查询”。

1. 在“动态成员资格规则”边栏选项卡的“配置规则”选项卡上，使用以下设置创建新规则 ：

    | 设置 | 值 |
    | --- | --- |
    | 属性 | **jobTitle** |
    | 运算符 | **等于** |
    | 值 | 云管理员 |

1. 组的成员身份应根据用户职位自动更新。 

1. 返回 Azure AD 租户的“组 - 所有组”边栏选项卡，单击“+ 新建组”按钮，并使用以下设置创建新组 ：

    | 设置 | 值 |
    | --- | --- |
    | 组类型 | **安全性** |
    | 组名称 | IT 系统管理员 |
    | 组说明 | Contoso IT 系统管理员 |
    | 成员身份类型 | 动态用户 |

1. 单击“添加动态查询”。

1. 在“动态成员资格规则”边栏选项卡的“配置规则”选项卡上，使用以下设置创建新规则 ：

    | 设置 | 值 |
    | --- | --- |
    | 属性 | **jobTitle** |
    | 运算符 | **等于** |
    | 值 | **系统管理员** |

1. 你还需要创建具有测试用户帐户的测试 Azure AD 租户，并向该帐户授予 Contoso Azure 订阅中资源的有限访问权限。 

1. 返回 Azure AD 租户的“组 - 所有组”边栏选项卡，单击“+ 新建组”按钮，并使用以下设置创建新组 ：

    | 设置 | 值 |
    | --- | --- |
    | 组类型 | **安全性** |
    | 组名称 | IT 实验室管理员 |
    | 组说明 | Contoso IT 实验室管理员 |
    | 成员身份类型 | **已分配** |
    
1. 请单击“未选定成员”。

1. 在“添加成员”边栏选项卡，搜索并选择“IT 云管理员”和“IT 系统管理员”组，然后回到“新组”边栏选项卡，单击“创建”。

1. Back on the <bpt id="p1">**</bpt>Groups - All groups<ept id="p1">**</ept> blade, click the entry representing the <bpt id="p2">**</bpt>IT Cloud Administrators<ept id="p2">**</ept> group and, on then display its <bpt id="p3">**</bpt>Members<ept id="p3">**</ept> blade. Verify that the <bpt id="p1">**</bpt>az104-01a-aaduser1<ept id="p1">**</ept> appears in the list of group members.

    >                **注意：** 我们提供 **[交互式实验室模拟](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)** ，让你能以自己的节奏点击浏览实验室。

1. 你可能会发现交互式模拟与托管实验室之间存在细微差异，但演示的核心概念和思想是相同的。

#### <a name="task-3-create-an-azure-active-directory-ad-tenant-optional---lab-environment-issue"></a>任务 3：创建 Azure Active Directory (AD) 租户（可选 - 实验室环境问题）

在此任务中，你将创建新的 Azure AD 租户。

   ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: There is a known issue with the Captcha verification in the lab environment. If you experience this issue, please skip both this task and the next. We are working on a solution.

1. 在 Azure 门户中，搜索并选择“Azure Active Directory”。

1. 单击“管理租户”，然后在下一个屏幕上单击“+ 创建”，并指定以下设置 ：

    | 设置 | 值 |
    | --- | --- |
    | 目录类型 | **Azure Active Directory** |
    
1. 单击“下一步: 配置”

    | 设置 | 值 |
    | --- | --- |
    | 组织名称 | Contoso 实验室 |
    | 初始域名 | 由小写字母和数字组成并以字母开头的任何有效 DNS 名称 | 
    | 国家/地区 | **美国** |

   > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: The <bpt id="p2">**</bpt>Initial domain name<ept id="p2">**</ept> should not be a legitimate name that potentially matches your organization or another. The green check mark in the <bpt id="p1">**</bpt>Initial domain name<ept id="p1">**</ept> text box will indicate that the domain name you typed in is valid and unique.

1. 依次单击“查看 + 创建”、“创建”。 

1. 显示新创建的 Azure AD 租户的边栏选项卡，方法是使用“单击此处导航到新租户：Contoso 实验室”链接或 Azure 门户工具栏中的“目录 + 订阅”按钮（正好在 Cloud Shell 按钮的右侧）。

#### <a name="task-4-manage-azure-ad-guest-users"></a>任务 4：管理 Azure AD 来宾用户。

在此任务中，你将创建 Azure AD 来宾用户，并授予他们访问 Azure 订阅中的资源的权限。

1. 在显示 Contoso 实验室 Azure AD 租户的 Azure 门户中，在“管理”部分，单击“用户”，然后单击“添加新用户”。

1. 使用以下设置创建新用户（将其他设置保留为默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 用户名 | az104-01b-aaduser1 |
    | 名称 | az104-01b-aaduser1 |
    | 让我创建密码 | enabled |
    | 初始密码 | 提供安全密码 |
    | 职务 | **系统管理员** |
    | 部门 | **IT** |

1. 单击新建的个人资料。

    ><bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: <bpt id="p2">**</bpt>Copy to clipboard<ept id="p2">**</ept> the full <bpt id="p3">**</bpt>User Principal Name<ept id="p3">**</ept> (user name plus domain). You will need it later in this task.

1. 使用 Azure 门户工具栏中的“目录 + 订阅”按钮（正在 Cloud Shell 按钮的右侧）切换回默认的 Azure AD 租户。

1. 导航回“用户 - 所有用户”边栏选项卡，然后单击“+ 新增来宾用户”。

1. 使用以下设置邀请新的来宾用户（其他设置保留默认值）：

    | 设置 | 值 |
    | --- | --- |
    | 名称 | az104-01b-aaduser1 |
    | 电子邮件地址 | 你之前在此任务中复制的用户主体名称 |
    | 使用位置 | **美国** |
    | 职务 | 实验室管理员 |
    | 部门 | **IT** |

1. 单击“邀请”。  

1. 回到“用户 - 所有用户”边栏选项卡，单击表示新创建的来宾用户帐户的条目。

1. 在“az104-01b-aaduser1 - 配置文件”边栏选项卡上，单击“组” 。

1. 请单击“+ 添加成员身份”并将来宾用户帐户添加到“IT 实验室管理员”组。


#### <a name="task-5-clean-up-resources"></a>任务 5：清理资源

> <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not incur unexpected costs. While, in this case, there are no additional charges associated with Azure Active Directory tenants and their objects, you might want to consider removing the user accounts, the group accounts, and the Azure Active Directory tenant you created in this lab.

 > <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>:  Don't worry if the lab resources cannot be immediately removed. Sometimes resources have dependencies and take a longer time to delete. It is a common Administrator task to monitor resource usage, so just periodically review your resources in the Portal to see how the cleanup is going. 

1. In the <bpt id="p1">**</bpt>Azure Portal<ept id="p1">**</ept> search for <bpt id="p2">**</bpt>Azure Active Directory<ept id="p2">**</ept> in the search bar. Within <bpt id="p1">**</bpt>Azure Active Directory<ept id="p1">**</ept> under <bpt id="p2">**</bpt>Manage<ept id="p2">**</ept> select <bpt id="p3">**</bpt>Licenses<ept id="p3">**</ept>. Once at <bpt id="p1">**</bpt>Licenses<ept id="p1">**</ept> under <bpt id="p2">**</bpt>Manage<ept id="p2">**</ept> select <bpt id="p3">**</bpt>All Products<ept id="p3">**</ept> and then select <bpt id="p4">**</bpt>Azure Active Directory Premium P2<ept id="p4">**</ept> item in the list. Proceed by then selecting <bpt id="p1">**</bpt>Licensed Users<ept id="p1">**</ept>. Select the user accounts <bpt id="p1">**</bpt>az104-01a-aaduser1<ept id="p1">**</ept> and <bpt id="p2">**</bpt>az104-01a-aaduser2<ept id="p2">**</ept> to which you assigned licenses in this lab, click <bpt id="p3">**</bpt>Remove license<ept id="p3">**</ept>, and, when prompted to confirm, click <bpt id="p4">**</bpt>Yes<ept id="p4">**</ept>.

1. 在 Azure 门户中，导航到“用户 - 所有用户”边栏选项卡，单击代表 az104-01b-aaduser1 来宾用户帐户的条目，在“az104-01b-aaduser1 - 个人资料”边栏选项卡上，单击“删除”，然后在系统提示确认时，单击“确定”。

1. 重复相同的步骤序列，删除你在本实验室创建的其余用户帐户。

1. 导航到“组 - 所有组”边栏选项卡，选择你在本实验室中创建的组，单击“删除”，然后在提示确认时，单击“确定”。

1. 在 Azure 门户中，使用 Azure 门户工具栏中的“目录 + 订阅”按钮（紧邻 Cloud Shell 按钮右侧）来显示“Contoso 实验室”Azure AD 租户的边栏选项卡。

1. 导航回到“用户 - 所有用户”边栏选项卡，单击代表“az104-01b-aaduser1”用户帐户的条目，在“az104-01b-aaduser1 - 配置文件”边栏选项卡上单击“删除”，然后在提示确认时单击“确定”。

1. Navigate to the <bpt id="p1">**</bpt>Contoso Lab - Overview<ept id="p1">**</ept> blade of the Contoso Lab Azure AD tenant, click <bpt id="p2">**</bpt>Manage tenants<ept id="p2">**</ept> and then on the next screen, select the box next to <bpt id="p3">**</bpt>Contoso Lab<ept id="p3">**</ept>, click <bpt id="p4">**</bpt>Delete<ept id="p4">**</ept>, on the <bpt id="p5">**</bpt>Delete tenant 'Contoso Labs'?<ept id="p5">**</ept> blade, click the <bpt id="p1">**</bpt>Get permission to delete Azure resources<ept id="p1">**</ept> link, on the <bpt id="p2">**</bpt>Properties<ept id="p2">**</ept> blade of Azure Active Directory, set <bpt id="p3">**</bpt>Access management for Azure resources<ept id="p3">**</ept> to <bpt id="p4">**</bpt>Yes<ept id="p4">**</ept> and click <bpt id="p5">**</bpt>Save<ept id="p5">**</ept>.

1. 导航回“删除目录‘Contoso 实验室’”边栏选项卡，然后依次单击“刷新”和“删除”  。

> <bpt id="p1">**</bpt>Note<ept id="p1">**</ept>: If a tenant has a trial license, then you would have to wait for the trial license expiration before you could delete the tenant. This would not incur any additional cost.

#### <a name="review"></a>审阅

在此实验室中，你执行了以下操作：

- 创建并配置了 Azure AD 用户
- 创建已分配动态成员身份的 Azure AD 组
- 创建 Azure Active Directory (AD) 租户
- 管理 Azure AD 来宾用户 
