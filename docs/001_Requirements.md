# Requirements

| 序号 | 版本号 | 修改日期   | 备注     |
| ---- | ------ | ---------- | -------- |
| 1    | 1.0    | 2022.10.11 | 初始版本 |
|      |        |            |          |

[TOC]

## 1 Goal and Scope

本文档的目的是为 STMS 项目的设计和开发提供清晰的目标，目标读者是 STMS 项目的设计者及后续开发者。

本文档描述并分析 STMS 项目需求。除必要情况，本文档将尽量避免描述具体设计，以免为后续设计工作造成不合理的约束。

另外，本文档描述的需求，或许不会在第一个版本实现，但可能在后续版本中实现。

## 2 Functional Requirements

### 2.1 Overview

本软件定义为：地铁售票与管理软件。主要面向两类使用者：地铁用户与地铁管理员。其中，地铁管理员又分为：CSS（customer service staff，客服人员）和 Root（最高管理员）。因此，将功能性需求拆解为三部分给予实现：

- 用户需求
- CSS 管理员需求
- Root 管理员需求

> 注：将管理员分为两类分别设计，目的在于将客服类管理员与最高管理员进行区别，避免给予其不必要的、危险的权限。

下面将对这三部分需求分点进行详述。每一点前的标识，代表此项需求的优先级：

- 【H】：高优先级，应在软件系统的第一个版本实现
- 【M】：中优先级，应在软件系统的后续版本实现
- 【L】：低优先级，选择性实现

### 2.3 User Requirements

#### 2.3.1 Account Management

- [ ] **【H】创建账户 / 销户**
  - 创建账户：输入用户昵称、手机号、身份证号（或其他有效证件号）、登录密码，从而创建账户
  - 销户：显示账户信息，二次确认后销户。要求账户余额为空

- [ ] **【H】用户登录 / 登出**
  - 使用手机验证登录：输入手机号，点击验证（暂时采用虚拟验证）
  - 使用密码登录：输入手机号及密码登录
  - 用户登出

- [ ] **【H】账户信息查询**
  - 显示账户信息：输出信息示例如下：
  
    | 用户昵称：     | CSUer                          |
    | -------------- | ------------------------------ |
    | 绑定手机号：   | * * * * * * * 3942             |
    | 绑定身份证号： | 510 * * * * * * * * * * * 0316 |
    | 创建日期：     | 2022.10.10                     |
  
- [ ] **【H】更改密码**

  - 更改密码：输入原密码，输入两次新密码，进行手机验证，可更改密码

#### 2.3.2 Daily Cash

- [ ] **【H】绑定或解绑银行卡 / 微信**

  - 绑定银行卡：输入银行卡号，自动识别银行，点击确认，之后输入银行卡密码，之后进行手机验证（虚拟验证）

  - > 注：可同时绑定多张银行卡

  - 绑定微信：输入手机号，之后进行手机验证（虚拟验证）

  - 解绑银行卡：点击解绑银行卡，显示银行卡信息，选择需要解绑的银行卡，二次确认

  - 解绑微信：点击解绑微信，二次确认

- [ ] **【H】账户充值 / 提现**

  - 从绑定的银行卡充值：未绑定银行卡，提醒其绑定。绑定了银行卡，输入充值金额，输入银行卡密码，充值成功
  - 绑定了微信充值：输入充值金额，输入微信支付密码，充值成功
  - 未绑定微信充值：输入微信账户手机号，输入微信支付密码，充值成功
  - 提现到银行卡
  - 提现到微信

- [ ] **【H】办理套餐**

  - 显示套餐类型：显示套餐类型及起止期限

  - 办理周卡：十次，15元，从余额扣除。可选择是否自动续费

  - 办理月卡1：三十次，45元，从余额扣除。可选择是否自动续费

  - 办理月卡2：不限次，60元，从余额扣除。可选择是否自动续费

  - 办理年卡：不限次，600元，从余额扣除。可选择是否自动续费

  - > 注：提醒用户，办理套餐后，原套餐（若存在）立即作废

- [ ] **【H】显示钱包信息**

  - 显示钱包信息：输出信息示例如下：

    | 账户余额   | 101.0                     |
    | ---------- | ------------------------- |
    | 套餐信息   | 年卡会员                  |
    |            | 2022.10.11 ~ 2023.10.11   |
    |            | 自动续费                  |
    | 绑定银行卡 | 工商银行                  |
    |            | 1234 * * * * * * * * 5678 |
    | 绑定微信   | 未绑定                    |

- [ ] **【H】显示交易流水**
  
  - 显示交易流水：输出信息示例如下：
  
    | **交易金额** | **交易类型** | **交易时间** |
    | ------------ | ------------ | ------------ |
    | -    600.0   | 办理套餐     | 2022.10.11   |
    | +   500.0    | 充值         | 2022.10.11   |
    | -    1.7     | 购票         | 2022.10.08   |
    | +   2.7      | 退票         | 2022.10.01   |
    | -    2.7     | 购票         | 2022.10.01   |

