## 开发规范(来自yjx的记录)

1. dev从master拉取，dev和master都只能被merge，不要push进去
2. 不要merge进master除非是迅速改bug，可以拉取一个分支并且merge进去，正确的做法是merge进dev，确定dev没有问题才能merge dev => master
3. 一定维护master的稳定性，不稳定测试和开发在dev上进行，保证演示不出bug
4. 每一个commit后面#加上issue

## 故障速查

1. 确保当前正在访问的页面不是cached的页面（disable cache/无痕模式）
2. 检查报错信息正误

## Issues

- [ ] 前端注册用户名带'-'
- [ ] 发布任务后，在管理员审核之前，发布方看到的任务状态为“空”
- [ ] 被封禁的用户依然会被分发(expected?)
- [ ] manager页面的返回操作
- [ ] users/intermediary/dotask 的上边栏
- [ ] 中介委托逻辑
- [ ] upperFrame requirement
- [ ] 委托中介费用不能为0

## apifox补充

tasks.status:

- PENDING = 1 需求方未确认
- RECRUITING = 2 招募中
- IN PROGRESS = 3 满人了，正在标
- COMPLETED = 4 trivial

## Hints

- 前端master已开启分支保护 请使用merge向主分支提交信息
- 可将todo分成gitlab上的issue（然后可以assign给自己表示认领这个任务，当然assign别人也可行），可以开PR时引用该issue
- commit规范参考：https://www.conventionalcommits.org/en/v1.0.0/

### 调试

- sep容器管理里面可以看log
- 输出调试：`console.log(...)`
- 由于CORS问题，前端反向代理是必须的。每次build完用nginx反代太耗时间，但nuxt似乎不大支持rewrites功能，在考虑用middleware实现或找现成轮子。

## 待讨论问题

1. 关于动态路由的问题 我看到vue-router要加':'表示动态，但是nuxt有file-based routing，使用navigateTo时不必单独加':'

现在不统一之后盖起来
如果我没理解错jxgg的思路的话，我目前觉得是有三条路

- 就按现在这样，navigateTo然后route.$params.var.slice(1)拼接的时候把':'删掉（又不是不能用.jpg
- 走nuxt路线 直接上navigate不加':'
- 走vue router路线 用router.push实现跳转

2. 已完成任务是否开放“审”的按钮？改成查看？

## Highlights

- [ ] ELMessage替代alert
- [ ] useFetch(vueuse) + vue Suspense进行async加载
- [ ] nuxt faster dev start
- [ ] ModifyTask 任务缓存设置
- [ ] TaskPost 更新中介选项内容

---

- [x] Basis
    - [x] **实现nuxt反向代理**
    - [x] nuxi typecheck
    - [x] eslint
    - [x] sonarqube
    - [x] async函数使用 （待参考小作业）
    - [x] master dev 双部署
- [x] APIs
    - [x] 需求方/发布方识别 考虑因素：
        - [x] ~~前端发送用户类别让后端判断是否正确，还是让后端返回用户类型？~~ 后端API会返回role
        - [x] 前端是否分开注册，是否分开登录
    - [x] ~~文档中权限申请的含义？一个用户是否可以身兼数职~~ 一用户一身份，管理员审核需求方发布的内容
    - [x] 页面鉴权（前端可使用auth middleware配置）后端是否需要一个接口判断该用户是否有权限访问该页面
    - [x] 登录/注册页错误处理/显示
        - [ ] 页面UI：或可snackbar
        - [x] 错误信息等后端API？
        - [x] CORS？
- [x] Pages
    - [x] CSS选型 ~~ChatWJS or UI组件库~~ Element ui plus
    - [x] 不同管理人员的登录界面？


## 测试架构

