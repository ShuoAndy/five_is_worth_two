## 数据库技术选型
我们选择了轻量级的的 SQLite3 数据库来对模型进行持久化存储。对于用户上传的文件，我们直接将其保存在服务器文件系统中并在数据库模型中记录了相应文件的路径。

## 模型描述
数据库存储的模型有以下几类：

- 用户模型（UserEntry）：存储用户的账号、密码、身份类型等个人信息。该模型统一实现了超级管理员、管理员、需求方、标注方以及中介五类用户的信息存储。
- 任务模型（Task）：存储了每个任务的详细信息，如任务名称、任务类型、任务完成和验收进展等等。
- 公告模型（Notice）：存储了超级管理员发布的公告。
- 银行卡模型（Bankcard）：存储了由超级管理员发布的虚拟银行卡，记录了每张银行卡的账号、密码、余额等信息。
- 举报信息模型（Report）：存储了针对需求方或标注方的举报，对于每条举报都记录了举报的发起者、举报对象、举报理由等信息。

![数据库 ER 图](../images/database%20ER.png)

## 用户模型
| 字段 | 类型 | 用途 |
| --- | --- | --- |
| id | BigAuto | 用户唯一 id，按照注册顺序分配 |
| username | Char | 用户名 |
| password | Char | 使用 SHA 256 加密过的用户密码 |
| role | SmallInteger | 代表用户身份类别的序号<sup id="fn1">[^1]</sup> |
| token | Char | 用于与会话进行比对，记录和验证用户登录状态的动态令牌 |
| banned | Boolean | 用户是否处于被封禁状态 |
| credits | Integer | 用户的信用分 |
| points | Integer | 用户的积分数额 |
| bankcard_number | Char | 用户绑定的银行卡号 |
| invitation_code | Char | 用户可用于邀请新用户的邀请码 |
| email | Char | 用户绑定的邮箱 |
| code | Char | 用户使用邮箱修改密码时发送给用户的验证码 |
| code_expiration | Char | 验证码过期时间 |
| extra_info | Text | 以 Json 字符串形式保存的附加信息，对于不同类型用户有所不同<sup id="fn2">[^2]</sup> |

    [^1]:共有五类用户身份：即超级管理员（1），管理员（2），需求方（3），标注方（4），中介（5）。

    [^2]:对于需求方而言，extra_info 中额外记录了其 VIP 等级和 VIP 过期时间；对于标注方而言，extra_info 额外记录了其历史上接受过的所有任务的 id，其通过完成任务获得的积分数额（区别于充值获得的，用于标注方排行榜）以及其接受任务的偏好情况。

## 任务模型
| 字段 | 类型 | 用途 |
| --- | --- | --- |
| id | BigAuto | 任务唯一 id，按照发布顺序分配 |
| name | Char | 任务名称 |
| status | SmallInteger | 任务状态对应的序号<sup id="fn1">[1]</sup> |
| created_time | Char | 任务创建时间 |
| duration | Integer | 任务总时限（分钟） |
| time_limit | Integer | 单题任务时限（秒） |
| category | SmallInteger | 任务类型对应的序号<sup id="fn2">[2]</sup> |
| distribute_user_list | List | 分发的标注方 id 列表 |
| dataset_size | Integer | 任务中的数据集规模 |
| assign_num | Integer | 完成任务所需的标注方总人数 |
| reward | Integer | 积分奖励量 |
| credit_lower_bound | Integer | 可接受该任务的标注方的信用分下限 |
| assigner | BigInteger | 发布任务的需求方用户 id |
| rejecter_list | List | 拒绝过此任务的标注方 id 列表 |
| thread_list | List | 正在进行该任务的标注方的进展<sup id="fn3">[3]</sup> |
| content_path | Text | 该任务对应的所有数据文件所在目录的路径 |
| provide_test | Boolean | 需求方是否提供了验证数据以供自动验收 ｜
| entrust_intermediary | Boolean | 该任务是否交由中介进行分发 |
| intermediary_id | Integer | 被委托的中介的 id |
| intermediary_commission | Integer | 委托中介的佣金数额 |

<div class="footnotes">

    [1] 共有 6 种有效任务状态：编辑中（1），招募中（2），进行中（3），已完成（4），暂时僵化（5），待审核（6）。


    [2] 共支持八种任务类型：文本分类（1），图片分类（2），音频理解（3），视频理解（4），图片目标检测（5），骨骼打点（6），实体关系三元组（7），审核（8）。


    [3] 该列表中每一项都对应着一个标注方的进展，具体来说，记录了标注方 id，该标注方接受任务的时间，该标注方已经完成的题目数量，该标注方标注结果的验收状态，该标注方标注结果的自动验收正确率，该标注方标注结果的人工验收记录。
</div>

## 公告模型
| 字段 | 类型 | 用途 |
| --- | --- | --- |
| id | BigAuto | 公告唯一 id，按照发布顺序分配 |
| sender | Integer | 公告发布者 id |
| title | Char | 公告标题 |
| content | Text | 公告内容 |
| publish_time | Char | 公告发布时间 |

## 银行卡模型
| 字段 | 类型 | 用途 |
| --- | --- | --- |
| number | Char | 银行卡号 |
| password | Char | 银行卡密码 |
| balance | Integer | 银行卡余额 |
| owners | List | 可使用该银行卡的用户的 id 列表 |

## 举报信息
| 字段 | 类型 | 用途 |
| --- | --- | --- |
| id | BigAuto | 举报信息唯一 id，按照发起顺序分配 |
| sender | Integer | 举报者 id |
| receiver | Integer | 被举报者 id |
| tid | Integer | 举报所涉任务 id |
| category | SmallInteger | 举报信息类型<sup id="fn1">[1]</sup> |
| status | SmallInteger | 举报受理状态<sup id="fn2">[2]</sup> |
| reason | Text | 举报理由 |
| credit_variance | Integer | 举报可能涉及的信用分调整数额 |
| sender_notified | Boolean | 举报发起人是否收到了关于举报受理结果的通知 |
| receiver_notified | Boolean | 举报成功后，被举报人是否收到了通知 |

<div class="footnotes">

    [1] 分为标注方举报需求方恶意审核（0）和需求方举报标注方恶意刷题（1）。

    [2] 根据举报的受理进度和结果分为待受理（0），举报成功（1），举报被驳回（2）。

</div>