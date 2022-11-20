# AD（Active Directory）第一部分-基础知识
![](https://pics5.baidu.com/feed/e61190ef76c6a7ef2b4d4c4fa4d1d25bf3de6622.png@f_auto?token=a69f763463b9e8ca45e4bdc1d3be3e2d)

本文侧重于从不同角度了解`Windows Active Directory`环境。如从管理员身份配置安全策略的角度、攻击者绕过安全策略的角度、检测攻击者的角度。导致`Active Directory`受攻击破坏的因素有很多，比如错误的配置、糟糕的维护程序以及管理员犯的其他很多错误。文章涉及基本和高级的概念、环境配置及攻击，内容可能有点长，但是这有助于模拟不同的攻击，模拟和了解红队的攻击行为。

`Active Directory`简单来说，就是`Microsfot`提供的一项功能服务，它充当集中存储库并存储与`Active Directory` 用户、计算机、服务器和组织内的其他资源等对象相关的所有数据，它使系统管理员的管理变得容易。但它的主要功能是提供一种在域环境中对用户和机器进行身份验证的方法。使用 `Active Directory`，可以远程管理用户、工作站及其权限等资源。因此，它是一个可从网络上的任何地方访问的单一管理界面。它主要是 `Microsoft Windows` 的一项功能，但其他操作系统也可以加入其中，例如你可以在 `Active Directory` 环境中加入`Linux` 主机。

简而言之，域可以称为共享公共 `Active Directory` 数据库的所有 `Active Directory` 对象（如用户、计算机、组等）的集合或结构，并由称为域控制器的域的主服务器管理。域始终以其唯一的名称来引用，并且具有正确的域名结构。

我们可以将`Active Directory`基础结构拆分成多个单独的域，以创建更小的边界，以便可以在大型网络中分离不同域的管理任务。在`Active Directory`环境中，域还可以为管理某些设置(如密码策略和帐户锁定策略)创建边界，以便它们只能应用于域级别的域用户帐户。我们将在本系列的后面部分详细讨论组策略和错误配置的策略。

![](https://pics3.baidu.com/feed/f7246b600c338744f24a47e20624a4f3d62aa052.jpeg@f_auto?token=ff8b45a2e781ebc3cdfac756a8f96460)

*   组、用户、计算机等对象。
    
*   身份认证服务
    
*   组策略
    
*   DNS
    
*   DHCP
    

# Active Directory PowerShell模块

通过在`Powershell`中导入`Active Directory`模块，我们可以检索有关域环境的基本信息。

> 默认情况下，`Active Directory`模块只存在于域控制器中，不存在工作站上。

这些文件的路径：`C:\Windows\Microsoft.NET\assembly\GAC_64\Microsoft.ActiveDirectory.Management\`

默认情况下，此模块需要在要启用需要管理权限的 `Active Directory powershell` 模块的客户端计算机上安装远程服务器管理工具包 (`RSAT`)。每个域控制器都安装了 `RSAT`。因此域控制器和成员服务器都安装了内置的 `Active Directory powershell` 模块。但是也有一种方法可以在工作站上使用它（无需安装 `RSAT`），只需从域控制器复制 `DLL` 文件并将其导入到 `powershell` 会话中即可。

要在加入域的工作站上导入它，请从此处下载它(https://github.com/ScarredMonk/RootDSE-ActiveDirectory)，然后使用`Import-Module`简单地导入它，然后就可以使用此模块中的任何命令。

`Import-Module '.\Microsoft.ActiveDirectory.Management.dll'`

PS C:\\Users\\scarred.monk> Get-ADDomainAllowedDNSSuffixes                 : {}ChildDomains                       : {matrix.rootdse.lab}ComputersContainer                 : CN=Computers,DC=rootdse,DC=labDeletedObjectsContainer            : CN=Deleted Objects,DC=rootdse,DC=labDistinguishedName                  : DC=rootdse,DC=labDNSRoot                            : rootdse.labDomainControllersContainer         : OU=Domain Controllers,DC=rootdse,DC=labDomainMode                         : Windows2016DomainDomainSID                          : S-1-5-21-580985966-2115238843-2989639066ForeignSecurityPrincipalsContainer : CN=ForeignSecurityPrincipals,DC=rootdse,DC=labForest                             : rootdse.labInfrastructureMaster               : RDSEDC01.rootdse.labLastLogonReplicationInterval       :LinkedGroupPolicyObjects           : {CN={31B2F340-016D-11D2-945F-00C04FB984F9},CN=Policies,CN=System,DC=rootdse,DC=lab}LostAndFoundContainer              : CN=LostAndFound,DC=rootdse,DC=labManagedBy                          :Name                               : rootdseNetBIOSName                        : rootdseObjectClass                        : domainDNSObjectGUID                         : 70b22e8c-d4e3-4690-b4e0-0998b0125fb2ParentDomain                       :PDCEmulator                        : RDSEDC01.rootdse.labPublicKeyRequiredPasswordRolling   : TrueQuotasContainer                    : CN=NTDS Quotas,DC=rootdse,DC=labReadOnlyReplicaDirectoryServers    : {}ReplicaDirectoryServers            : {RDSEDC01.rootdse.lab}RIDMaster                          : RDSEDC01.rootdse.labSubordinateReferences              : {DC=matrix,DC=rootdse,DC=lab, DC=ForestDnsZones,DC=rootdse,DC=lab, DC=DomainDnsZones,DC=rootdse,DC=lab, CN=Configuration,DC=rootdse,DC=lab}SystemsContainer                   : CN=System,DC=rootdse,DC=labUsersContainer                     : CN=Users,DC=rootdse,DC=lab

【----帮助网安学习，以下所有学习资料免费领！关注我，私信回复“资料”获取！】

PS C:\\Users\\scarred.monk> (Get-ADDomain).DNSRootrootdse.lab

每个域都有一个唯一的 `SID`（安全标识符）来标识它。通常，`SID` 用于唯一标识安全主体，例如用户帐户、计算机帐户或在安全上下文中运行的进程或用户或计算机帐户。 `SID` 在其范围内（域或本地）是唯一的，并且永远不会被重用。对于域帐户，安全主体的 `SID` 是通过将域的 `SID` 与帐户的相对标识符 (`RID`) 连接起来创建的。

`RID`(相对标识符)是`Active Directory`对象的安全标识符(`SID`)的一部分，用于唯一标识域中的帐户或组。它在创建时分配给`Active Directory`对象。`RID`是`SID`的最后一部分。

例如，如果域是 `matrix.rootdse.lab`，并且矩阵域中的计算机具有主机名 `MTRXDC01`，则该计算机的 `FQDN` 将是 `mtrxdc01.matrix.rootdse.lab`

### # 4、域控制器（Domain Controller）

简而言之，`Active Directory`域控制器承载对域中的身份验证请求进行响应的服务。它对网络上的用户访问进行身份验证和验证。当用户和计算机帐户登录到网络时，他们向域控制器进行身份验证，域控制器验证他们的信息(如用户名、密码)，然后决定是允许还是拒绝这些用户的访问。域控制器是攻击者的重要服务器和主要目标，因为它持有`Active Directory`环境的密钥。每个域至少有一个域控制器(也可以有其他域控制器)。

PS C:\\Users\\scarred.monk> (Get-ADDomainController).HostNameRDSEDC01.rootdse.lab

域控制器提供名称解析服务，并负责将域数据库中有关域对象的信息保持为最新。`Active Directory`数据库存储在文件`C：\WINDOWS\NTDS\ntds.dit`中，该文件在域控制器中维护。如果此文件被盗，则有关`Active Directory`对象(如用户、计算机、组、GPO等)的所有信息(包括用户凭据)也会受到威胁。

出于备份目的，域控制器有三种类型，即主域控制器、只读域控制器和附加域控制器。只读域控制器(RODC)不允许对数据库进行任何更改。如果是只读域控制器，则必须在可写域控制器上进行更改，然后将其复制到特定域中的只读域控制器。只读域控制器是为了解决在远程位置的分支机构中常见的问题，这些分支机构可能没有域控制器，或者物理安全性差、网络带宽差，或者没有当地的专业知识来支持它。只读域控制器的主要用途是促进来自远程办公分支机构的身份验证，并允许用户访问域资源。

域树表示为一系列以分层顺序连接在一起的域，这些域使用相同的DNS命名空间。当我们将子域添加到父域时，会创建域树。例如，有一个根域rootdse.lab，并向其添加了一个新的域矩阵(FQDN为matrix.rootdse.lab)，一旦在两者之间自动创建树系信任，它就会成为同一域树的一部分。信任将在下一节中解释。

`Active Directory`林是共享公共架构的多个域树的集合，所有域通过信任连接在一起。林中的每个域都可以有一个或多个域控制器，这些域控制器可以与其他域交互，也可以访问来自其他域的资源。林的名称与根域相同。如果林包含单个域，则该域本身就是根域。

我们可以按如下方式检查`Active Directory`中的林名称：

PS C:\\Users\\scarred.monk> Get-ADForestApplicationPartitions : {DC=DomainDnsZones,DC=matrix,DC=rootdse,DC=lab, DC=ForestDnsZones,DC=rootdse,DC=lab, DC=DomainDnsZones,DC=rootdse,DC=lab}CrossForestReferences : {}DomainNamingMaster    : RDSEDC01.rootdse.labDomains               : {matrix.rootdse.lab, rootdse.lab}ForestMode            : Windows2016ForestGlobalCatalogs        : {RDSEDC01.rootdse.lab, MTRXDC01.matrix.rootdse.lab}Name                  : rootdse.labPartitionsContainer   : CN=Partitions,CN=Configuration,DC=rootdse,DC=labRootDomain            : rootdse.labSchemaMaster          : RDSEDC01.rootdse.labSites                 : {Default-First-Site-Name}SPNSuffixes           : {}UPNSuffixes           : {}

通过从上面的输出请求`RootDomain`属性，可以过滤上面的命令以提取林名称：

PS C:\\Users\\scarred.monk> (Get-ADForest).RootDomainrootdse.lab

同样，我们可以使用此方法查看任何特定的属性，方法是将整个命令放在括号中，然后键入要查看的属性名称。

在林中，域通过称为信任的连接相互连接。这就是为什么一个域的用户能够访问其他域的资源。在 `Active Directory` 环境中，一旦在两个域之间建立信任关系，它就会向跨实体的用户、组和计算机授予对资源的访问权限。这是通过连接域之间的身份验证系统并允许身份验证流量在它们之间流动来完成的。稍后将详细讨论这一点，以了解当一个域中的用户请求访问另一个域的资源时会发生什么，当前域控制器向用户返回一个特殊的票证（用域间信任密钥签名），该票证指的是另一个域的域控制器。这部分会在后续的 `Kerberos` 部分详细解释。

信托可以是单向的，也可以是双向的。在单向信任域中，域一信任域二，这意味着域一是信任域，域二将是受信任域。某个域中的用户访问另一个域中的资源，该用户需要在信任域中。下图显示了两个域之间信任流的图形表示。

![](https://pics7.baidu.com/feed/f2deb48f8c5494ee67320ae17dde9df498257eef.jpeg@f_auto?token=016b3f15f56b0290e59b9c37eb3cae8d)

在双向信任的情况下，所有域都可以与所有用户共享资源，而不管它们属于哪个域。顾名思义，信任是双向的。当我们在两个域（域一和域二）之间创建信任时，域一中的用户帐户将可以访问域二中的资源，反之亦然。

有各种类型的信任。信任可以是传递性的，也可以是非传递性的。下表解释了不同类型的信任。

![](https://pics6.baidu.com/feed/d788d43f8794a4c2220571645cdf66dfad6e3914.jpeg@f_auto?token=bb5232530ccec4ad5dab67e1751f0199)

![](https://pics6.baidu.com/feed/1b4c510fd9f9d72af77ac1238301553e349bbbac.jpeg@f_auto?token=c0485cf96147063c4bdcd1fc75837eb1)

在这里，信任关系通过每个受信任域。因为它是可传递的信任，所以它允许域1中的用户帐户访问域3中的资源，反之亦然(而不必在域1和域3之间创建额外的信任)

在不可传递信任的情况下，与信任之外的域的关系受到限制。这意味着不允许其他域访问信任之外的资源。他们将无法通过其身份验证信息。

![](https://pics3.baidu.com/feed/adaf2edda3cc7cd9e3183f95692a5c35b80e917e.jpeg@f_auto?token=98befee7ef9fbf183e1512091814784f)

在上面的示例中，域1和域2之间建立了不可传递的信任关系，两个域中的用户帐户都可以访问另一个域中的资源。因此，当我们添加新的域3并在域2和域3之间创建信任时，域1中的用户不会自动被允许访问域3中的资源。

默认情况下，当添加子域或添加域树时，会自动创建双向可传递信任。两种默认信任类型是父子信任和树根信任。

### # 8、全局编录 Global Catalog(GC)

全局编录用于执行全林搜索，因为全局编录服务器包含所有对象的完整副本。默认情况下，域中的根域控制器被视为全局编录服务器。为了加快对林中其他域中对象的查询速度，全局编录服务器具有其自己域的副本和其他域对象的只读分区。假设我们必须从当前域以外的域中查询特定用户的描述属性，在这种情况下，全局编录将检索它而无需查询其他域的域控制器。

让我们举一个具有四个域的 `Active Directory` 林的示例，其中域1是根域：

![](https://pics7.baidu.com/feed/f603918fa0ec08fa9e62a8f90fc5406754fbdac0.jpeg@f_auto?token=a4b316f71ac1ea01f7e8d26bcc90994f)

由于域1是根域控制器，因此它保存当前域的完全可写目录分区：

![](https://pics3.baidu.com/feed/caef76094b36acaf83f859752ef2f01a00e99cd1.jpeg@f_auto?token=4c7ed5eec8efd227112afd65ab11b414)

全局编录服务器在目录数据库文件(`Ntds.dit`)中保存其自己域的副本(完整且可写)和林中所有其他域的部分只读副本：

![](https://pics4.baidu.com/feed/9c16fdfaaf51f3de968a18decac58d153a297901.jpeg@f_auto?token=b3b977a74721e920d26edc07ddedfb81)

1、`Active Directory`是一种目录服务，充当集中式存储库并保存与`Active Directory`对象相关的所有数据

2、`Active Directory`域是共享`Active Directory`数据库的所有对象(如用户、计算机、组等)的结构

3、域代表`Active Directory`林中的逻辑分区

4、`SID`（安全标识符）用于唯一标识用户、计算机帐户等安全主体

5、`RID (Relative identifier)` 是 `SID` 的最后一部分，用于唯一标识域内的帐户或组

10、全局编录包含所有对象的完整副本，在执行林范围搜索时使用

在第2部分中，我们将介绍不同类型的Active Directory对象以及如何查询它们。