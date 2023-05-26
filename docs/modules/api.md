# api

> v1.0.0

Base URLs:

* <a href="http://prod-cn.your-api-server.com">Prod Env: http://prod-cn.your-api-server.com</a>

# 用户接口

## POST 用户注册

POST /users/register

注册用户。
需求方注册后需要管理员审核权限，管理员注册后需要需求方审核权限。
发送给后端的密码应当为SHA256加密后的密码。
填写邀请码可以让邀请码所有者获得1000积分，邀请码由6位大写字母/数字组成。
邮箱可以为空。

> Body 请求参数

```json
{
  "username": "傅秀英",
  "password": "10A6E6CC8311A3E2BCC09BF6C199ADECD5DD59408C343E926B129C4914F3CB01",
  "role": 3,
  "check_password": 1,
  "email_address": "fuxy21@mails.tsinghua.edu.cn",
  "invitation_code": "ABC123"
}
```

### 请求参数

| 名称              | 位置 | 类型    | 必选 | 说明                                                     |
| ----------------- | ---- | ------- | ---- | -------------------------------------------------------- |
| body              | body | object  | 否   | none                                                     |
| » username        | body | string  | 是   | 长度不超过 40，只能包含大小写字母/数字                   |
| » password        | body | string  | 是   | SHA 256 加密后的密码，长度为 64 的十六进制串（字母大写） |
| » role            | body | integer | 是   | 只能是 2 (管理员），3（需求方）或 4（标注者）            |
| » invitation_code | body | string  | 否   | 6位邀请码，邀请方可获得积分奖励                          |
| » email_address   | body | string  | 否   | none                                                     |

> 返回示例

> 请求成功

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "user": {
      "username": "钱静",
      "id": 233,
      "role": 3,
      "bankcard_number": "1234567890123456789"
    }
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明     | 数据模型 |
| ------ | ------------------------------------------------------- | -------- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | 请求成功 | Inline   |

### 返回数据结构

状态码 **200**

| 名称         | 类型    | 必选 | 约束 | 中文名 | 说明                                                         |
| ------------ | ------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code       | integer | true | none |        | none                                                         |
| » message    | string  | true | none |        | "success"                                                    |
| » data       | object  | true | none |        | none                                                         |
| »» user      | object  | true | none |        | none                                                         |
| »»» username | string  | true | none |        | 用户名                                                       |
| »»» id       | integer | true | none |        | 用户id                                                       |
| »»» role     | integer | true | none |        | 用户角色，1为超级管理员，2为管理员，3为需求方，4为标注方，5为中介 |
| »»» email    | string  | true | none |        | 用户邮箱                                                     |

## POST 用户登录

POST /users/login

用户登录。后端会利用会话（session）保存登录信息。
用户名和密码均正确且用户未被封禁则登陆成功。

> Body 请求参数

```json
{
  "username": "宋刚",
  "password": "10A6E6CC8311A3E2BCC09BF6C199ADECD5DD59408C343E926B129C4914F3CB01"
}
```

### 请求参数

| 名称       | 位置 | 类型   | 必选 | 说明                        |
| ---------- | ---- | ------ | ---- | --------------------------- |
| body       | body | object | 否   | none                        |
| » username | body | string | 是   | 同 register API 的 username |
| » password | body | string | 是   | 同 register API 的 username |

> 返回示例

> 请求成功

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "user": {
      "username": "赖军",
      "id": 83,
      "role": 5,
      "credits": 100,
      "points": 10000,
      "bankcard_number": "5387685970351591619",
      "invitation_code": "123ABC",
      "email": "z.bbwry@yuxfc.gov"
    }
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明     | 数据模型 |
| ------ | ------------------------------------------------------- | -------- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | 请求成功 | Inline   |

### 返回数据结构

状态码 **200**

| 名称                | 类型    | 必选 | 约束 | 中文名 | 说明                                                         |
| ------------------- | ------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code              | integer | true | none |        | none                                                         |
| » message           | string  | true | none |        | none                                                         |
| » data              | object  | true | none |        | none                                                         |
| »» user             | object  | true | none |        | none                                                         |
| »»» username        | string  | true | none |        | 用户名                                                       |
| »»» id              | integer | true | none |        | 用户id                                                       |
| »»» role            | integer | true | none |        | 用户角色，1为超级管理员，2为管理员，3为需求方，4为标注方，5为中介 |
| »»» credits         | integer | true | none |        | 用户信用分                                                   |
| »»» points          | integer | true | none |        | 用户积分                                                     |
| »»» bankcard_number | string  | true | none |        | 银行卡号，为19位数字                                         |
| »»» invitation_code | string  | true | none |        | 邀请码，6位，由大写字母和数字构成                            |
| »»» email           | string  | true | none |        | 用户邮箱                                                     |

## POST 用户登出

POST /users/logout

用户退出登录。

> 返回示例

> OK

```json
{
  "message": "sucess",
  "data": {},
  "code": 200
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明      |
| --------- | ------- | ---- | ---- | ------ | --------- |
| » code    | integer | true | none |        | none      |
| » message | string  | true | none |        | "success" |
| » data    | object  | true | none |        | {}        |

## POST 充值积分

POST /users/topupPoints

用户用银行账户中的存款充值积分。

> Body 请求参数

```json
{
  "topup_amount": 1000
}
```

### 请求参数

| 名称           | 位置 | 类型    | 必选 | 说明                                      |
| -------------- | ---- | ------- | ---- | ----------------------------------------- |
| body           | body | object  | 否   | none                                      |
| » topup_amount | body | integer | 是   | 充值积分数量，以积分为单位，1块钱=100积分 |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "points": 5000,
    "bank_balance": 100
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称            | 类型    | 必选 | 约束 | 中文名 | 说明                   |
| --------------- | ------- | ---- | ---- | ------ | ---------------------- |
| » code          | integer | true | none |        | none                   |
| » message       | string  | true | none |        | none                   |
| » data          | object  | true | none |        | none                   |
| »» points       | integer | true | none |        | 用户目前的积分余额     |
| »» bank_balance | integer | true | none |        | 用户目前的银行账户余额 |

## POST 积分提现

POST /users/withdrawPoints

将积分提现到银行账户。

> Body 请求参数

```json
{
  "withdraw_amount": 1000
}
```

### 请求参数

| 名称              | 位置 | 类型    | 必选 | 说明                     |
| ----------------- | ---- | ------- | ---- | ------------------------ |
| body              | body | object  | 否   | none                     |
| » withdraw_amount | body | integer | 是   | 提现金额，以人民币为单位 |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "points": 2000,
    "bank_balance": 233
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称            | 类型    | 必选 | 约束 | 中文名 | 说明                   |
| --------------- | ------- | ---- | ---- | ------ | ---------------------- |
| » code          | integer | true | none |        | none                   |
| » message       | string  | true | none |        | none                   |
| » data          | object  | true | none |        | none                   |
| »» points       | integer | true | none |        | 用户目前的积分余额     |
| »» bank_balance | integer | true | none |        | 用户目前的银行账户余额 |

## GET 获取排行榜

GET /users/getlLeaderboard

返回标注方依据积分排名的leaderboard

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "leaderboard": [
      {
        "points_earned_by_laboring": 10000,
        "username": "somebody",
        "id": 105,
        "bankcard_number": "3596581785638447645",
        "invitation_code": "HUBIX2",
        "email": "m.uoqjff@hafoxxznt.ci"
      },
      {
        "points_earned_by_laboring": 9000,
        "username": "someone_else",
        "id": 106,
        "bankcard_number": "8455881854477894453",
        "invitation_code": "L2T292",
        "email": "o.gsxi@ynchtneuuo.tel"
      },
      {
        "points_earned_by_laboring": 8000,
        "username": "nobody",
        "id": 107,
        "bankcard_number": "8684318232263114241",
        "invitation_code": "2262V2",
        "email": "x.jlpa@mvotri.cv"
      },
      {
        "points_earned_by_laboring": 7000,
        "username": "no_one",
        "id": 108,
        "bankcard_number": "0741719996221556757",
        "invitation_code": "2F8N02",
        "email": "x.srecp@wccruk.zr"
      }
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                          | 类型     | 必选 | 约束 | 中文名 | 说明                              |
| ----------------------------- | -------- | ---- | ---- | ------ | --------------------------------- |
| » code                        | integer  | true | none |        | none                              |
| » message                     | string   | true | none |        | none                              |
| » data                        | object   | true | none |        | none                              |
| »» leaderboard                | [object] | true | none |        | none                              |
| »»» username                  | string   | true | none |        | 用户名                            |
| »»» id                        | integer  | true | none |        | 用户id                            |
| »»» bankcard_number           | string   | true | none |        | 银行卡号，为19位数字              |
| »»» invitation_code           | string   | true | none |        | 邀请码，6位，由大写字母和数字构成 |
| »»» email                     | string   | true | none |        | 用户邮箱                          |
| »»» points_earned_by_laboring | integer  | true | none |        | 用户做任务获得的积分              |

## GET 查看 VIP 状态

GET /users/viewVIP

获取当前用户（需求方）的VIP等级和过期时间。
VIP 等级 1 为白银，2 为黄金，3 为铂金。VIP 等级为 0 表示当前用户不是会员。

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "vip_status": {
    "vip_level": 3,
    "expiration_time": "2023-09-28 07:29:39"
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                | 类型    | 必选 | 约束 | 中文名 | 说明                |
| ------------------- | ------- | ---- | ---- | ------ | ------------------- |
| » code              | integer | true | none |        | none                |
| » message           | string  | true | none |        | none                |
| » data              | object  | true | none |        | none                |
| »» vip_status       | object  | true | none |        | none                |
| »»» vip_level       | integer | true | none |        | 当前的 VIP 等级     |
| »»» expiration_time | string  | true | none |        | 当前 VIP 的过期时间 |

## POST 升级 VIP

POST /users/upgradeVIP

升级 VIP 等级（vip 等级 1 为白银，2 为黄金，3 为铂金），时长可选 15s 或 30s 或 60s 或 180 秒 或 3600 秒。
如果当前已经是 VIP，则只能申请升级到当前等级（延长时间）或更高等级，且申请的时间必须长于当前VIP的过期时间。
花费积分 = 等级 * 时间长度（秒），如果已经是 VIP 对于重叠部分会自动计算减免。

> Body 请求参数

```json
{
  "vip_level": 3,
  "duration": 15
}
```

### 请求参数

| 名称        | 位置 | 类型    | 必选 | 说明                                        |
| ----------- | ---- | ------- | ---- | ------------------------------------------- |
| body        | body | object  | 否   | none                                        |
| » vip_level | body | integer | 是   | 想要升级的 VIP 等级，可选为 1,2,3           |
| » duration  | body | integer | 是   | 想要充值的 VIP 时长，可选 15,30,60,180,3600 |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "vip_status": {
      "vip_level": 3,
      "expiration_time": "2016-07-12 11:48:20",
      "cost": 84,
      "points_left": 10000
    }
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                | 类型    | 必选 | 约束 | 中文名 | 说明                  |
| ------------------- | ------- | ---- | ---- | ------ | --------------------- |
| » code              | integer | true | none |        | none                  |
| » message           | string  | true | none |        | none                  |
| » data              | object  | true | none |        | none                  |
| »» vip_status       | object  | true | none |        | none                  |
| »»» vip_level       | integer | true | none |        | 升级后的 VIP 等级     |
| »»» expiration_time | string  | true | none |        | 升级后 VIP 的过期时间 |
| »»» cost            | integer | true | none |        | 升级所花费的积分5     |
| »»» points_left     | string  | true | none |        | 升级后的积分余额      |

## POST 获取 VIP 令牌

POST /users/getVIPToken

在实现了 VIP 限流功能后，前端每次要发送上传文件的请求之前，都必须先调用这个接口；
这个接口会根据用户的 VIP 等级赋予用户一个令牌，具有 VIP 令牌的用户才能调用相应速度的传输接口；
在一次文件传输后令牌会被销毁，因此 ** 每次 ** 调用传输文件的接口前（即使用户不是VIP），都需要先调用此接口。
这个接口的引入是由于本项目中支持的 VIP 时长较短，可能会出现文件上传过程中用户 VIP 等级发生变化的情况。在开始上传文件之前调用这个请求可以使后端确定文件上传过程刚刚开始时用户的 VIP 等级，并采取相应的速率限制。

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "vip_status": {
      "vip_level": 3,
      "expiration_time": "2020-11-14 10:13:53"
    }
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                | 类型    | 必选 | 约束 | 中文名 | 说明                    |
| ------------------- | ------- | ---- | ---- | ------ | ----------------------- |
| » code              | integer | true | none |        | none                    |
| » message           | string  | true | none |        | none                    |
| » data              | object  | true | none |        | none                    |
| »» vip_status       | object  | true | none |        | none                    |
| »»» vip_level       | integer | true | none |        | 当前的 VIP 等级         |
| »»» expiration_time | string  | true | none |        | 当前 VIP 状态的过期时间 |

## POST 获取邮箱验证码

POST /users/getCodeEmail

给出用户名，如果该用户注册时绑定了邮箱，则会向该邮箱发送验证码。
这个验证码可以用于调用 changePassword 接口修改密码，这个邮箱验证码会在 10 分钟后失效。
这个接口可以在用户没有登陆的情况下被调用。

> Body 请求参数

```json
{
  "username": "汪丽"
}
```

### 请求参数

| 名称       | 位置 | 类型   | 必选 | 说明                     |
| ---------- | ---- | ------ | ---- | ------------------------ |
| body       | body | object | 否   | none                     |
| » username | body | string | 是   | 想要获取验证码的用户名。 |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## POST 修改密码

POST /users/changePassword

使用邮箱验证码修改密码。
与注册接口相同，这个请求需要传入使用 SHA 256 加密过的密码。

> Body 请求参数

```json
{
  "username": "丁丽",
  "code": 123456,
  "password": "10A6E6CC8311A3E2BCC09BF6C199ADECD5DD59408C343E926B129C4914F3CB01"
}
```

### 请求参数

| 名称       | 位置 | 类型   | 必选 | 说明                         |
| ---------- | ---- | ------ | ---- | ---------------------------- |
| body       | body | object | 否   | none                         |
| » username | body | string | 是   | 将要修改其密码的用户的用户名 |
| » code     | body | string | 是   | 邮箱验证码                   |
| » password | body | string | 是   | SHA 256 加密后的新密码       |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

# 用户接口/管理接口

## POST 修改状态

POST /users/changeStatus

作为超管登录状态下，可以修改管理员的可登录状态；
作为管理员登录状态下，可修改标注方或需求方的可登录状态。

> Body 请求参数

```json
{
  "username": "尹军",
  "status": 0
}
```

### 请求参数

| 名称       | 位置 | 类型    | 必选 | 说明                                         |
| ---------- | ---- | ------- | ---- | -------------------------------------------- |
| body       | body | object  | 否   | none                                         |
| » username | body | string  | 是   | 要修改权限的用户的用户名                     |
| » status   | body | integer | 是   | 0 表示将其改为不可登录，1 表示将其改为可登录 |

> 返回示例

> OK

```json
{
  "code": 200,
  "data": {},
  "message": "success"
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明      |
| --------- | ------- | ---- | ---- | ------ | --------- |
| » code    | integer | true | none |        | none      |
| » message | string  | true | none |        | "success" |
| » data    | object  | true | none |        | none      |

## GET 查看用户

GET /users/staffViewUsers

管理员调用，查看除超管、管理员之外的用户信息。

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "users": [
      {
        "username": "$(!W87*XvC!^&zovo@Es@G#P8Lw1CZz^",
        "id": 160,
        "role": 5,
        "banned": true,
        "credits": 67,
        "points": 34500,
        "bankcard_number": "6439825358037648074",
        "invitation_code": "88227Q",
        "email": "s.ihjtmqpgqf@sady.mh",
        "extra_info": "velit deserunt ullamco reprehenderit sit"
      },
      {
        "username": "QA@YRniM[^V",
        "id": 161,
        "role": 1,
        "banned": true,
        "credits": 10,
        "points": 400,
        "bankcard_number": "6101797524891772058",
        "invitation_code": "MH2422",
        "email": "o.suwcjtebm@bwodv.pw",
        "extra_info": "magna proident esse"
      },
      {
        "username": "W$[9joStc)0!!5&#fX[Kz7CDkSzs$(^*x0CbwBp",
        "id": 162,
        "role": 1,
        "banned": false,
        "credits": 100,
        "points": 1000,
        "bankcard_number": "7184765715447718446",
        "invitation_code": "F2252P",
        "email": "c.uuctnenp@xzhmw.ve",
        "extra_info": "eu qui et enim"
      }
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                | 类型     | 必选 | 约束 | 中文名 | 说明                                                         |
| ------------------- | -------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code              | integer  | true | none |        | none                                                         |
| » message           | string   | true | none |        | none                                                         |
| » data              | object   | true | none |        | none                                                         |
| »» users            | [object] | true | none |        | none                                                         |
| »»» username        | string   | true | none |        | 用户名                                                       |
| »»» id              | integer  | true | none |        | 用户id                                                       |
| »»» role            | integer  | true | none |        | 用户角色，1为超级管理员，2为管理员，3为需求方，4为标注方，5为中介 |
| »»» banned          | boolean  | true | none |        | 是否被封禁                                                   |
| »»» credits         | integer  | true | none |        | 用户信用分                                                   |
| »»» points          | integer  | true | none |        | 用户积分                                                     |
| »»» bankcard_number | string   | true | none |        | 银行卡号，为19位数字                                         |
| »»» invitation_code | string   | true | none |        | 邀请码，6位，由大写字母和数字构成                            |
| »»» email           | string   | true | none |        | 用户邮箱                                                     |
| »»» extra_info      | string   | true | none |        | 额外信息                                                     |

## GET 查看管理员

GET /users/rootViewStaffs

超管调用，查看当前的所有管理员。

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "username": "55SXQ[&MFB6s1Gg1e",
    "id": 164,
    "role": 2,
    "banned": true,
    "extra_info": "minim ea aute Ut"
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称          | 类型    | 必选 | 约束 | 中文名 | 说明                                                         |
| ------------- | ------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code        | integer | true | none |        | none                                                         |
| » message     | string  | true | none |        | none                                                         |
| » data        | object  | true | none |        | none                                                         |
| »» username   | string  | true | none |        | 用户名                                                       |
| »» id         | integer | true | none |        | 用户id                                                       |
| »» role       | integer | true | none |        | 用户角色，1为超级管理员，2为管理员，3为需求方，4为标注方，5为中介 |
| »» banned     | boolean | true | none |        | 是否被封禁                                                   |
| »» extra_info | string  | true | none |        | 额外信息                                                     |

# 任务接口/需求方接口

## POST 发布任务

POST /tasks/post

需求方发布一个任务。发布任务时暂时不需要上传数据。

> Body 请求参数

```json
{
  "name": "string",
  "duration": 0,
  "time_limit": 0,
  "category": 0,
  "assign_num": 0,
  "reward": 0,
  "provide_test": true
}
```

### 请求参数

| 名称           | 位置 | 类型    | 必选 | 说明                                                         |
| -------------- | ---- | ------- | ---- | ------------------------------------------------------------ |
| body           | body | object  | 否   | none                                                         |
| » name         | body | string  | 是   | 任务名称                                                     |
| » duration     | body | integer | 是   | 容许的标注方总用时                                           |
| » time_limit   | body | integer | 是   | 单个任务时间限制                                             |
| » category     | body | integer | 是   | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| » assign_num   | body | integer | 是   | 需要分配的标注方的数量                                       |
| » reward       | body | integer | 是   | 每道题奖励的积分量                                           |
| » provide_test | body | boolean | 是   | 需求方是否提供测试集                                         |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "task": {
      "id": 55,
      "provide_test": false,
      "category": 23
    }
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称             | 类型    | 必选 | 约束 | 中文名 | 说明       |
| ---------------- | ------- | ---- | ---- | ------ | ---------- |
| » code           | integer | true | none |        | none       |
| » message        | string  | true | none |        | none       |
| » data           | object  | true | none |        | none       |
| »» task          | object  | true | none |        | 发布的任务 |
| »»» id           | integer | true | none |        | none       |
| »»» provide_test | boolean | true | none |        | none       |
| »»» category     | integer | true | none |        | none       |

## GET 查看任务

GET /tasks/demanderView

需求方查看自己已发布过的所有任务。

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "tasks": [
      {
        "name": "MRReSV]r^n(oCsoOlkdC^",
        "id": 3,
        "status": 1,
        "created_time": "1982-07-22 AM 04:40:57",
        "duration": 825614353822581,
        "time_limit": 2705033488284871,
        "category": 8,
        "distribute_user_list": [
          23820177461907,
          85669663267541,
          5853007919933088,
          4054645643606308,
          2410956621780146
        ],
        "dataset_size": 2144586012872006,
        "assign_num": 2653746003516297,
        "reward": 2503287609866157,
        "assigner": 8334204459904320,
        "thread_list": [
          {
            "id": 3780908029594907,
            "accept_time": "1979-04-07 AM 03:02:33",
            "completed_num": 1062620560657561,
            "review_status": 1,
            "auto_check_score": 78.596116112493
          },
          {
            "id": 3538642686930996,
            "accept_time": "1971-10-20 AM 01:03:13",
            "completed_num": 6074081191968778,
            "review_status": 1,
            "auto_check_score": 58.4448623629
          }
        ],
        "credit_lower_bound": 5167146581080010,
        "rejecter_list": [
          3524998700419419
        ],
        "provide_test": true,
        "entrust_intermediary": false,
        "intermediary_id": 1864239362231245,
        "intermediary_commmission": 22293539029191
      },
      {
        "name": "WdRx5j^4v5NjZ",
        "id": 4,
        "status": 1,
        "created_time": "1999-05-20 PM 20:27:44",
        "duration": 3236164862457449,
        "time_limit": 5221311406236357,
        "category": 7,
        "distribute_user_list": [
          4485216414451591,
          5615807230851998
        ],
        "dataset_size": 2676471278494081,
        "assign_num": 6999997158571303,
        "reward": 3207624648504277,
        "assigner": 6317352988694713,
        "thread_list": [
          {
            "id": 5849531025461947,
            "accept_time": "2011-10-13 PM 17:56:22",
            "completed_num": 8563745295849608,
            "review_status": 2,
            "auto_check_score": 2.72
          }
        ],
        "credit_lower_bound": 8563288898871696,
        "rejecter_list": [
          4859103555516811,
          7855720259806525,
          7232480617854657,
          2755094737008330
        ],
        "provide_test": true,
        "entrust_intermediary": false,
        "intermediary_id": 3674804242180375,
        "intermediary_commmission": 2504441400713693
      },
      {
        "name": "69y*t$rSdZ5z",
        "id": 5,
        "status": 4,
        "created_time": "1992-07-08 PM 16:06:46",
        "duration": 6422688793109225,
        "time_limit": 8037750677919019,
        "category": 2,
        "distribute_user_list": [
          6983570053894276,
          6803639226264244
        ],
        "dataset_size": 5852564535582850,
        "assign_num": 5153361557416463,
        "reward": 2408668161096371,
        "assigner": 6131934325752383,
        "thread_list": [
          {
            "id": 3434498548167971,
            "accept_time": "1970-10-13 AM 07:58:42",
            "completed_num": 5371210224232740,
            "review_status": 0,
            "auto_check_score": 33.88683
          },
          {
            "id": 2613266977785567,
            "accept_time": "2022-03-18 PM 14:26:04",
            "completed_num": 1423874216228523,
            "review_status": 4,
            "auto_check_score": 12.265422
          }
        ],
        "credit_lower_bound": 2073794841375205,
        "rejecter_list": [
          6824715607738267,
          3854198774020343,
          3454026265992154,
          3585284394462751,
          5834126378971033
        ],
        "provide_test": false,
        "entrust_intermediary": false,
        "intermediary_id": 5285236472326864,
        "intermediary_commmission": 4645254742797550
      },
      {
        "name": "5]jjt1Ktri$)Rwbq8Pzx^y5AEy4HZdp@JAcrjN",
        "id": 6,
        "status": 2,
        "created_time": "1988-05-30 PM 20:10:24",
        "duration": 1114932786635661,
        "time_limit": 3958605480252036,
        "category": 6,
        "distribute_user_list": [
          4494720225687081,
          5422166156850104,
          2771112317942200
        ],
        "dataset_size": 7499348060452210,
        "assign_num": 2740985045528495,
        "reward": 936401529992327,
        "assigner": 418057630232748,
        "thread_list": [
          {
            "id": 4526028651676443,
            "accept_time": "2018-10-04 PM 15:33:59",
            "completed_num": 5299626912561515,
            "review_status": 3,
            "auto_check_score": 23.854311014112607
          },
          {
            "id": 3294618151851672,
            "accept_time": "1976-02-20 PM 13:36:35",
            "completed_num": 8917431073142886,
            "review_status": 3,
            "auto_check_score": 95.6848964741
          },
          {
            "id": 2502849811595801,
            "accept_time": "2008-10-30 PM 16:43:45",
            "completed_num": 3583317674802280,
            "review_status": 2,
            "auto_check_score": 48.06421246326
          },
          {
            "id": 2724814633271508,
            "accept_time": "1982-11-01 AM 05:37:27",
            "completed_num": 201451791668832,
            "review_status": 1,
            "auto_check_score": 36.81625375
          },
          {
            "id": 8653250864088714,
            "accept_time": "1994-12-21 PM 20:57:43",
            "completed_num": 5988946234077561,
            "review_status": 2,
            "auto_check_score": 96.57262408863
          }
        ],
        "credit_lower_bound": 3606423123767497,
        "rejecter_list": [
          722343831767437,
          1758224028997478,
          6721550228770531,
          6993445824804303,
          3850762633993385
        ],
        "provide_test": false,
        "entrust_intermediary": false,
        "intermediary_id": 6899987430815988,
        "intermediary_commmission": 1583050045221063
      }
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                         | 类型      | 必选 | 约束 | 中文名 | 说明                                                         |
| ---------------------------- | --------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code                       | integer   | true | none |        | none                                                         |
| » message                    | string    | true | none |        | none                                                         |
| » data                       | object    | true | none |        | none                                                         |
| »» tasks                     | [object]  | true | none |        | 用户已发布过的所有任务                                       |
| »»» name                     | string    | true | none |        | 任务名称                                                     |
| »»» id                       | integer   | true | none |        | 任务id                                                       |
| »»» status                   | integer   | true | none |        | 任务状态，0-6分别表示未定义、管理员审核中、招募中、进行中、已完成、无法分配、审核结果中 |
| »»» created_time             | string    | true | none |        | 任务创建时间                                                 |
| »»» duration                 | integer   | true | none |        | 容许的标注方总用时                                           |
| »»» time_limit               | integer   | true | none |        | 单个任务时间限制                                             |
| »»» category                 | integer   | true | none |        | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| »»» distribute_user_list     | [integer] | true | none |        | 任务分配用户列表                                             |
| »»» dataset_size             | integer   | true | none |        | 数据集中条目的个数                                           |
| »»» assign_num               | integer   | true | none |        | 需要分配的标注方的数量                                       |
| »»» reward                   | integer   | true | none |        | 每道题奖励的积分量                                           |
| »»» assigner                 | integer   | true | none |        | 任务发布者id                                                 |
| »»» thread_list              | [object]  | true | none |        | 每一项代表一个标注者的进度等信息                             |
| »»»» id                      | integer   | true | none |        | 标注者id                                                     |
| »»»» accept_time             | string    | true | none |        | 标注者接受任务的时间                                         |
| »»»» completed_num           | integer   | true | none |        | 标注者已完成的条目数（用于向需求方显示进度以及确定标注方需要标注的下一条数据） |
| »»»» review_status           | integer   | true | none |        | 0-未完成；1-待审核；2-合格; 3-不合格; 4-超时                 |
| »»»» auto_check_score        | number    | true | none |        | 浮点数，自动检查得到的正确率                                 |
| »»» credit_lower_bound       | integer   | true | none |        | 任务分配需求方的信用分下限                                   |
| »»» rejecter_list            | [integer] | true | none |        | 已拒绝此任务的标注方列表，分发是不会再分发给这些用户         |
| »»» provide_test             | boolean   | true | none |        | 需求方是否提供测试集                                         |
| »»» entrust_intermediary     | boolean   | true | none |        | 是否委托中介                                                 |
| »»» intermediary_id          | integer   | true | none |        | 委托的中介id                                                 |
| »»» intermediary_commmission | integer   | true | none |        | 中介委托费，以积分为单位                                     |

## POST 修改任务

POST /tasks/modify/{TID}

需求方自己已发布但尚未确认的任务。
请求体与发布任务完全相同。
返回体重包含 need_reload_data 信息，需要对需求方用户进行反馈，提示其是否需要重新上传数据（在 provide_test 或 category 发生变化时，会需要重新上传数据）。

> Body 请求参数

```json
{
  "name": "string",
  "status": 0,
  "duration": 0,
  "time_limit": 0,
  "category": 0,
  "distribute_user_list": [
    0
  ],
  "assign_num": 0,
  "reward": 0,
  "credit_lower_bound": 0,
  "provide_test": true
}
```

### 请求参数

| 名称                   | 位置 | 类型      | 必选 | 说明                                                         |
| ---------------------- | ---- | --------- | ---- | ------------------------------------------------------------ |
| TID                    | path | integer   | 是   | none                                                         |
| body                   | body | object    | 否   | none                                                         |
| » name                 | body | string    | 是   | 任务名称                                                     |
| » status               | body | integer   | 是   | 任务状态，0-6分别表示未定义、管理员审核中、招募中、进行中、已完成、无法分配、审核结果中 |
| » duration             | body | integer   | 是   | 容许的标注方总用时                                           |
| » time_limit           | body | integer   | 是   | 单个任务时间限制                                             |
| » category             | body | integer   | 是   | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| » distribute_user_list | body | [integer] | 是   | 任务分配用户列表                                             |
| » assign_num           | body | integer   | 是   | 需要分配的标注方的数量                                       |
| » reward               | body | integer   | 是   | 每道题奖励的积分量                                           |
| » credit_lower_bound   | body | integer   | 是   | 任务分配需求方的信用分下限                                   |
| » provide_test         | body | boolean   | 是   | 需求方是否提供测试集                                         |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "need_reload_data": false
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                | 类型    | 必选 | 约束 | 中文名 | 说明                                 |
| ------------------- | ------- | ---- | ---- | ------ | ------------------------------------ |
| » code              | integer | true | none |        | none                                 |
| » message           | string  | true | none |        | none                                 |
| » data              | object  | true | none |        | none                                 |
| »» need_reload_data | boolean | true | none |        | 对任务进行修改后是否需要重新上传文件 |

## POST 删除任务

POST /tasks/delete/{tid}

需求方删除自己发布过但尚未删除的一个任务。

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明              |
| ---- | ---- | ------- | ---- | ----------------- |
| tid  | path | integer | 是   | 要删除的任务的 ID |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## POST 确认任务

POST /tasks/confirm/{tid}

需求方确认将已经创建、并完成了文件上传的任务进行发布，并支付积分，此后不能再删除、更改这个任务。
发布的任务进入待审核状态，被管理员审核通过后即被分发。
参数中的 credit_lower_bound 给出可以接受该任务的标注方的信用分下限，可以为空（视为默认值90）。
如果初始时按这个信用分下限就找不到足够的用户，则会发返回一个错误，提示用户修改 lower_bound 后再尝试确认任务。

> Body 请求参数

```json
{
  "credit_lower_bound": 0,
  "entrust_intermediary": true,
  "intermediary_commission": 0
}
```

### 请求参数

| 名称                      | 位置 | 类型    | 必选 | 说明                                                         |
| ------------------------- | ---- | ------- | ---- | ------------------------------------------------------------ |
| tid                       | path | integer | 是   | 要确认的任务 ID                                              |
| body                      | body | object  | 否   | none                                                         |
| » credit_lower_bound      | body | integer | 否   | 表示分发时对 worker credit 的约束，请求体可以不含这个字段，视为默认值90 |
| » entrust_intermediary    | body | boolean | 否   | 是否中介分发，不含此字段则默认为假                           |
| » intermediary_commission | body | integer | 否   | 如果委托中介的话必须包含此字段，且必须大于0                  |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## GET 查看任务详情

GET /tasks/demanderView/{tid}

需求方查看自己发布过的某一个任务的详细情况。

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明              |
| ---- | ---- | ------- | ---- | ----------------- |
| tid  | path | integer | 是   | 要查看的任务的 ID |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "task": {
      "name": "56)f",
      "id": 7,
      "status": 2,
      "created_time": "1982-08-14 PM 12:59:15",
      "duration": 4563076743911899,
      "time_limit": 284780734272451,
      "category": 7,
      "distribute_user_list": [
        8233927966424323,
        3219008495158348,
        4104011275009451
      ],
      "dataset_size": 7898193072721409,
      "assign_num": 5944026765598034,
      "reward": 2654726087828571,
      "assigner": 2325049726961067,
      "thread_list": [
        {
          "id": 8354504053583051,
          "accept_time": "1975-03-27 AM 08:01:08",
          "completed_num": 6583928603750900,
          "review_status": 1,
          "auto_check_score": 26.66271
        }
      ],
      "credit_lower_bound": 8055280839656601,
      "provide_test": true,
      "entrust_intermediary": true,
      "intermediary_id": 4533210602460329,
      "intermediary_commmission": 5018155413726342
    }
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                         | 类型      | 必选 | 约束 | 中文名 | 说明                                                         |
| ---------------------------- | --------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code                       | integer   | true | none |        | none                                                         |
| » message                    | string    | true | none |        | none                                                         |
| » data                       | object    | true | none |        | none                                                         |
| »» task                      | object    | true | none |        | none                                                         |
| »»» name                     | string    | true | none |        | 任务名称                                                     |
| »»» id                       | integer   | true | none |        | 任务id                                                       |
| »»» status                   | integer   | true | none |        | 任务状态，0-6分别表示未定义、管理员审核中、招募中、进行中、已完成、无法分配、审核结果中 |
| »»» created_time             | string    | true | none |        | 任务创建时间                                                 |
| »»» duration                 | integer   | true | none |        | 容许的标注方总用时                                           |
| »»» time_limit               | integer   | true | none |        | 单个任务时间限制                                             |
| »»» category                 | integer   | true | none |        | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| »»» distribute_user_list     | [integer] | true | none |        | 任务分配用户列表                                             |
| »»» dataset_size             | integer   | true | none |        | 数据集中条目的个数                                           |
| »»» assign_num               | integer   | true | none |        | 需要分配的标注方的数量                                       |
| »»» reward                   | integer   | true | none |        | 每道题奖励的积分量                                           |
| »»» assigner                 | integer   | true | none |        | 任务发布者id                                                 |
| »»» thread_list              | [object]  | true | none |        | 每一项代表一个标注者的进度等信息                             |
| »»»» id                      | integer   | true | none |        | 标注者id                                                     |
| »»»» accept_time             | string    | true | none |        | 标注者接受任务的时间                                         |
| »»»» completed_num           | integer   | true | none |        | 标注者已完成的条目数（用于向需求方显示进度以及确定标注方需要标注的下一条数据） |
| »»»» review_status           | integer   | true | none |        | 0-未完成；1-待审核；2-合格; 3-不合格; 4-超时                 |
| »»»» auto_check_score        | number    | true | none |        | 浮点数，自动检查得到的正确率                                 |
| »»» credit_lower_bound       | integer   | true | none |        | 任务分配需求方的信用分下限                                   |
| »»» provide_test             | boolean   | true | none |        | 需求方是否提供测试集                                         |
| »»» entrust_intermediary     | boolean   | true | none |        | 是否委托中介                                                 |
| »»» intermediary_id          | integer   | true | none |        | 委托的中介id                                                 |
| »»» intermediary_commmission | integer   | true | none |        | 中介委托费，以积分为单位                                     |

## GET 检查标注结果

GET /tasks/checkAnno/{tid}

需求方检查某个标注者的某条标注结果。前端可以使用这个接口实现需求方对标注结果的全量检查和抽样检查。

> Body 请求参数

```json
{}
```

### 请求参数

| 名称      | 位置  | 类型    | 必选 | 说明                    |
| --------- | ----- | ------- | ---- | ----------------------- |
| tid       | path  | integer | 是   | 要进行验收的任务 ID     |
| thread_id | query | integer | 否   | 要进行验收的线程 ID     |
| data_id   | query | integer | 否   | 验收中想要查看的题目 ID |
| body      | body  | object  | 否   | none                    |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "data": "dolor incididunt ex aliquip",
    "anno_id": 78,
    "lables": [
      "et",
      "elit dolore ipsum nostrud"
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称       | 类型     | 必选 | 约束 | 中文名 | 说明                   |
| ---------- | -------- | ---- | ---- | ------ | ---------------------- |
| » code     | integer  | true | none |        | none                   |
| » message  | string   | true | none |        | none                   |
| » data     | object   | true | none |        | none                   |
| »» data    | string   | true | none |        | 待标注数据             |
| »» anno_id | integer  | true | none |        | 标注结果（标签的序号） |
| »» lables  | [string] | true | none |        | 所有标签               |

## GET 下载标注结果

GET /tasks/downloadAnno/{tid}

下载指定的已完成任务的标注结果，需要在 query 串中给出整理标注结果的模式：
“ALL”：将各thread的数据全部返回，每行是每一个题目的所有标注结果；
“CLUSTER”：按最多数结果整理标注结果。
** 非分类任务只能使用 ALL 模式 **。
这个接口会根据需求方的 VIP 等级在 header 中添加限速信息。由前端的 nginx 进行处理。

> Body 请求参数

```json
{}
```

### 请求参数

| 名称 | 位置  | 类型    | 必选 | 说明                 |
| ---- | ----- | ------- | ---- | -------------------- |
| tid  | path  | integer | 是   | 要下载结果的任务 ID  |
| mode | query | string  | 否   | "ALL" 或者 "CLUSTER" |
| body | body  | object  | 否   | none                 |

> 返回示例

> OK

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

## POST 验收通过

POST /tasks/approveThread/{tid}

任务的发布者验收通过某个任务的某线程的标注结果。

> Body 请求参数

```json
{
  "thread_id": 0
}
```

### 请求参数

| 名称        | 位置 | 类型    | 必选 | 说明                 |
| ----------- | ---- | ------- | ---- | -------------------- |
| tid         | path | integer | 是   | 正在验收的任务       |
| body        | body | object  | 否   | none                 |
| » thread_id | body | integer | 是   | 验收通过的线程的序号 |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## GET 查看任务图片

GET /tasks/demanderGetImage/{tid}

对于含有图片数据的任务，需求方可以在进行验收时调用此接口查看指定的题目的图片数据从而判断标注质量。

### 请求参数

| 名称    | 位置  | 类型    | 必选 | 说明              |
| ------- | ----- | ------- | ---- | ----------------- |
| tid     | path  | integer | 是   | 验收的任务的 ID   |
| data_id | query | integer | 否   | 要查看的题目的 ID |

> 返回示例

> OK

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

## POST 调整信用分限制

POST /tasks/modifyCreditRestriction/{tid}

对于 recruiting 或 zombie 状态的任务，可以尝试修改信用分下限，放松标准从而提高快速完成分发的可能性。
zombie 任务在调用该请求后还会自动以新标准重新尝试招募标注方。

> Body 请求参数

```json
{
  "credit_lower_bound": 0
}
```

### 请求参数

| 名称                 | 位置 | 类型    | 必选 | 说明                                       |
| -------------------- | ---- | ------- | ---- | ------------------------------------------ |
| tid                  | path | integer | 是   | 要调整的任务的 ID                          |
| body                 | body | object  | 否   | none                                       |
| » credit_lower_bound | body | integer | 是   | 新的信用分下届限制，不能高于原有信用分下限 |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "task": {
      "name": "bxgK4PzzJC8(PL",
      "id": 8,
      "status": 0,
      "created_time": "2014-12-28 AM 09:42:37",
      "duration": 6658718017730165,
      "time_limit": 1330165536530636,
      "category": 7,
      "distribute_user_list": [
        1423580899498745,
        6851582615243077
      ],
      "dataset_size": 5665853943492229,
      "assign_num": 1647320215624315,
      "reward": 7787577383413952,
      "assigner": 1818033878502257,
      "thread_list": [
        {
          "id": 7099700181973988,
          "accept_time": "1971-08-25 PM 21:22:26",
          "completed_num": 6465115883381227,
          "review_status": 2,
          "auto_check_score": 81.97
        },
        {
          "id": 1176892633416868,
          "accept_time": "2018-01-30 AM 04:14:07",
          "completed_num": 8084972435304736,
          "review_status": 0,
          "auto_check_score": 46.2
        },
        {
          "id": 7453987700044862,
          "accept_time": "2016-09-20 PM 13:14:22",
          "completed_num": 3310197320024669,
          "review_status": 3,
          "auto_check_score": 74.512463
        },
        {
          "id": 4382208931038285,
          "accept_time": "2001-07-07 AM 08:42:19",
          "completed_num": 510378557356592,
          "review_status": 1,
          "auto_check_score": 9.085117068683878
        },
        {
          "id": 2015070507767010,
          "accept_time": "2015-07-18 PM 14:36:00",
          "completed_num": 332733899800160,
          "review_status": 3,
          "auto_check_score": 45.41575181826451
        }
      ],
      "credit_lower_bound": 5827061265648813,
      "provide_test": false,
      "entrust_intermediary": false,
      "intermediary_id": 286503222623984,
      "intermediary_commmission": 78416704173312
    }
  }
}
```

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "task": {
      "name": "bxgK4PzzJC8(PL",
      "id": 8,
      "status": 0,
      "created_time": "2014-12-28 AM 09:42:37",
      "duration": 6658718017730165,
      "time_limit": 1330165536530636,
      "category": 7,
      "distribute_user_list": [
        1423580899498745,
        6851582615243077
      ],
      "dataset_size": 5665853943492229,
      "assign_num": 1647320215624315,
      "reward": 7787577383413952,
      "assigner": 1818033878502257,
      "thread_list": [
        {
          "id": 7099700181973988,
          "accept_time": "1971-08-25 PM 21:22:26",
          "completed_num": 6465115883381227,
          "review_status": 2,
          "auto_check_score": 81.97
        },
        {
          "id": 1176892633416868,
          "accept_time": "2018-01-30 AM 04:14:07",
          "completed_num": 8084972435304736,
          "review_status": 0,
          "auto_check_score": 46.2
        },
        {
          "id": 7453987700044862,
          "accept_time": "2016-09-20 PM 13:14:22",
          "completed_num": 3310197320024669,
          "review_status": 3,
          "auto_check_score": 74.512463
        },
        {
          "id": 4382208931038285,
          "accept_time": "2001-07-07 AM 08:42:19",
          "completed_num": 510378557356592,
          "review_status": 1,
          "auto_check_score": 9.085117068683878
        },
        {
          "id": 2015070507767010,
          "accept_time": "2015-07-18 PM 14:36:00",
          "completed_num": 332733899800160,
          "review_status": 3,
          "auto_check_score": 45.41575181826451
        }
      ],
      "credit_lower_bound": 5827061265648813,
      "provide_test": false,
      "entrust_intermediary": false,
      "intermediary_id": 286503222623984,
      "intermediary_commmission": 78416704173312
    }
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                         | 类型      | 必选 | 约束 | 中文名 | 说明                                                         |
| ---------------------------- | --------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code                       | integer   | true | none |        | none                                                         |
| » message                    | string    | true | none |        | none                                                         |
| » data                       | object    | true | none |        | none                                                         |
| »» task                      | object    | true | none |        | 进行了信用分下限调整的任务                                   |
| »»» name                     | string    | true | none |        | 任务名称                                                     |
| »»» id                       | integer   | true | none |        | 任务id                                                       |
| »»» status                   | integer   | true | none |        | 任务状态，0-6分别表示未定义、管理员审核中、招募中、进行中、已完成、无法分配、审核结果中 |
| »»» created_time             | string    | true | none |        | 任务创建时间                                                 |
| »»» duration                 | integer   | true | none |        | 容许的标注方总用时                                           |
| »»» time_limit               | integer   | true | none |        | 单个任务时间限制                                             |
| »»» category                 | integer   | true | none |        | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| »»» distribute_user_list     | [integer] | true | none |        | 任务分配用户列表                                             |
| »»» dataset_size             | integer   | true | none |        | 数据集中条目的个数                                           |
| »»» assign_num               | integer   | true | none |        | 需要分配的标注方的数量                                       |
| »»» reward                   | integer   | true | none |        | 每道题奖励的积分量                                           |
| »»» assigner                 | integer   | true | none |        | 任务发布者id                                                 |
| »»» thread_list              | [object]  | true | none |        | 每一项代表一个标注者的进度等信息                             |
| »»»» id                      | integer   | true | none |        | 标注者id                                                     |
| »»»» accept_time             | string    | true | none |        | 标注者接受任务的时间                                         |
| »»»» completed_num           | integer   | true | none |        | 标注者已完成的条目数（用于向需求方显示进度以及确定标注方需要标注的下一条数据） |
| »»»» review_status           | integer   | true | none |        | 0-未完成；1-待审核；2-合格; 3-不合格; 4-超时                 |
| »»»» auto_check_score        | number    | true | none |        | 浮点数，自动检查得到的正确率                                 |
| »»» credit_lower_bound       | integer   | true | none |        | 任务分配需求方的信用分下限                                   |
| »»» provide_test             | boolean   | true | none |        | 需求方是否提供测试集                                         |
| »»» entrust_intermediary     | boolean   | true | none |        | 是否委托中介                                                 |
| »»» intermediary_id          | integer   | true | none |        | 委托的中介id                                                 |
| »»» intermediary_commmission | integer   | true | none |        | 中介委托费，以积分为单位                                     |

## POST 重新进行分发

POST /tasks/refreshDistribution/{tid}

对于 recruiting 状态的任务，需求方通过这一请求可以换一批标注方重新进行分发，重新分发依然按照该任务的 credit_lower_bound 进行过滤，重新分发不会影响已经接受任务的标注方。

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明                    |
| ---- | ---- | ------- | ---- | ----------------------- |
| tid  | path | integer | 是   | 要进行重新分发的任务 ID |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "task": {
      "name": "BwHAK$89mVnue2gUZxp0zz0ViSL(Im4",
      "id": 9,
      "status": 1,
      "created_time": "2004-06-19 PM 23:10:34",
      "duration": 7122426468010081,
      "time_limit": 3300463519043985,
      "category": 2,
      "distribute_user_list": [
        4662732171855080,
        7389034316012135,
        248169276814507,
        2838412948453970
      ],
      "dataset_size": 1702238807046341,
      "assign_num": 7255410731245254,
      "reward": 5126404026505602,
      "assigner": 8494219887013695,
      "thread_list": [
        {
          "id": 1710930594892755,
          "accept_time": "1973-09-23 AM 06:08:05",
          "completed_num": 3491571197044617,
          "review_status": 3,
          "auto_check_score": 34.66
        },
        {
          "id": 7976782456769273,
          "accept_time": "1984-03-20 PM 13:21:51",
          "completed_num": 4959619666106362,
          "review_status": 3,
          "auto_check_score": 47.49338
        }
      ],
      "credit_lower_bound": 1765902694497948,
      "provide_test": true,
      "entrust_intermediary": false,
      "intermediary_id": 5493419241209918,
      "intermediary_commmission": 8434784523461264
    }
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                         | 类型      | 必选 | 约束 | 中文名 | 说明                                                         |
| ---------------------------- | --------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code                       | integer   | true | none |        | none                                                         |
| » message                    | string    | true | none |        | none                                                         |
| » data                       | object    | true | none |        | none                                                         |
| »» task                      | object    | true | none |        | 进行了重新分发的任务                                         |
| »»» name                     | string    | true | none |        | 任务名称                                                     |
| »»» id                       | integer   | true | none |        | 任务id                                                       |
| »»» status                   | integer   | true | none |        | 任务状态，0-6分别表示未定义、管理员审核中、招募中、进行中、已完成、无法分配、审核结果中 |
| »»» created_time             | string    | true | none |        | 任务创建时间                                                 |
| »»» duration                 | integer   | true | none |        | 容许的标注方总用时                                           |
| »»» time_limit               | integer   | true | none |        | 单个任务时间限制                                             |
| »»» category                 | integer   | true | none |        | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| »»» distribute_user_list     | [integer] | true | none |        | 任务分配用户列表                                             |
| »»» dataset_size             | integer   | true | none |        | 数据集中条目的个数                                           |
| »»» assign_num               | integer   | true | none |        | 需要分配的标注方的数量                                       |
| »»» reward                   | integer   | true | none |        | 每道题奖励的积分量                                           |
| »»» assigner                 | integer   | true | none |        | 任务发布者id                                                 |
| »»» thread_list              | [object]  | true | none |        | 每一项代表一个标注者的进度等信息                             |
| »»»» id                      | integer   | true | none |        | 标注者id                                                     |
| »»»» accept_time             | string    | true | none |        | 标注者接受任务的时间                                         |
| »»»» completed_num           | integer   | true | none |        | 标注者已完成的条目数（用于向需求方显示进度以及确定标注方需要标注的下一条数据） |
| »»»» review_status           | integer   | true | none |        | 0-未完成；1-待审核；2-合格; 3-不合格; 4-超时                 |
| »»»» auto_check_score        | number    | true | none |        | 浮点数，自动检查得到的正确率                                 |
| »»» credit_lower_bound       | integer   | true | none |        | 任务分配需求方的信用分下限                                   |
| »»» provide_test             | boolean   | true | none |        | 需求方是否提供测试集                                         |
| »»» entrust_intermediary     | boolean   | true | none |        | 是否委托中介                                                 |
| »»» intermediary_id          | integer   | true | none |        | 委托的中介id                                                 |
| »»» intermediary_commmission | integer   | true | none |        | 中介委托费，以积分为单位                                     |

## POST 刷新僵化任务

POST /tasks/refreshZombie/{tid}

对于处于 zombie 状态（即按照该任务当前设置的 credit_lower_bound，平台上没有足够的标注方可以进行分发）的任务，尝试进行刷新（在上一次刷新后可能有新用户注册为标注方，或者有用户的信用分达到了标准）。
如果没能成功使 zombie 状态的任务转为recruiting状态，则会返回一个错误。

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明                    |
| ---- | ---- | ------- | ---- | ----------------------- |
| tid  | path | integer | 是   | 要进行刷新的僵化任务 ID |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "task": {
      "name": "5rXeoAKSHuAORkQ#VI^FBr3JK%hl",
      "id": 11,
      "status": 1,
      "created_time": "1977-08-22 PM 20:59:38",
      "duration": 1890114045939779,
      "time_limit": 7374043702250907,
      "category": 8,
      "distribute_user_list": [
        8784737509766007,
        14667636849976
      ],
      "dataset_size": 3585568843389969,
      "assign_num": 739441308413670,
      "reward": 3398664942569599,
      "assigner": 5722991393823040,
      "thread_list": [
        {
          "id": 8117554492111648,
          "accept_time": "2010-11-11 PM 16:56:41",
          "completed_num": 5554277609009444,
          "review_status": 3,
          "auto_check_score": 56.0388
        }
      ],
      "credit_lower_bound": 8953099090789757,
      "provide_test": true,
      "entrust_intermediary": false,
      "intermediary_id": 7196509154113086,
      "intermediary_commmission": 6889943045129466
    }
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                         | 类型      | 必选 | 约束 | 中文名 | 说明                                                         |
| ---------------------------- | --------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code                       | integer   | true | none |        | none                                                         |
| » message                    | string    | true | none |        | none                                                         |
| » data                       | object    | true | none |        | none                                                         |
| »» task                      | object    | true | none |        | none                                                         |
| »»» name                     | string    | true | none |        | 任务名称                                                     |
| »»» id                       | integer   | true | none |        | 任务id                                                       |
| »»» status                   | integer   | true | none |        | 任务状态，0-6分别表示未定义、管理员审核中、招募中、进行中、已完成、无法分配、审核结果中 |
| »»» created_time             | string    | true | none |        | 任务创建时间                                                 |
| »»» duration                 | integer   | true | none |        | 容许的标注方总用时                                           |
| »»» time_limit               | integer   | true | none |        | 单个任务时间限制                                             |
| »»» category                 | integer   | true | none |        | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| »»» distribute_user_list     | [integer] | true | none |        | 任务分配用户列表                                             |
| »»» dataset_size             | integer   | true | none |        | 数据集中条目的个数                                           |
| »»» assign_num               | integer   | true | none |        | 需要分配的标注方的数量                                       |
| »»» reward                   | integer   | true | none |        | 每道题奖励的积分量                                           |
| »»» assigner                 | integer   | true | none |        | 任务发布者id                                                 |
| »»» thread_list              | [object]  | true | none |        | 每一项代表一个标注者的进度等信息                             |
| »»»» id                      | integer   | true | none |        | 标注者id                                                     |
| »»»» accept_time             | string    | true | none |        | 标注者接受任务的时间                                         |
| »»»» completed_num           | integer   | true | none |        | 标注者已完成的条目数（用于向需求方显示进度以及确定标注方需要标注的下一条数据） |
| »»»» review_status           | integer   | true | none |        | 0-未完成；1-待审核；2-合格; 3-不合格; 4-超时                 |
| »»»» auto_check_score        | number    | true | none |        | 浮点数，自动检查得到的正确率                                 |
| »»» credit_lower_bound       | integer   | true | none |        | 任务分配需求方的信用分下限                                   |
| »»» provide_test             | boolean   | true | none |        | 需求方是否提供测试集                                         |
| »»» entrust_intermediary     | boolean   | true | none |        | 是否委托中介                                                 |
| »»» intermediary_id          | integer   | true | none |        | 委托的中介id                                                 |
| »»» intermediary_commmission | integer   | true | none |        | 中介委托费，以积分为单位                                     |

## POST 验收不通过

POST /tasks/vetoThread/{tid}

在验收标注结果时，否决某个任务的某个线程。

> Body 请求参数

```json
{
  "thread_id": 0
}
```

### 请求参数

| 名称        | 位置 | 类型    | 必选 | 说明            |
| ----------- | ---- | ------- | ---- | --------------- |
| tid         | path | integer | 是   | 验收的任务的 ID |
| body        | body | object  | 否   | none            |
| » thread_id | body | integer | 是   | 要否决的线程 ID |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## POST 上传文件

POST /tasks/VIPupload/{tid}

需求方为已发布的任务上传数据文件，后端会对数据文件进行格式检查，如果格式错误则将返回一个错误。
这个接口也可以对已经上传过文件的任务进行调用，如果新上传的文件符合格式要求，则会覆盖原有数据文件。
注意这个接口会检查 VIP 令牌，因此无论用户是否是 VIP，在调用这个请求前都需要先调用 getVIPToken 请求来获取 VIP 令牌。该接口会在后端检查 token，并根据 token 进行相应的速度限制。
为了便于调试，这个接口会返回文件大小，速度限制，以及“设置token->后端完成文件接受”的实际计时。

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明                    |
| ---- | ---- | ------- | ---- | ----------------------- |
| tid  | path | integer | 是   | 要上传文件的 task 的 id |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "speed": 10000,
    "size": 5000000,
    "time_consumed": 500
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称             | 类型    | 必选 | 约束 | 中文名 | 说明                                |
| ---------------- | ------- | ---- | ---- | ------ | ----------------------------------- |
| » code           | integer | true | none |        | none                                |
| » message        | string  | true | none |        | none                                |
| » data           | object  | true | none |        | none                                |
| »» speed         | integer | true | none |        | 上传过程的速率，单位是bytes per sec |
| »» size          | integer | true | none |        | 上传的文件的大小，单位是 bytes      |
| »» time_consumed | integer | true | none |        | 上传过程耗时，单位是secs            |

## GET 查看任务视频

GET /tasks/demanderGetVideo/{tid}

需求方在验收标注结果时，可以通过该接口获取指定视频任务中指定题目的视频数据。

### 请求参数

| 名称    | 位置  | 类型    | 必选 | 说明                      |
| ------- | ----- | ------- | ---- | ------------------------- |
| tid     | path  | integer | 是   | 验收的任务 ID             |
| data_id | query | integer | 否   | 要查看的视频所属的题目 ID |

> 返回示例

> OK

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

## GET 查看任务音频

GET /tasks/demanderGetAudio/{tid}

对于语音任务，需求方验收时可以通过该接口获取指定题目的语音数据。

### 请求参数

| 名称    | 位置  | 类型    | 必选 | 说明                          |
| ------- | ----- | ------- | ---- | ----------------------------- |
| tid     | path  | integer | 是   | 验收的任务的 ID               |
| data_id | query | integer | 否   | 要查看的音频数据所属的题目 ID |

> 返回示例

> OK

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

# 任务接口/标注方接口

## GET 查看任务

GET /tasks/workerView

获取当前用户接受的所有任务

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "tasks": [
      {
        "accept_time": "1999-02-07 18:02:58",
        "completed_num": 10,
        "status": 0,
        "name": "L*z3Ax9^l",
        "id": 122,
        "created_time": "1971-10-19 06:16:14",
        "duration": 20000,
        "time_limit": 100,
        "category": 3,
        "distribute_user_list": [
          2,
          3,
          4,
          6
        ],
        "dataset_size": 123,
        "assign_num": 5,
        "reward": 1,
        "assigner": 10,
        "credit_lower_bound": 80
      },
      {
        "accept_time": "1994-06-18 20:40:48",
        "completed_num": 1,
        "status": 0,
        "name": "!UUQL8BkWHYfYSQsbKW",
        "id": 13,
        "created_time": "1977-01-13 07:28:26",
        "duration": 3000,
        "time_limit": 10,
        "category": 5,
        "distribute_user_list": [
          53,
          46,
          32
        ],
        "dataset_size": 10,
        "assign_num": 2,
        "reward": 10,
        "assigner": 90,
        "credit_lower_bound": 95
      }
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                     | 类型      | 必选 | 约束 | 中文名 | 说明                                                         |
| ------------------------ | --------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code                   | integer   | true | none |        | none                                                         |
| » message                | string    | true | none |        | none                                                         |
| » data                   | object    | true | none |        | none                                                         |
| »» tasks                 | [object]  | true | none |        | none                                                         |
| »»» name                 | string    | true | none |        | 任务名称                                                     |
| »»» id                   | integer   | true | none |        | 任务id                                                       |
| »»» created_time         | string    | true | none |        | 任务创建时间                                                 |
| »»» duration             | integer   | true | none |        | 容许的标注方总用时                                           |
| »»» time_limit           | integer   | true | none |        | 单个任务时间限制                                             |
| »»» category             | integer   | true | none |        | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| »»» distribute_user_list | [integer] | true | none |        | 任务分配用户列表                                             |
| »»» dataset_size         | integer   | true | none |        | 数据集中条目的个数                                           |
| »»» assign_num           | integer   | true | none |        | 需要分配的标注方的数量                                       |
| »»» reward               | integer   | true | none |        | 每道题奖励的积分量                                           |
| »»» assigner             | integer   | true | none |        | 任务发布者id                                                 |
| »»» credit_lower_bound   | integer   | true | none |        | 任务分配需求方的信用分下限                                   |
| »»» accept_time          | string    | true | none |        | none                                                         |
| »»» completed_num        | integer   | true | none |        | none                                                         |
| »»» status               | integer   | true | none |        | 0为未完成，1为等待审核中，2为合格，3为不合格，4为超时        |

## POST 接受任务

POST /tasks/accept/{tid}

标注方接受一个任务

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明 |
| ---- | ---- | ------- | ---- | ---- |
| tid  | path | integer | 是   | none |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## POST 拒绝任务

POST /tasks/reject/{tid}

标注方拒绝一个任务，此后不会再向其推送该任务。

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明 |
| ---- | ---- | ------- | ---- | ---- |
| tid  | path | integer | 是   | none |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## POST 保存标注

POST /tasks/saveAnnotation

标注方完成了一道题目，保存标注结果。

> Body 请求参数

```json
"{\r\n    \"tid\": 23,\r\n    \"annotationResult\": {\r\n        \"label\": 2,\r\n    },\r\n    \"item_id\": 2\r\n}"
```

### 请求参数

| 名称               | 位置 | 类型     | 必选 | 说明                                                         |
| ------------------ | ---- | -------- | ---- | ------------------------------------------------------------ |
| body               | body | object   | 否   | none                                                         |
| » tid              | body | integer  | 是   | none                                                         |
| » item_id          | body | integer  | 是   | 所储存数据在data中的序号（从0开始计数），方便判断是否在测试集中 |
| » annotationResult | body | object   | 是   | 根据任务类型不同而定，文本分类任务返回labels.txt中的一个int类型的label，从0开始计数 |
| »» label           | body | integer  | 否   | 标注类任务需要                                               |
| »» content         | body | string   | 否   | 语音、视频标注需要                                           |
| »» bounding_boxes  | body | [string] | 否   | 图片框选类任务需要                                           |
| »» key_points      | body | [string] | 否   | 骨骼打点类任务需要                                           |
| »» relation_pairs  | body | [string] | 否   | 实体三元组标注类需要                                         |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## GET 获取条目

GET /tasks/getItem

获取任务id为tid的任务中，序号为item_id的数据。

> Body 请求参数

```yaml
{}

```

### 请求参数

| 名称    | 位置  | 类型    | 必选 | 说明 |
| ------- | ----- | ------- | ---- | ---- |
| tid     | query | integer | 是   | none |
| item_id | query | integer | 是   | none |
| body    | body  | object  | 否   | none |

> 返回示例

> OK

```json
{
  "data": {
    "item": {
      "item_id": 0,
      "dataset_size": 10,
      "time_limit": "1999-10-22 10:54:05",
      "category": 1,
      "content": "这是一条文本",
      "labels": [
        "0",
        "1",
        "2"
      ]
    }
  },
  "code": 200,
  "message": "success"
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称               | 类型     | 必选  | 约束 | 中文名 | 说明                                                         |
| ------------------ | -------- | ----- | ---- | ------ | ------------------------------------------------------------ |
| » code             | integer  | true  | none |        | none                                                         |
| » message          | string   | true  | none |        | none                                                         |
| » data             | object   | true  | none |        | 具体内容根据任务类型而定                                     |
| »» item            | object   | true  | none |        | none                                                         |
| »»» item_id        | integer  | true  | none |        | 条目序号                                                     |
| »»» dataset_size   | integer  | true  | none |        | 任务数据集大小                                               |
| »»» time_limit     | string   | true  | none |        | 任务单题限时                                                 |
| »»» category       | integer  | true  | none |        | 任务种类                                                     |
| »»» content        | string   | false | none |        | 文本分类、三元组标注返回的是string类型的该行文本，审核任务如果该文件为文本会返回当前内容否则为空 |
| »»» labels         | [string] | false | none |        | 标注类任务含有，返回的是标签列表                             |
| »»» object         | string   | false | none |        | 图片框选类任务含有，返回带框选物体的名字                     |
| »»» keypoint_names | [string] | false | none |        | 骨骼打点类任务含有，返回关键点列表                           |
| »»» relation       | string   | false | none |        | 三元组标注类任务含有，返回任务要求的关系                     |

## POST 获取任务

POST /tasks/get

根据标注方设置的模式和筛选条件，获取标注方当前可接受的任务列表，前端按顺序推送给标注方。

> Body 请求参数

```json
"{\r\n    \"mode\": 2,\r\n    \"filter\": {\r\n        \"reward_lower_bound\": 1,\r\n        \"duration_lower_bound\": 0,\r\n        \"time_limit_lower_bound\": 50,\r\n        \"categories\": [\r\n            1,\r\n            2,\r\n            3,\r\n        ],\r\n        \"dataset_size_lower_bound\": 10,\r\n        \"dataset_size_upper_bound\": 100\r\n    }\r\n}"
```

### 请求参数

| 名称                        | 位置 | 类型      | 必选 | 说明                                |
| --------------------------- | ---- | --------- | ---- | ----------------------------------- |
| body                        | body | object    | 否   | none                                |
| » mode                      | body | integer   | 是   | 0表示顺序，1表示随机，2表示偏好推送 |
| » filter                    | body | object    | 是   | 0表示不筛选，1表示筛选              |
| »» reward_lower_bound       | body | integer   | 是   | none                                |
| »» duration_lower_bound     | body | integer   | 是   | none                                |
| »» time_limit_lower_bound   | body | integer   | 是   | none                                |
| »» dataset_size_lower_bound | body | integer   | 是   | none                                |
| »» dataset_size_upper_bound | body | integer   | 是   | none                                |
| »» categories               | body | [integer] | 是   | 允许的分类任务模板                  |

> 返回示例

> 200 Response

```json
{
  "code": 0,
  "message": "string",
  "data": {
    "tasks": [
      {
        "name": "string",
        "id": 0,
        "status": 0,
        "created_time": "string",
        "duration": 0,
        "time_limit": 0,
        "category": 0,
        "distribute_user_list": [
          0
        ],
        "dataset_size": 0,
        "assign_num": 0,
        "reward": 0,
        "assigner": 0,
        "credit_lower_bound": 0,
        "entrust_intermediary": true,
        "intermediary_id": 0,
        "intermediary_commmission": 0
      }
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                         | 类型      | 必选 | 约束 | 中文名 | 说明                                                         |
| ---------------------------- | --------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code                       | integer   | true | none |        | none                                                         |
| » message                    | string    | true | none |        | none                                                         |
| » data                       | object    | true | none |        | none                                                         |
| »» tasks                     | [object]  | true | none |        | none                                                         |
| »»» name                     | string    | true | none |        | 任务名称                                                     |
| »»» id                       | integer   | true | none |        | 任务id                                                       |
| »»» status                   | integer   | true | none |        | 任务状态，0-6分别表示未定义、管理员审核中、招募中、进行中、已完成、无法分配、审核结果中 |
| »»» created_time             | string    | true | none |        | 任务创建时间                                                 |
| »»» duration                 | integer   | true | none |        | 容许的标注方总用时                                           |
| »»» time_limit               | integer   | true | none |        | 单个任务时间限制                                             |
| »»» category                 | integer   | true | none |        | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| »»» distribute_user_list     | [integer] | true | none |        | 任务分配用户列表                                             |
| »»» dataset_size             | integer   | true | none |        | 数据集中条目的个数                                           |
| »»» assign_num               | integer   | true | none |        | 需要分配的标注方的数量                                       |
| »»» reward                   | integer   | true | none |        | 每道题奖励的积分量                                           |
| »»» assigner                 | integer   | true | none |        | 任务发布者id                                                 |
| »»» credit_lower_bound       | integer   | true | none |        | 任务分配需求方的信用分下限                                   |
| »»» entrust_intermediary     | boolean   | true | none |        | 是否委托中介                                                 |
| »»» intermediary_id          | integer   | true | none |        | 委托的中介id                                                 |
| »»» intermediary_commmission | integer   | true | none |        | 中介委托费，以积分为单位                                     |

## POST 任务超时

POST /tasks/timeout

任务单题超时时前端发送此请求。

> Body 请求参数

```json
{
  "tid": 66
}
```

### 请求参数

| 名称  | 位置 | 类型    | 必选 | 说明 |
| ----- | ---- | ------- | ---- | ---- |
| body  | body | object  | 否   | none |
| » tid | body | integer | 是   | none |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## GET 获取图片

GET /tasks/getItemImage

需要返回图片的item调用此接口，返回jpeg格式的图片。

### 请求参数

| 名称    | 位置  | 类型    | 必选 | 说明 |
| ------- | ----- | ------- | ---- | ---- |
| tid     | query | integer | 否   | none |
| item_id | query | integer | 否   | none |

> 返回示例

> 200 Response

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

## GET 下载数据

GET /tasks/downloadDataset/{tid}

标注方调用此接口下载任务数据的zip格式，用于excel标注。

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明 |
| ---- | ---- | ------- | ---- | ---- |
| tid  | path | integer | 是   | none |

> 返回示例

> 200 Response

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

## POST 上传标注

POST /tasks/uploadAnno/{tid}

标注方上传excel标注的数据。
对于普通标注类任务，每一行为一个结果，跳题则空行。
对于三元组标注，第几行对应text.txt里第几行，每行一个格子是一个关系组，用空格分隔，例如某一格的内容为"学校 我"。

> Body 请求参数

```yaml
anno: file://D:\files\lessons\SoftwareEngineering\crowdsourcingPlatform-backend\fileForTest\excel_annotations\text_with_test_mini.xlsx

```

### 请求参数

| 名称   | 位置 | 类型           | 必选 | 说明 |
| ------ | ---- | -------------- | ---- | ---- |
| tid    | path | integer        | 是   | none |
| body   | body | object         | 否   | none |
| » anno | body | string(binary) | 否   | none |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## GET 获取进度

GET /tasks/labelingProgress/{tid}

用于获取当前任务的完成情况，对于文本类任务还会返回任务内容预览。

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明   |
| ---- | ---- | ------- | ---- | ------ |
| tid  | path | integer | 是   | 任务id |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "contents": [
      "dolor in",
      "enim quis consequat",
      "ut",
      "deserunt eiusmod Lorem non"
    ],
    "completed_list": [
      false,
      true,
      true,
      true
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称              | 类型      | 必选 | 约束 | 中文名 | 说明                     |
| ----------------- | --------- | ---- | ---- | ------ | ------------------------ |
| » code            | integer   | true | none |        | none                     |
| » message         | string    | true | none |        | none                     |
| » data            | object    | true | none |        | none                     |
| »» contents       | [string]  | true | none |        | 除文本分类任务外都返回空 |
| »» completed_list | [boolean] | true | none |        | none                     |

## GET 获取音频

GET /tasks/getItemAudio

需要返回音频的item调用此接口，返回mp3格式的音频。

### 请求参数

| 名称    | 位置  | 类型    | 必选 | 说明 |
| ------- | ----- | ------- | ---- | ---- |
| tid     | query | integer | 是   | none |
| item_id | query | integer | 是   | none |

> 返回示例

> 200 Response

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

## GET 获取视频

GET /tasks/getItemVideo

需要返回视频的item调用此接口，返回mp4格式的视频。

### 请求参数

| 名称    | 位置  | 类型    | 必选 | 说明 |
| ------- | ----- | ------- | ---- | ---- |
| tid     | query | integer | 是   | none |
| item_id | query | integer | 是   | none |

> 返回示例

> 200 Response

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

# 任务接口/管理员接口

## GET 查看待审核任务

GET /tasks/staffViewTasks

管理员调用此接口，即可查看所有待审查的任务。

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "tasks": [
      {
        "name": "jw0AYne",
        "id": 1,
        "created_time": "1978-08-16 AM 05:32:17",
        "duration": 530678506288567,
        "time_limit": 5835349538106488,
        "category": 1,
        "distribute_user_list": [
          7844087140365236,
          7671073855702677,
          6293703603225128,
          5504665058234269,
          1092834078121038
        ],
        "dataset_size": 2737330833591910,
        "assign_num": 79951883587838,
        "reward": 945154949958503,
        "assigner": 956460994720369,
        "thread_list": [
          {
            "id": 5671606121408143,
            "accept_time": "2010-09-22 AM 02:47:58",
            "completed_num": 8452344808700317,
            "review_status": 3,
            "auto_check_score": 67.55666
          },
          {
            "id": 6832747126410514,
            "accept_time": "2015-07-24 PM 12:10:19",
            "completed_num": 6946569100062723,
            "review_status": 4,
            "auto_check_score": 26
          },
          {
            "id": 2278583062139351,
            "accept_time": "1981-10-03 PM 19:11:19",
            "completed_num": 1078073074184786,
            "review_status": 2,
            "auto_check_score": 17.000743279822
          }
        ],
        "credit_lower_bound": 3687618208677384,
        "provide_test": false,
        "entrust_intermediary": true,
        "intermediary_id": 8903596616182889,
        "intermediary_commmission": 202691427790647
      },
      {
        "name": "H5l@J",
        "id": 2,
        "created_time": "2011-10-24 PM 19:19:19",
        "duration": 7813879385124809,
        "time_limit": 1776454465671481,
        "category": 1,
        "distribute_user_list": [
          2606329251904519,
          3986787721414353,
          3778460962172381,
          4897739494473831,
          3121859441662444
        ],
        "dataset_size": 284736174915658,
        "assign_num": 4637204091393866,
        "reward": 2342537115393773,
        "assigner": 4062417410980939,
        "thread_list": [
          {
            "id": 8570345689961293,
            "accept_time": "2008-10-08 AM 04:47:48",
            "completed_num": 1403700990469669,
            "review_status": 2,
            "auto_check_score": 84.3136
          },
          {
            "id": 8621883248961966,
            "accept_time": "1975-03-24 PM 20:55:41",
            "completed_num": 2451107096426103,
            "review_status": 2,
            "auto_check_score": 1.07
          },
          {
            "id": 6727522792077194,
            "accept_time": "2022-02-21 PM 15:10:53",
            "completed_num": 892146128228402,
            "review_status": 3,
            "auto_check_score": 77.032796
          }
        ],
        "credit_lower_bound": 5588950756542061,
        "provide_test": true,
        "entrust_intermediary": true,
        "intermediary_id": 8757837012302725,
        "intermediary_commmission": 4664959857360264
      }
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                         | 类型      | 必选 | 约束 | 中文名 | 说明                                                         |
| ---------------------------- | --------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code                       | integer   | true | none |        | none                                                         |
| » message                    | string    | true | none |        | none                                                         |
| » data                       | object    | true | none |        | none                                                         |
| »» tasks                     | [object]  | true | none |        | 待审核的任务                                                 |
| »»» name                     | string    | true | none |        | 任务名称                                                     |
| »»» id                       | integer   | true | none |        | 任务id                                                       |
| »»» created_time             | string    | true | none |        | 任务创建时间                                                 |
| »»» duration                 | integer   | true | none |        | 容许的标注方总用时                                           |
| »»» time_limit               | integer   | true | none |        | 单个任务时间限制                                             |
| »»» category                 | integer   | true | none |        | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| »»» distribute_user_list     | [integer] | true | none |        | 任务分配用户列表                                             |
| »»» dataset_size             | integer   | true | none |        | 数据集中条目的个数                                           |
| »»» assign_num               | integer   | true | none |        | 需要分配的标注方的数量                                       |
| »»» reward                   | integer   | true | none |        | 每道题奖励的积分量                                           |
| »»» assigner                 | integer   | true | none |        | 任务发布者id                                                 |
| »»» thread_list              | [object]  | true | none |        | 每一项代表一个标注者的进度等信息                             |
| »»»» id                      | integer   | true | none |        | 标注者id                                                     |
| »»»» accept_time             | string    | true | none |        | 标注者接受任务的时间                                         |
| »»»» completed_num           | integer   | true | none |        | 标注者已完成的条目数（用于向需求方显示进度以及确定标注方需要标注的下一条数据） |
| »»»» review_status           | integer   | true | none |        | 0-未完成；1-待审核；2-合格; 3-不合格; 4-超时                 |
| »»»» auto_check_score        | number    | true | none |        | 浮点数，自动检查得到的正确率                                 |
| »»» credit_lower_bound       | integer   | true | none |        | 任务分配需求方的信用分下限                                   |
| »»» provide_test             | boolean   | true | none |        | 需求方是否提供测试集                                         |
| »»» entrust_intermediary     | boolean   | true | none |        | 是否委托中介                                                 |
| »»» intermediary_id          | integer   | true | none |        | 委托的中介id                                                 |
| »»» intermediary_commmission | integer   | true | none |        | 中介委托费，以积分为单位                                     |

## POST 通过任务

POST /tasks/approveTask/{tid}

审核通过指定的任务。

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明 |
| ---- | ---- | ------- | ---- | ---- |
| tid  | path | integer | 是   | none |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## POST 否决任务

POST /tasks/vetoTask/{tid}

对于指定的任务，提出审核不通过结果。

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明 |
| ---- | ---- | ------- | ---- | ---- |
| tid  | path | integer | 是   | none |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## GET 查看任务数据

GET /tasks/staffViewData/{tid}

管理员需要对任务给出审核结果之前可以调用此接口查看任务中的文本类数据。
对于图片、视频、音频等任务，可能还需要配合调用用于查看图片等其他类型数据的接口来给管理员提供完整任务数据作为审核依据。

### 请求参数

| 名称    | 位置  | 类型    | 必选 | 说明                                     |
| ------- | ----- | ------- | ---- | ---------------------------------------- |
| tid     | path  | integer | 是   | 想查看的任务的 ID                        |
| data_id | query | integer | 否   | 想要查看的数据在整个任务所有数据中的序号 |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "labels": [
      "anim",
      "esse ad aute laboris Lorem",
      "eu proident labore ad"
    ],
    "item": "ullamco"
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型     | 必选  | 约束 | 中文名 | 说明         |
| --------- | -------- | ----- | ---- | ------ | ------------ |
| » code    | integer  | true  | none |        | none         |
| » message | string   | true  | none |        | none         |
| » data    | object   | true  | none |        | none         |
| »» labels | [string] | true  | none |        | 分类选项标签 |
| »» item   | string   | false | none |        | 待分类文本   |

## GET 查看任务图片

GET /tasks/staffViewImage/{tid}

管理员可以调用此接口查看任务中的图片，作为审核任务的依据。

### 请求参数

| 名称    | 位置  | 类型    | 必选 | 说明                        |
| ------- | ----- | ------- | ---- | --------------------------- |
| tid     | path  | integer | 是   | 想要查看的任务的 ID         |
| data_id | query | integer | 是   | 想要查看的图片所属的题目 ID |

> 返回示例

> OK

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## GET 查看任务音频

GET /tasks/staffViewAudio/{tid}

管理员可以调用此接口来获取任务中的音频作为审核依据。

### 请求参数

| 名称    | 位置  | 类型    | 必选 | 说明                      |
| ------- | ----- | ------- | ---- | ------------------------- |
| tid     | path  | integer | 是   | 要审核的任务的 ID         |
| data_id | query | integer | 否   | 要获取的音频所属的题目 ID |

> 返回示例

> OK

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## GET 查看任务视频

GET /tasks/staffViewVideo/{tid}

管理员可以调用此接口查看任务中的视频数据。

### 请求参数

| 名称    | 位置  | 类型    | 必选 | 说明                            |
| ------- | ----- | ------- | ---- | ------------------------------- |
| tid     | path  | integer | 是   | 要审核的任务的 ID               |
| data_id | query | integer | 否   | 要查看的视频数据所属的题目的 ID |

> 返回示例

> OK

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

# 任务接口/中介接口

## GET 获取任务

GET /tasks/getEntrustedTasks

中介调用，返回所有委托给中介但是尚未有中介接单的任务。

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "tasks": [
      {
        "name": "yqo#f96LzyGu5EM^Iuf[&e1ymi^RM2W[nBj#",
        "id": 136,
        "status": 2,
        "created_time": "1974-02-05 13:23:19",
        "duration": 4000,
        "time_limit": 100,
        "category": 5,
        "dataset_size": 30,
        "assign_num": 4,
        "reward": 2,
        "assigner": 34,
        "provide_test": true,
        "intermediary_commmission": 400
      },
      {
        "name": "m&Xm$s4RY!g^oV##QP0",
        "id": 137,
        "status": 2,
        "created_time": "2005-05-05 15:53:00",
        "duration": 6000,
        "time_limit": 10,
        "category": 2,
        "dataset_size": 45,
        "assign_num": 2,
        "reward": 1,
        "assigner": 45,
        "provide_test": false,
        "intermediary_commmission": 1000
      }
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                         | 类型     | 必选 | 约束 | 中文名 | 说明                                                         |
| ---------------------------- | -------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code                       | integer  | true | none |        | none                                                         |
| » message                    | string   | true | none |        | none                                                         |
| » data                       | object   | true | none |        | none                                                         |
| »» tasks                     | [object] | true | none |        | none                                                         |
| »»» name                     | string   | true | none |        | 任务名称                                                     |
| »»» id                       | integer  | true | none |        | 任务id                                                       |
| »»» status                   | integer  | true | none |        | 任务状态，0-6分别表示未定义、管理员审核中、招募中、进行中、已完成、无法分配、审核结果中 |
| »»» created_time             | string   | true | none |        | 任务创建时间                                                 |
| »»» duration                 | integer  | true | none |        | 容许的标注方总用时                                           |
| »»» time_limit               | integer  | true | none |        | 单个任务时间限制                                             |
| »»» category                 | integer  | true | none |        | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| »»» dataset_size             | integer  | true | none |        | 数据集中条目的个数                                           |
| »»» assign_num               | integer  | true | none |        | 需要分配的标注方的数量                                       |
| »»» reward                   | integer  | true | none |        | 每道题奖励的积分量                                           |
| »»» assigner                 | integer  | true | none |        | 任务发布者id                                                 |
| »»» provide_test             | boolean  | true | none |        | 需求方是否提供测试集                                         |
| »»» intermediary_commmission | integer  | true | none |        | 中介委托费，以积分为单位                                     |

## POST 接受任务

POST /tasks/receiveOrder/{tid}

中介接受某个任务的分发请求。

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明 |
| ---- | ---- | ------- | ---- | ---- |
| tid  | path | integer | 是   | none |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## GET 查看任务

GET /tasks/viewOrders

中介查看自己已经接的任务单。

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "orders": [
      {
        "name": "6kd9ajrtuIT&fU&^78][r",
        "id": 140,
        "status": 3,
        "created_time": "2016-01-10 21:09:40",
        "duration": 300,
        "time_limit": 10,
        "category": 3,
        "distribute_user_list": [
          23,
          45,
          67
        ],
        "dataset_size": 23,
        "assign_num": 3,
        "reward": 3,
        "assigner": 12,
        "thread_list": [
          {
            "id": 3,
            "accept_time": "1986-12-27 AM 11:31:37",
            "completed_num": 4,
            "review_status": 3,
            "auto_check_score": 51.82345606671887
          },
          {
            "id": 4,
            "accept_time": "1987-12-03 AM 06:04:51",
            "completed_num": 5,
            "review_status": 3,
            "auto_check_score": 59.4
          },
          {
            "id": 5,
            "accept_time": "2017-12-15 PM 15:45:06",
            "completed_num": 9,
            "review_status": 3,
            "auto_check_score": 69
          }
        ],
        "rejecter_list": [
          6,
          7,
          8
        ],
        "provide_test": false,
        "intermediary_commmission": 700
      }
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                         | 类型      | 必选 | 约束 | 中文名 | 说明                                                         |
| ---------------------------- | --------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| » code                       | integer   | true | none |        | none                                                         |
| » message                    | string    | true | none |        | none                                                         |
| » data                       | object    | true | none |        | none                                                         |
| »» orders                    | [object]  | true | none |        | none                                                         |
| »»» name                     | string    | true | none |        | 任务名称                                                     |
| »»» id                       | integer   | true | none |        | 任务id                                                       |
| »»» status                   | integer   | true | none |        | 任务状态，0-6分别表示未定义、管理员审核中、招募中、进行中、已完成、无法分配、审核结果中 |
| »»» created_time             | string    | true | none |        | 任务创建时间                                                 |
| »»» duration                 | integer   | true | none |        | 容许的标注方总用时                                           |
| »»» time_limit               | integer   | true | none |        | 单个任务时间限制                                             |
| »»» category                 | integer   | true | none |        | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| »»» distribute_user_list     | [integer] | true | none |        | 任务分配用户列表                                             |
| »»» dataset_size             | integer   | true | none |        | 数据集中条目的个数                                           |
| »»» assign_num               | integer   | true | none |        | 需要分配的标注方的数量                                       |
| »»» reward                   | integer   | true | none |        | 每道题奖励的积分量                                           |
| »»» assigner                 | integer   | true | none |        | 任务发布者id                                                 |
| »»» thread_list              | [object]  | true | none |        | 每一项代表一个标注者的进度等信息                             |
| »»»» id                      | integer   | true | none |        | 标注者id                                                     |
| »»»» accept_time             | string    | true | none |        | 标注者接受任务的时间                                         |
| »»»» completed_num           | integer   | true | none |        | 标注者已完成的条目数（用于向需求方显示进度以及确定标注方需要标注的下一条数据） |
| »»»» review_status           | integer   | true | none |        | 0-未完成；1-待审核；2-合格; 3-不合格; 4-超时                 |
| »»»» auto_check_score        | number    | true | none |        | 浮点数，自动检查得到的正确率                                 |
| »»» rejecter_list            | [integer] | true | none |        | 已拒绝此任务的标注方列表，分发是不会再分发给这些用户         |
| »»» provide_test             | boolean   | true | none |        | 需求方是否提供测试集                                         |
| »»» intermediary_commmission | integer   | true | none |        | 中介委托费，以积分为单位                                     |

## GET 查看标注方

GET /tasks/intermediaryViewWorkers

中介调用，用来看到所有worker的情况，若指定任务id则还会返回该任务接受和拒绝的历史。

### 请求参数

| 名称 | 位置  | 类型    | 必选 | 说明 |
| ---- | ----- | ------- | ---- | ---- |
| tid  | query | integer | 否   | none |

> 返回示例

> 200 Response

```json
{
  "code": 0,
  "message": "string",
  "data": {
    "workers": {
      "username": "string",
      "id": 0,
      "banned": true,
      "credits": 0,
      "points": 0,
      "bankcard_number": "string",
      "email": "string",
      "accepted_tasks": [
        {
          "status": 0
        }
      ],
      "points_earned_by_laboring": 0
    },
    "rejecter_list": [
      0
    ],
    "accepter_list": [
      0
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                          | 类型      | 必选  | 约束 | 中文名 | 说明                                         |
| ----------------------------- | --------- | ----- | ---- | ------ | -------------------------------------------- |
| » code                        | integer   | true  | none |        | none                                         |
| » message                     | string    | true  | none |        | none                                         |
| » data                        | object    | true  | none |        | none                                         |
| »» workers                    | object    | true  | none |        | none                                         |
| »»» username                  | string    | true  | none |        | 用户名                                       |
| »»» id                        | integer   | true  | none |        | 用户id                                       |
| »»» banned                    | boolean   | true  | none |        | 是否被封禁                                   |
| »»» credits                   | integer   | true  | none |        | 用户信用分                                   |
| »»» points                    | integer   | true  | none |        | 用户积分                                     |
| »»» bankcard_number           | string    | true  | none |        | 银行卡号，为19位数字                         |
| »»» email                     | string    | true  | none |        | 用户邮箱                                     |
| »»» accepted_tasks            | [object]  | true  | none |        | none                                         |
| »»»» status                   | integer   | true  | none |        | 0-未完成；1-待审核；2-合格；3-不合格；4-超时 |
| »»» points_earned_by_laboring | integer   | true  | none |        | none                                         |
| »» rejecter_list              | [integer] | false | none |        | none                                         |
| »» accepter_list              | [integer] | false | none |        | none                                         |

## POST 分配任务

POST /tasks/intermediaryDistribute/{tid}

中介分发任务，直接返回修改后的完整分发列表即可。

> Body 请求参数

```json
{
  "distribute_user_list": [
    89,
    92,
    12
  ]
}
```

### 请求参数

| 名称                   | 位置 | 类型      | 必选 | 说明 |
| ---------------------- | ---- | --------- | ---- | ---- |
| tid                    | path | string    | 是   | none |
| body                   | body | object    | 否   | none |
| » distribute_user_list | body | [integer] | 是   | none |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

# 举报接口

## POST 发送举报

POST /reports/send

用户发送举报调用，标注方只能举报需求方恶意审核，需求方只能举报标注方恶意刷题。

> Body 请求参数

```json
{
  "receiver_id": 65,
  "category": 0,
  "tid": 59,
  "reason": "ullamco voluptate fugiat esse nulla"
}
```

### 请求参数

| 名称          | 位置 | 类型    | 必选 | 说明                     |
| ------------- | ---- | ------- | ---- | ------------------------ |
| body          | body | object  | 否   | none                     |
| » receiver_id | body | integer | 是   | 被举报者的id             |
| » category    | body | integer | 是   | 0为恶意审核，1为恶意刷题 |
| » tid         | body | integer | 是   | 与举报相关的任务id       |
| » reason      | body | string  | 是   | 举报理由，随便写写       |

> 返回示例

> 200 Response

```json
{
  "code": 0,
  "message": "string",
  "data": {
    "id": 0,
    "sender": 0,
    "receiver": 0,
    "tid": 0,
    "category": 0,
    "status": 0,
    "reason": "string",
    "credit_variance": 0
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称               | 类型    | 必选 | 约束 | 中文名 | 说明                                                 |
| ------------------ | ------- | ---- | ---- | ------ | ---------------------------------------------------- |
| » code             | integer | true | none |        | none                                                 |
| » message          | string  | true | none |        | none                                                 |
| » data             | object  | true | none |        | none                                                 |
| »» id              | integer | true | none |        | 该举报的id                                           |
| »» sender          | integer | true | none |        | 举报者id                                             |
| »» receiver        | integer | true | none |        | 被举报者id                                           |
| »» tid             | integer | true | none |        | 与举报相关的任务id                                   |
| »» category        | integer | true | none |        | 0为恶意审核，1为恶意刷题                             |
| »» status          | integer | true | none |        | 举报当前的状态，0为未审核，1为通过，2为驳回          |
| »» reason          | string  | true | none |        | 举报理由                                             |
| »» credit_variance | integer | true | none |        | （管理员同意后设置）举报通过扣除被举报者的信用分额度 |

## GET 查看举报

GET /reports/get

仅管理员调用，设置筛选条件（可选）获取当前所有举报信息。

> Body 请求参数

```json
{
  "category": 0,
  "status": 1
}
```

### 请求参数

| 名称       | 位置  | 类型    | 必选 | 说明                   |
| ---------- | ----- | ------- | ---- | ---------------------- |
| category   | query | integer | 否   | 为空则返回所有的，下同 |
| status     | query | integer | 否   | none                   |
| body       | body  | object  | 否   | none                   |
| » category | body  | integer | 否   | none                   |
| » status   | body  | integer | 否   | none                   |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "reports": [
      {
        "id": 20,
        "sender": 1,
        "receiver": 2,
        "tid": 3,
        "category": 1,
        "status": 1,
        "reason": "LhlNS",
        "sender_notified": false,
        "receiver_notified": false,
        "credit_variance": 600
      },
      {
        "id": 149,
        "sender": 23,
        "receiver": 54,
        "tid": 65,
        "category": 1,
        "status": 0,
        "reason": "3DA1vD",
        "sender_notified": false,
        "receiver_notified": false,
        "credit_variance": 0
      }
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                  | 类型                      | 必选 | 约束 | 中文名 | 说明                                                 |
| --------------------- | ------------------------- | ---- | ---- | ------ | ---------------------------------------------------- |
| » code                | integer                   | true | none |        | none                                                 |
| » message             | string                    | true | none |        | none                                                 |
| » data                | object                    | true | none |        | none                                                 |
| »» reports            | [[Report](#schemareport)] | true | none |        | none                                                 |
| »»» id                | integer                   | true | none |        | 该举报的id                                           |
| »»» sender            | integer                   | true | none |        | 举报者id                                             |
| »»» receiver          | integer                   | true | none |        | 被举报者id                                           |
| »»» tid               | integer                   | true | none |        | 与举报相关的任务id                                   |
| »»» category          | integer                   | true | none |        | 0为恶意审核，1为恶意刷题                             |
| »»» status            | integer                   | true | none |        | 举报当前的状态，0为未审核，1为通过，2为驳回          |
| »»» reason            | string                    | true | none |        | 举报理由                                             |
| »»» credit_variance   | integer                   | true | none |        | （管理员同意后设置）举报通过扣除被举报者的信用分额度 |
| »»» sender_notified   | boolean                   | true | none |        | 是否已经通知举报者                                   |
| »»» receiver_notified | boolean                   | true | none |        | 是否已经通知被举报者                                 |

## PATCH 处理举报

PATCH /reports/status

仅管理员调用，对举报进行审核。

> Body 请求参数

```json
{
  "rid": 79,
  "status": 1,
  "credit_variance": 100
}
```

### 请求参数

| 名称              | 位置 | 类型    | 必选 | 说明                                   |
| ----------------- | ---- | ------- | ---- | -------------------------------------- |
| body              | body | object  | 否   | none                                   |
| » rid             | body | integer | 是   | none                                   |
| » status          | body | integer | 是   | none                                   |
| » credit_variance | body | integer | 否   | 如果通过举报，管理员设置信用分扣除额度 |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## GET 通知举报

GET /reports/notify

普通用户登录后自动调用，获取所有新处理的举报信息。

> Body 请求参数

```json
{}
```

### 请求参数

| 名称 | 位置 | 类型   | 必选 | 说明 |
| ---- | ---- | ------ | ---- | ---- |
| body | body | object | 否   | none |

> 返回示例

> OK

```json
{
  "code": 20,
  "message": "irure elit qui aliquip",
  "data": {
    "reports": {
      "id": 153,
      "sender": 5,
      "receiver": 6,
      "tid": 2,
      "category": 1,
      "status": 1,
      "reason": "EUZ56C",
      "credit_variance": 8
    }
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                | 类型    | 必选 | 约束 | 中文名 | 说明                                                 |
| ------------------- | ------- | ---- | ---- | ------ | ---------------------------------------------------- |
| » code              | integer | true | none |        | none                                                 |
| » message           | string  | true | none |        | none                                                 |
| » data              | object  | true | none |        | none                                                 |
| »» reports          | object  | true | none |        | none                                                 |
| »»» id              | integer | true | none |        | 该举报的id                                           |
| »»» sender          | integer | true | none |        | 举报者id                                             |
| »»» receiver        | integer | true | none |        | 被举报者id                                           |
| »»» tid             | integer | true | none |        | 与举报相关的任务id                                   |
| »»» category        | integer | true | none |        | 0为恶意审核，1为恶意刷题                             |
| »»» status          | integer | true | none |        | 举报当前的状态，0为未审核，1为通过，2为驳回          |
| »»» reason          | string  | true | none |        | 举报理由                                             |
| »»» credit_variance | integer | true | none |        | （管理员同意后设置）举报通过扣除被举报者的信用分额度 |

## GET 获取举报

GET /reports/self

查看与自己相关的所有举报

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "successt",
  "data": {
    "reports": [
      {
        "id": 158,
        "sender": 1,
        "receiver": 11,
        "tid": 23,
        "category": 1,
        "status": 2,
        "reason": "#Q[Zsn",
        "credit_variance": 10
      },
      {
        "id": 159,
        "sender": 11,
        "receiver": 2,
        "tid": 43,
        "category": 2,
        "status": 1,
        "reason": "#E6%$",
        "credit_variance": 0
      }
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称                | 类型     | 必选 | 约束 | 中文名 | 说明                                                 |
| ------------------- | -------- | ---- | ---- | ------ | ---------------------------------------------------- |
| » code              | integer  | true | none |        | none                                                 |
| » message           | string   | true | none |        | none                                                 |
| » data              | object   | true | none |        | none                                                 |
| »» reports          | [object] | true | none |        | none                                                 |
| »»» id              | integer  | true | none |        | 该举报的id                                           |
| »»» sender          | integer  | true | none |        | 举报者id                                             |
| »»» receiver        | integer  | true | none |        | 被举报者id                                           |
| »»» tid             | integer  | true | none |        | 与举报相关的任务id                                   |
| »»» category        | integer  | true | none |        | 0为恶意审核，1为恶意刷题                             |
| »»» status          | integer  | true | none |        | 举报当前的状态，0为未审核，1为通过，2为驳回          |
| »»» reason          | string   | true | none |        | 举报理由                                             |
| »»» credit_variance | integer  | true | none |        | （管理员同意后设置）举报通过扣除被举报者的信用分额度 |

## POST 完成通知

POST /reports/notified

前端每给用户发一个notification后调用，将report对于该用户的状态设为notified。

> Body 请求参数

```json
{
  "rid": 98
}
```

### 请求参数

| 名称  | 位置 | 类型    | 必选 | 说明   |
| ----- | ---- | ------- | ---- | ------ |
| body  | body | object  | 否   | none   |
| » rid | body | integer | 是   | 举报id |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

# 公告接口

## GET 查看公告

GET /notices/view

查看公告，top_k 是一个可选的query参数，指定只获取最新发布的 top_k 条公告，否则返回所有公告。
未登录用户也可以调用此接口。

### 请求参数

| 名称  | 位置  | 类型    | 必选 | 说明                             |
| ----- | ----- | ------- | ---- | -------------------------------- |
| top_k | query | integer | 否   | 可选参数，指定要查看的公告的条数 |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "notices": [
      {
        "id": 4,
        "title": "用户公约",
        "content": "labore",
        "publish_time": "2013-01-21 09:47:25",
        "sender": 12
      },
      {
        "id": 47,
        "title": "标注方温馨提示",
        "content": "veniam proident sunt sed dolore",
        "publish_time": "2001-09-25 00:44:11",
        "sender": 94
      },
      {
        "id": 31,
        "title": "管理员招募",
        "content": "et est adipisicing dolore",
        "publish_time": "2002-08-18 04:23:55",
        "sender": 62
      }
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称             | 类型     | 必选 | 约束 | 中文名 | 说明                    |
| ---------------- | -------- | ---- | ---- | ------ | ----------------------- |
| » code           | integer  | true | none |        | none                    |
| » message        | string   | true | none |        | none                    |
| » data           | object   | true | none |        | none                    |
| »» notices       | [object] | true | none |        | none                    |
| »»» id           | integer  | true | none |        | none                    |
| »»» title        | string   | true | none |        | none                    |
| »»» content      | string   | true | none |        | none                    |
| »»» publish_time | string   | true | none |        | none                    |
| »»» sender       | integer  | true | none |        | 发布者的uid，只能是超管 |

## POST 发布公告

POST /notices/post

发布公告，只能由超管进行。

> Body 请求参数

```json
{
  "title": "用户公约",
  "content": "互联网不是法外之地，使用本平台的过程中请遵守相关法律法规。"
}
```

### 请求参数

| 名称      | 位置 | 类型   | 必选 | 说明     |
| --------- | ---- | ------ | ---- | -------- |
| body      | body | object | 否   | none     |
| » title   | body | string | 是   | 公告标题 |
| » content | body | string | 是   | 公告内容 |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## POST 删除公告

POST /notices/delete/{nid}

删除指定的公告，只能由超管进行。

### 请求参数

| 名称 | 位置 | 类型    | 必选 | 说明 |
| ---- | ---- | ------- | ---- | ---- |
| nid  | path | integer | 是   | none |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

# 银行卡接口

## POST 添加银行卡

POST /bankcards/post

超管调用，在数据库中添加一张银行卡。

> Body 请求参数

```json
{
  "number": "1234567890987654321",
  "password": "123456",
  "balance": 75
}
```

### 请求参数

| 名称       | 位置 | 类型    | 必选 | 说明       |
| ---------- | ---- | ------- | ---- | ---------- |
| body       | body | object  | 否   | none       |
| » number   | body | string  | 是   | 银行卡卡号 |
| » password | body | string  | 是   | 银行卡密码 |
| » balance  | body | integer | 是   | 银行卡余额 |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "bankcard": {
      "number": "7591687250464716828",
      "balance": 1000
    }
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称        | 类型    | 必选 | 约束 | 中文名 | 说明                                          |
| ----------- | ------- | ---- | ---- | ------ | --------------------------------------------- |
| » code      | integer | true | none |        | 银行卡卡号                                    |
| » message   | string  | true | none |        | 银行卡密码                                    |
| » data      | object  | true | none |        | 银行卡总余额                                  |
| »» bankcard | object  | true | none |        | none                                          |
| »»» number  | string  | true | none |        | patttern为19位数字，数据库中以此为primary_key |
| »»» balance | integer | true | none |        | 银行卡余额                                    |

## POST 绑定银行卡

POST /bankcards/bind

用户调用此接口，输入卡号和密码绑定银行卡

> Body 请求参数

```json
{
  "number": "3518965958976656876",
  "password": "335814"
}
```

### 请求参数

| 名称       | 位置 | 类型   | 必选 | 说明     |
| ---------- | ---- | ------ | ---- | -------- |
| body       | body | object | 否   | none     |
| » number   | body | string | 是   | 银行卡号 |
| » password | body | string | 是   | 银行密码 |

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## POST 解绑银行卡

POST /bankcards/unbind

用户解绑当前账户绑定的银行卡。

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {}
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## DELETE 删除银行卡

DELETE /bankcards/delete

删除一张银行卡，已绑定此银行卡的用户全部解绑。

> Body 请求参数

```json
{
  "number": "3873557344891813482"
}
```

### 请求参数

| 名称     | 位置 | 类型   | 必选 | 说明 |
| -------- | ---- | ------ | ---- | ---- |
| body     | body | object | 否   | none |
| » number | body | string | 是   | none |

> 返回示例

> OK

```json
{
  "code": 200,
  "data": {},
  "message": "success"
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## PATCH 修改卡密

PATCH /bankcards/password

超管修改银行卡密码。

> Body 请求参数

```json
{
  "old_password": "126441",
  "new_password": "035457",
  "number": "0224709917927862582"
}
```

### 请求参数

| 名称           | 位置 | 类型   | 必选 | 说明               |
| -------------- | ---- | ------ | ---- | ------------------ |
| body           | body | object | 否   | none               |
| » old_password | body | string | 是   | 旧的银行卡密码     |
| » new_password | body | string | 是   | 修改后的银行卡密码 |
| » number       | body | string | 是   | 银行卡号           |

> 返回示例

> OK

```json
{
  "code": 200,
  "data": {},
  "message": "success"
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## PATCH 修改余额

PATCH /bankcards/balance

超管修改银行卡余额。

> Body 请求参数

```json
{
  "balance": 37,
  "number": "0342596869823843106"
}
```

### 请求参数

| 名称      | 位置 | 类型    | 必选 | 说明               |
| --------- | ---- | ------- | ---- | ------------------ |
| body      | body | object  | 否   | none               |
| » balance | body | integer | 是   | 修改后银行余额的值 |
| » number  | body | string  | 是   | none               |

> 返回示例

> OK

```json
{
  "code": 200,
  "data": {},
  "message": "success"
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称      | 类型    | 必选 | 约束 | 中文名 | 说明 |
| --------- | ------- | ---- | ---- | ------ | ---- |
| » code    | integer | true | none |        | none |
| » message | string  | true | none |        | none |
| » data    | object  | true | none |        | none |

## GET 获取银行卡

GET /bankcards/ownerView

银行卡绑定用户查看银行卡余额。

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "bankcard": {
      "number": "4024959986922881525",
      "balance": 635029920871842
    }
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称        | 类型    | 必选 | 约束 | 中文名 | 说明                                          |
| ----------- | ------- | ---- | ---- | ------ | --------------------------------------------- |
| » code      | integer | true | none |        | none                                          |
| » message   | string  | true | none |        | none                                          |
| » data      | object  | true | none |        | none                                          |
| »» bankcard | object  | true | none |        | 如果没有绑定银行卡，卡号返回空，余额返回0     |
| »»» number  | string  | true | none |        | patttern为19位数字，数据库中以此为primary_key |
| »»» balance | integer | true | none |        | 银行卡余额                                    |

## GET 查看银行卡

GET /bankcards/rootView

超管看所有的银行卡。

> 返回示例

> OK

```json
{
  "code": 200,
  "message": "success",
  "data": {
    "bankcards": [
      {
        "number": "6391379712571042405",
        "password": "806096",
        "owners": [
          43,
          23,
          433
        ],
        "balance": 1231
      },
      {
        "number": "4805823523331414626",
        "password": "345494",
        "owners": [
          574,
          34,
          87
        ],
        "balance": 1
      },
      {
        "number": "6532868631877804421",
        "password": "836988",
        "owners": [
          23,
          689,
          456,
          233
        ],
        "balance": 0
      }
    ]
  }
}
```

### 返回结果

| 状态码 | 状态码含义                                              | 说明 | 数据模型 |
| ------ | ------------------------------------------------------- | ---- | -------- |
| 200    | [OK](https://tools.ietf.org/html/rfc7231#section-6.3.1) | OK   | Inline   |

### 返回数据结构

状态码 **200**

| 名称         | 类型                          | 必选 | 约束 | 中文名 | 说明                                          |
| ------------ | ----------------------------- | ---- | ---- | ------ | --------------------------------------------- |
| » code       | integer                       | true | none |        | none                                          |
| » message    | string                        | true | none |        | none                                          |
| » data       | object                        | true | none |        | none                                          |
| »» bankcards | [[Bankcard](#schemabankcard)] | true | none |        | none                                          |
| »»» number   | string                        | true | none |        | patttern为19位数字，数据库中以此为primary_key |
| »»» password | string                        | true | none |        | pattern为6位数字                              |
| »»» balance  | integer                       | true | none |        | 银行卡余额                                    |
| »»» owners   | [integer]                     | true | none |        | none                                          |

# 数据模型

## Bankcard

<a id="schemabankcard"></a>
<a id="schema_Bankcard"></a>
<a id="tocSbankcard"></a>
<a id="tocsbankcard"></a>

```json
{
  "number": "string",
  "password": "string",
  "balance": 0,
  "owners": [
    0
  ]
}

```

### 属性

| 名称     | 类型      | 必选 | 约束 | 中文名 | 说明                                          |
| -------- | --------- | ---- | ---- | ------ | --------------------------------------------- |
| number   | string    | true | none |        | patttern为19位数字，数据库中以此为primary_key |
| password | string    | true | none |        | pattern为6位数字                              |
| balance  | integer   | true | none |        | 银行卡余额                                    |
| owners   | [integer] | true | none |        | none                                          |

## Notice

<a id="schemanotice"></a>
<a id="schema_Notice"></a>
<a id="tocSnotice"></a>
<a id="tocsnotice"></a>

```json
{
  "id": 0,
  "title": "string",
  "content": "string",
  "publish_time": "string",
  "sender": 0
}

```

### 属性

| 名称         | 类型    | 必选 | 约束 | 中文名 | 说明                    |
| ------------ | ------- | ---- | ---- | ------ | ----------------------- |
| id           | integer | true | none |        | none                    |
| title        | string  | true | none |        | none                    |
| content      | string  | true | none |        | none                    |
| publish_time | string  | true | none |        | none                    |
| sender       | integer | true | none |        | 发布者的uid，只能是超管 |

## Report

<a id="schemareport"></a>
<a id="schema_Report"></a>
<a id="tocSreport"></a>
<a id="tocsreport"></a>

```json
{
  "id": 0,
  "sender": 0,
  "receiver": 0,
  "tid": 0,
  "category": 0,
  "status": 0,
  "reason": "string",
  "credit_variance": 0,
  "sender_notified": true,
  "receiver_notified": true
}

```

### 属性

| 名称              | 类型    | 必选 | 约束 | 中文名 | 说明                                                 |
| ----------------- | ------- | ---- | ---- | ------ | ---------------------------------------------------- |
| id                | integer | true | none |        | 该举报的id                                           |
| sender            | integer | true | none |        | 举报者id                                             |
| receiver          | integer | true | none |        | 被举报者id                                           |
| tid               | integer | true | none |        | 与举报相关的任务id                                   |
| category          | integer | true | none |        | 0为恶意审核，1为恶意刷题                             |
| status            | integer | true | none |        | 举报当前的状态，0为未审核，1为通过，2为驳回          |
| reason            | string  | true | none |        | 举报理由                                             |
| credit_variance   | integer | true | none |        | （管理员同意后设置）举报通过扣除被举报者的信用分额度 |
| sender_notified   | boolean | true | none |        | 是否已经通知举报者                                   |
| receiver_notified | boolean | true | none |        | 是否已经通知被举报者                                 |

## Worker

<a id="schemaworker"></a>
<a id="schema_Worker"></a>
<a id="tocSworker"></a>
<a id="tocsworker"></a>

```json
{
  "username": "string",
  "id": 0,
  "role": 0,
  "password": "string",
  "token": "string",
  "banned": true,
  "credits": 0,
  "points": 0,
  "bankcard_number": "string",
  "invitation_code": "string",
  "email": "string",
  "extra_info": "string",
  "preferred_categories": [
    {
      "accept_count": 0,
      "reject_count": 0
    }
  ],
  "accepted_tasks": [
    {
      "status": 0
    }
  ],
  "points_earned_by_laboring": 0
}

```

### 属性

| 名称                      | 类型     | 必选 | 约束 | 中文名 | 说明                                                         |
| ------------------------- | -------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| username                  | string   | true | none |        | 用户名                                                       |
| id                        | integer  | true | none |        | 用户id                                                       |
| role                      | integer  | true | none |        | 用户角色，1为超级管理员，2为管理员，3为需求方，4为标注方，5为中介 |
| password                  | string   | true | none |        | 用户密码                                                     |
| token                     | string   | true | none |        | 登录token                                                    |
| banned                    | boolean  | true | none |        | 是否被封禁                                                   |
| credits                   | integer  | true | none |        | 用户信用分                                                   |
| points                    | integer  | true | none |        | 用户积分                                                     |
| bankcard_number           | string   | true | none |        | 银行卡号，为19位数字                                         |
| invitation_code           | string   | true | none |        | 邀请码，6位，由大写字母和数字构成                            |
| email                     | string   | true | none |        | 用户邮箱                                                     |
| extra_info                | string   | true | none |        | 额外信息                                                     |
| preferred_categories      | [object] | true | none |        | 列表长度和已有的任务模板数相同                               |
| » accept_count            | integer  | true | none |        | 接受过此任务的总数                                           |
| » reject_count            | integer  | true | none |        | 拒绝过此任务的总数                                           |
| accepted_tasks            | [object] | true | none |        | 此标注方接受过的任务列表                                     |
| » status                  | integer  | true | none |        | 0-未完成；1-待审核；2-合格；3-不合格；4-超时                 |
| points_earned_by_laboring | integer  | true | none |        | 通过完成任务获得的积分                                       |

## Task

<a id="schematask"></a>
<a id="schema_Task"></a>
<a id="tocStask"></a>
<a id="tocstask"></a>

```json
{
  "name": "string",
  "id": 0,
  "status": 0,
  "created_time": "string",
  "duration": 0,
  "time_limit": 0,
  "category": 0,
  "distribute_user_list": [
    0
  ],
  "dataset_size": 0,
  "assign_num": 0,
  "reward": 0,
  "assigner": 0,
  "thread_list": [
    {
      "id": 0,
      "accept_time": "string",
      "completed_num": 0,
      "review_status": 0,
      "auto_check_score": 0
    }
  ],
  "credit_lower_bound": 0,
  "rejecter_list": [
    0
  ],
  "content_path": "string",
  "provide_test": true,
  "entrust_intermediary": true,
  "intermediary_id": 0,
  "intermediary_commmission": 0
}

```

### 属性

| 名称                     | 类型      | 必选 | 约束 | 中文名 | 说明                                                         |
| ------------------------ | --------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| name                     | string    | true | none |        | 任务名称                                                     |
| id                       | integer   | true | none |        | 任务id                                                       |
| status                   | integer   | true | none |        | 任务状态，0-6分别表示未定义、管理员审核中、招募中、进行中、已完成、无法分配、审核结果中 |
| created_time             | string    | true | none |        | 任务创建时间                                                 |
| duration                 | integer   | true | none |        | 容许的标注方总用时                                           |
| time_limit               | integer   | true | none |        | 单个任务时间限制                                             |
| category                 | integer   | true | none |        | 任务类型，0-8分别表示未定义、文本标注、图片标注、语音标注、视频标注、图片框选、骨骼打点、实体三元组标注、审核任务 |
| distribute_user_list     | [integer] | true | none |        | 任务分配用户列表                                             |
| dataset_size             | integer   | true | none |        | 数据集中条目的个数                                           |
| assign_num               | integer   | true | none |        | 需要分配的标注方的数量                                       |
| reward                   | integer   | true | none |        | 每道题奖励的积分量                                           |
| assigner                 | integer   | true | none |        | 任务发布者id                                                 |
| thread_list              | [object]  | true | none |        | 每一项代表一个标注者的进度等信息                             |
| » id                     | integer   | true | none |        | 标注者id                                                     |
| » accept_time            | string    | true | none |        | 标注者接受任务的时间                                         |
| » completed_num          | integer   | true | none |        | 标注者已完成的条目数（用于向需求方显示进度以及确定标注方需要标注的下一条数据） |
| » review_status          | integer   | true | none |        | 0-未完成；1-待审核；2-合格; 3-不合格; 4-超时                 |
| » auto_check_score       | number    | true | none |        | 浮点数，自动检查得到的正确率                                 |
| credit_lower_bound       | integer   | true | none |        | 任务分配需求方的信用分下限                                   |
| rejecter_list            | [integer] | true | none |        | 已拒绝此任务的标注方列表，分发是不会再分发给这些用户         |
| content_path             | string    | true | none |        | 数据集文件路径                                               |
| provide_test             | boolean   | true | none |        | 需求方是否提供测试集                                         |
| entrust_intermediary     | boolean   | true | none |        | 是否委托中介                                                 |
| intermediary_id          | integer   | true | none |        | 委托的中介id                                                 |
| intermediary_commmission | integer   | true | none |        | 中介委托费，以积分为单位                                     |

# UserEntry

<a id="schemauserentry"></a>
<a id="schema_UserEntry"></a>
<a id="tocSuserentry"></a>
<a id="tocsuserentry"></a>

```json
{
  "username": "string",
  "id": 0,
  "role": 0,
  "password": "string",
  "token": "string",
  "banned": true,
  "credits": 0,
  "points": 0,
  "bankcard_number": "string",
  "invitation_code": "string",
  "email": "string",
  "extra_info": "string"
}

```

### 属性

| 名称            | 类型    | 必选 | 约束 | 中文名 | 说明                                                         |
| --------------- | ------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| username        | string  | true | none |        | 用户名                                                       |
| id              | integer | true | none |        | 用户id                                                       |
| role            | integer | true | none |        | 用户角色，1为超级管理员，2为管理员，3为需求方，4为标注方，5为中介 |
| password        | string  | true | none |        | 用户密码                                                     |
| token           | string  | true | none |        | 登录token                                                    |
| banned          | boolean | true | none |        | 是否被封禁                                                   |
| credits         | integer | true | none |        | 用户信用分                                                   |
| points          | integer | true | none |        | 用户积分                                                     |
| bankcard_number | string  | true | none |        | 银行卡号，为19位数字                                         |
| invitation_code | string  | true | none |        | 邀请码，6位，由大写字母和数字构成                            |
| email           | string  | true | none |        | 用户邮箱                                                     |
| extra_info      | string  | true | none |        | 额外信息                                                     |

