## components/
组件

### AuditList.vue

管理员审核任务组件

**data**

| 变量名  | 功能           |
| -------- | -------------------- |
| `auditlist` | 展示管理员所有需要审核的任务 |


### DataPost.vue

标注方做任务组件

**methods**

| 方法名  |参数| 功能           |
| -------- | -------- | -------------------- |
| `getdata` |无| 获取该题信息 |
| `getcontent` |无| 获取该题所有文字信息，用于文字预览 |
| `submit` |无| 提交该题结果 |

### Detective.vue

图片框选任务的辅助组件，用于显示所有框选结果

### FilePost.vue

需求方上传文件组件

**methods**

| 方法名  |参数| 功能           |
| -------- | -------- | -------------------- |
| `submit` |无| 提交文件并提示上传速度 |

### HeaderCard.vue

登录/注册界面的顶框

### LeaderBoard.vue

排行榜组件

**methods**

| 方法名  |参数| 功能           |
| -------- | -------- | -------------------- |
| `getleaderList` |无| 获取前十名标注方 |

### Loading.vue

用于在卡顿时显示“loading...”的组件

### LoadingFile.vue

限制不同会员不同上传文件速度的组件

### LoginCard.vue

注册/登录组件

**data**

| 变量名  | 功能           |
| -------- | -------------------- |
| `validatePass1` | 限制注册密码的安全性 |
| `validatePass2` | 用于确认密码的正确性 |

**methods**

| 方法名  |参数| 功能           |
| -------- | -------- | -------------------- |
| `submit` |无| 实现注册/登录，并对密码进行sha256加密 |

### Mediary.vue

中介组件

### PreferForm.vue

标注方对于新接收任务筛选的组件

**methods**

| 方法名  |参数| 功能           |
| -------- | -------- | -------------------- |
| `getTasksList` |无| 获取新的任务并根据preferform进行过滤 |
| `accept` |无| 接受任务 |
| `reject` |无| 拒绝任务 |

### ReportList.vue

举报组件

**data**

| 变量名  | 功能           |
| -------- | -------------------- |
| `reportlist` | 举报信息 |

### TaskList.vue

需求方/标注方查看任务组件

**data**

| 变量名  | 功能           |
| -------- | -------------------- |
| `goodslist` | 展示需求方创建/标注方接受的所有任务 |

### TaskPost.vue

需求方上传任务信息组件（流程是先上传任务信息再上传文件），用于创建/修改任务。

### TaskPresent.vue

需求方审核标注结果的组件

### UpperFrame.vue

顶框组件

### ViewList.vue

超管组件

**methods**

| 方法名  |参数| 功能           |
| -------- | -------- | -------------------- |
| `newcard` |无| 创建银行卡 |
| `deletecard` |无| 删除银行卡 |
| `password` |无| 修改银行卡密码 |
| `deposit` |无| 修改银行卡余额 |
| `notice` |无| 发布公告 |
| `deletenotice` |无| 根据ID删除公告 |

### components/TaskContent
不同任务模板的组件

#### Audio.vue

#### Censor.vue

#### Detect.vue

#### Image.vue

#### index.vue

#### Keypoint.vue

#### Text.vue

#### Triple.vue

#### Video.vue

#### components/TaskContent/partial

##### Anno.vue

## pages/
页面

### index.vue

默认位于登录界面

### pages/users

#### pages/users/demander

#### pages/users/intermediary

#### pages/users/manager

#### pages/users/super_manager

#### pages/users/worker

#### info.vue

#### leaderboard.vue

#### login.vue

#### password_reset.vue

#### register.vue

#### report.vue