#### 2.3.3 Ticket Service

- [ ] **【H】自动推荐线路 / 购票 / 退票服务**

  - 自动推荐线路：输入出发站和目的站，自动推荐线路，输出票价。输出信息示例如下：

    | 线路推荐              | 预计用时 | 预计拥挤情况 |
    | --------------------- | -------- | ------------ |
    | A站 =1=> B站 =2=> D站 | 46分钟   | 十分拥挤     |
    | A站 =3=> C站 =2=> D站 | 54分钟   | 良好         |
    | **票价**              |          |              |
    | 2.7 元                |          |              |

  - > 注：预计用时与经过的每个站点用时有关，同时加上预计等待换乘时间；拥挤情况与乘坐的线路有关，应通过数值计算得出；票价只与出发站和目的站相关

  - 购票服务：获得推荐线路后，点击立即购票。扣款成功后，输出信息示例如下：

    | 出发站 | 目的站 | 票价   | 使用期限   |
    | ------ | ------ | ------ | ---------- |
    | A站    | B站    | 2.7 元 | 2022.10.10 |

  - 退票服务：当天购买的地铁票，若状态为未使用，可在当天24:00前进行退票

- [ ] **【H】显示购票记录**

  - 显示购票记录：输出信息示例如下：

    | 日期       | 出发站 | 目的站 | 支付票价 | 使用状态 |
    | ---------- | ------ | ------ | -------- | -------- |
    | 2022.10.11 | A站    | B站    | 1.7 元   | 待使用   |
    | 2022.10.10 | B站    | A站    | 1.7 元   | 已使用   |
    | 2022.10.09 | B站    | A站    | 1.7 元   | 已退款   |
    | 2022.10.08 | B站    | A站    | 1.7 元   | 已过期   |

  - 依据日期显示购票记录：用户可通过购票日期进行筛选，输出信息格式同上

#### 2.3.4 Complain

- [ ] **【L】提交申诉 / 建议**
  - 提交申诉：对账户余额、购票信息等存在疑虑的，可以向管理员提出申诉
  - 提交建议：对地铁管理、本软件的建议，可以向管理员反馈
- [ ] **【L】查看申诉进度及反馈**
  - 查看申诉进度及反馈：可查看申诉进度及反馈

### 2.4 CSS Requirements

### 2.5 Root Requirements

## 3 Interaction Requirements

## 4 Performance Requirements

## 5 Security Requirements

## 6 Testability Requirements

## 7 Other Requirements

## Appendix

### Demand Book Summary

**课设题目：**

某市地铁售票管理与客流量预测系统

**基本要求：**

[1] 编写某市地铁售票管理系统，实现对地铁售票的管理，能够根据售票记录预测不同时间段的乘车人数，提示错峰出行。系统须支持多条地铁线路、多趟车次、多用户购票，记录地铁线路信息、车次信息，以及用户的购票信息。

[2] 用户有不同的类型，比如：普通用户、月卡用户、年票用户，每种用户购票的折扣不同。用户的信息根据需要自己确定。定义地铁的线路，每条线路包含：代码、名称、经过的站点、承载的车次、运载人数等信息，其中每个运行时段乘客量的预测通过成员函数实现。车票信息包括以下信息：车次、始发地、目的地、乘车区间、车票价格、发车时间等。

**基本管理功能：**

[1] 购票管理：针对车票信息进行管理。包括：

- 购票：添加车票信息。
- 显示：根据购票时间显示某一时间段的购票信息。
- 退票：删除相应车票信息。

[2] 用户管理：主要是针对月卡用户和年卡用户，包括：

- 买卡：用户添加，增加月卡用户和年卡用户；
- 续费：修改其中的余额；
- 退卡：删除用户；
- 显示：显示当前持卡用户信息；
- 查询：根据卡号查询相应的余额等信息

[3] 地铁线路管理：

- 添加：为某条线路增加车次；
- 删除：为某条线路删除车次；
- 显示：显示某条线路处于运行状态的车次。

[4] 统计功能：根据需要设计合理的统计功能，比如：每天乘客的数量、发车次数、运行收入等。

[5] 退出功能：要求点击退出，可以退出系统。

**其他要求及说明：**

[1] 需要定义的类、类的成员变量、成员函数不限于以上的要求，根据合理的设想自行设计。需要设计不同类型的用户，不少于三条地铁线路；

[2] 由于需要预测某时段是否高峰期或者旅客人数，需要保留尽量多的历史数据。可以根据历史上某个时段的人数采用时间序列方法进行乘客人数的预测，根据人数提示当前是否高峰期；或者对不同时段的乘客人数进行统计，根据统计将每个时段分类到不同的情况下，比如高峰、即将到达高峰、人数较少等。

[3] 鼓励大家设计更多的系统功能，比如显示线路、路径推荐等。

> 注：考虑到实际情况，本文档描述的需求对原需求进行了部分修改