测试框架[vitest](https://vitest.dev)，使用[nuxt-vitest](https://github.com/danielroe/nuxt-vitest)与nuxt进行整合。  
组件框架@vue/test-utils，想调试组件内逻辑必须品，使用文档[这里看](https://test-utils.vuejs.org)  
E2E框架目前没有（看上去不要求x

建议参考:

- https://vuejs.org/guide/scaling-up/testing.html#testing
- [vitest](https://vitest.dev)
- [@vue/test-utils](https://test-utils.vuejs.org)

测试库会识别仓库内所有的*.tests.(ts+js)或*.spec.(ts+js)文件，但是建议把测试文件放在tests/下或者和组件在一起  
示例：tests_sy分支下的`~/tests/components/TaskPresent.spec.ts`

## Spec

### 任务分发设计说明
模型的调整：  
在 task 模型中增加了 distribute_user_list 字段，记录了所有可见该任务的需求方的uid，这个数组的长度总是等于任务所需人数（除了标注方人数不足，任务处于 zombie 状态的特殊情况，以及数据库中现有的按照没有该字段的原设计所保存的任务）。前端在向标注方推送任务前需逐任务加一检查。  
Task 模型还增加了 credit_lower_bound 字段，表示任务分发时对标注方信用分的限制。这个字段会在任务 confirm 时确定，后续对于 recruiting/zombie 状态的任务可以通过 modifyCreditRestriction 接口修改（见API文档）。confirm 请求也略有调整，可以在 body 中传入一个键为“credit_lower_bound”的整形数作为分发信用分限制，为了做到向前兼容，body中也可以不含这个值，此时视为使用默认的信用分阈值 90.     
Task 的 status 增加了一种状态 zombie（编号为5），表示在上一次检查时发现，按照该任务的信用分限制以及已经拒绝过的标注方的情况，即使考虑平台上所有标注方，也不可能有足够的标注方可以进行分发。需求方可以尝试调用两个请求来尝试使 zombie 状态的任务进入 recruiting 状态（见下）。  

在以下时机可能涉及任务分发目标（即 distribute_user_list）的变化：  

1. 任务确认时。任务确认时会按照 credit_lower_bound 尝试确定一批分发对象（顺序分发），如果此时就出现了标注方用户不足的情况，confirm 请求不会成功，任务将仍保持在 pending 状态，会返回给前端一个错误，提示需求方尝试降低 credit_lower_bound 进行 confirm。  
2. 有标注方用户拒绝了任务，或超时，或标注结果被拒绝。后端会尝试按照 credit_lower_bound 递补分发对象。但这个递补可能由于已经没有足够的信用分够高的标注方用户而失败，此时任务状态会被置为 zombie。  
3. 需求方尝试干预分发。
    1. 对于处于 recruiting 状态的任务，需求方可以使用 modifyCreditRestriction 请求调整信用分限制（不会立竿见影地影响分发对象列表，但会影响之后的分发递补等行为），也可以使用 refreshDistribution 请求尝试更新一批分发对象（已经接受任务的标注方不会受到影响，被更新掉的标注方不会被视为拒绝了这个任务，且这个请求不会引发标注方不足的问题，因为标注方不足时会只替换一部分分发对象）。
    2. 对于处于 zombie 状态的任务，考虑到需求方可以降低 credit 限制，而且平台可能会有新注册的标注方用户，以及可能会有标注方用户信用分升高，达到被分发的标注，因此需求方可以尝试通过两个请求来分发 zombie 任务。其一是需求方可以使用 modifyCreditRestriction 请求调整信用分限制，对于 zombie 任务，调用该请求后会立即用新标准试图进行分发，因此任务有可能立即从 zombie 状态转为 recruiting；其二是需求方可以调用refreshZombie请求来尝试检查按照现有的 credit 标准有没有新的、足够多的符合分发标准的标注方，从而尝试将任务从 zombie 状态转为 recruiting，如果失败，会返回一个提示需求方先降低信用分标准的错误。  


### 中介
1. task模型中增加bool类型的entrust_intermediary字段，需要在confirm的时候设置；增加intermediary_id字段，若任务交给中介且中介接单后设置为中介id。
2. 任务增加intermediary_commision字段，default为0，若需要委托中介则需设置，表示给中介的提成。并且平台会固定抽成一定reward的金额，需要前端展示一下抽成积分金额，后端计算的时候check需求方积分余额是否不小于平台抽成+中介抽成+需求方总抽成的值。
3. 若需求方选择委托中介分发，则不可调用refreshDistribution、refreshZombie接口，完全交给中介负责。
4. 中介可查看所有被委托的任务（/tasks/getEntrustedTasks）并选择接单(/tasks/receiveOrder)，可以查看自己接的单（/tasks/getOrders）。
5. 简单起见，不可更换被委托的中介，中介接单的任务完成后就才可获得需求方提供的抽成积分。
6. 中介可以查看当前的worker(/users/intermediaryViewWorkers)，手动选择标注方对接单的任务进行分发(tasks/intermediaryDistribute)，分发的用户不可超过assign_num。若没有足够的用户可供分发，则需要中介一段时间后再查看并分发以确保任务完成，后端不会对此进行处理。
一点需要修改的地方：
积分报酬不能为0，不管是给标注方还是给中介


## 实现架构


### 代码架构

现有架构见gitlab

#### users/

##### login/
##### register/

#### tasks/

##### [id]/

###### modify
###### upload

##### Establish