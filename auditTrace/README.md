# 审计追溯

## 1. 介绍

### 1.1 审计追溯与 21 CFR

- 21 CFR Part 11 – 分类 B 电子批记录
  - 需有系统自动生成的,安全的,带时间戳的独立审计追溯,以记录操作员进入系统和操作(创建,编辑,删除)电子记录的的日期和时间等.
  - 修改的记录不允许覆盖之前的记录.
  - 审计追溯信息应当满足数据保存的有效期,可供机构审查和复制.

### 1.2 审计追溯的说明

- 系统本身及其所管控的生产资料的配置,使用,及历史情况:
- 系统及配置类信息的追溯,含以下类型:
  - 系统配置:系统配置参数的增,删,改
  - 访问控制:用户,用户组及访问权限的增,删,改;登入登出等信息
  - 系统基础数据的配置:危险防范,取样规则,标签,电子签名等信息的增,删,改
- 业务数据的管理追溯,含以下类型:
  - 基础数据的维护:物料,处方,工单,批次,库存,设备,衡器等信息的增,删,改及其他管理操作
  - MI 的增,删,改及其他管理操作
  - 生产的过程历史信息:如物料接收,称量,配料,MI 执行等
- 数据管理的记录,含以下类型:
  - 数据的导入导出,备份,归档等

### 1.3 审计追溯的目的

- 法规要求: 符合 21CFR part 11 的规则要求
- 可追溯性: 确保产品与制造的可追溯性
- 安全性: 确保数据及签名的真实性,完整性
- 质量要求: 为偏差的追根溯源提供保证,如数据的修改历史,用户的操作历史等

### 1.4 审计追溯的常规组成内容

- 操作员: 进行操作的人员信息
- 操作类型: 如修改,删除,验证等
- 操作时间: 操作的时间
- 操作地点: 工作中心,工站
- 操作内容: 对什么数据进行了什么操作, 新值旧值等
- 电子签名: 签名人,签名时间,签名说明
- 业务常用索引对象:
  - 库位号
  - 托盘号
  - 容器号
  - 物料号
  - 批次号
  - 工单号+版本
  - 处方号+版本
  - 设备号
  - MI 号+版本
  - 数量
  - 单位

### 1.5 审计追溯体系(3 部分)

- 常规审计追溯信息(AuditTrace)
- MI 执行的历史记录(MI Review)
- 物料的谱系信息(Genealogy)
- 导航关系如下图:
  - ![pic](/pics/审计追溯_关联.png)
  - 说明:
    - 从 MI Review 按工单/批次导航到 Genealogy
    - 从 MI Review 按工单/批次导航到 AuditTrace
    - 从 Genealogy 按工单/批次号导航到 MI Review
    - 从 Genealogy 按工单/批次号导航到 AuditTrace
    - 从 AuditTrace 按工单/批次号导航到 Genealogy

### 1.6 审计追溯的使用方式

- 查询
- 打印

---

## 2. 常规审计追溯信息(AuditTrace)

<!-- - <常规字段>如下:
  - 操作时间
  - 操作
  - 用户
  - 工作站
  - 工作中心
  - 签名时间
  - 签名 1
  - 备注 1
  - 签名 2
  - 备注 2
  - 操作内容 -->

### 2.1 工厂信息-企业管理

#### 2.1.1 介绍

- 企业管理中企业的维护:添加,编辑,删除的操作记录

#### 2.1.2 界面说明

- 主界面![图片](/pics/审计追溯_企业管理.png)

#### 2.1.3 字段说明

- 编码:企业编码
- 名称:企业名称
- 其他字段见 schema 设计

#### 2.1.4 操作说明

##### 2.1.4.1 排序筛选

- 主界面-概览(界面左部)
  - 排序
    - 默认排序:按照修改时间降序
    - 字段范围:编码,名称,最近修改
  - 筛选
    - 默认筛选:全选状态
    - 字段范围:编码,名称,最近修改
    - 支持全选,全不选
- 主界面-明细(界面右部)
  - 排序
    - 默认排序:按操作时间降序
    - 字段范围:操作时间,用户名
  - 筛选
    - 默认筛选:无
    - 字段范围:操作时间,用户名,签名时间
    - 支持全选,全不选

##### 2.1.4.1 查询

- S1:左侧输入编码值,进行模糊查询,编码值为空时,查询全部.
  - 编码的初始值为空.
  - 显示的字段见 UI
- S2:勾选 S1 的查询结果列表,自动进行更细致的追溯信息查询
  - 以选中行的`编码`为查询条件
  - 可多选
  - 没有勾选时,不显示明细追溯信息
  - 显示的字段见 UI
  - 默认查询结果为全选状态；

##### 2.1.4.1 打印

- 打印内容: 所查询筛选后的所有(全部分页)的`明细追溯信息`
- 打印所有需要打印的内容

### 2.2 工厂信息-厂区管理

#### 2.2.1 介绍

- 工厂管理中工厂的维护:添加,编辑,删除的操作记录

#### 2.2.2 界面说明

- 同企业管理

#### 2.2.3 字段说明

- 编码:工厂编码
- 名称:工厂名称
- 其他字段见 schema 设计

#### 2.2.4 操作说明

- 编码使用厂区编码,其他与企业管理相同.

### 2.3 工厂信息-区域管理

#### 2.3.1 介绍

- 区域管理中区域的维护:添加,编辑,删除的操作记录

#### 2.3.2 界面说明

- 同企业管理

#### 2.3.3 字段说明

- 编码:区域编码
- 名称:区域名称
- 其他字段见 schema 设计

#### 2.3.4 操作说明

- 编码使用区域编码,其他与企业管理相同.

### 2.4 工厂信息-区段管理

#### 2.4.1 介绍

- 区段管理中区段的维护:添加,编辑,删除的操作记录

#### 2.4.2 界面说明

- 同企业管理

#### 2.4.3 字段说明

- 编码:区段编码
- 名称:区段名称
- 其他字段见 schema 设计

#### 2.4.4 操作说明

- 编码使用区段编码,其他与企业管理相同.

### 2.5 工厂信息-位置管理

#### 2.5.1 介绍

- 位置管理中位置的维护:添加,编辑,删除的操作记录

#### 2.5.2 界面说明

- 同企业管理

#### 2.5.3 字段说明

- 编码:位置编码
- 名称:位置名称
- 其他字段见 schema 设计

#### 2.5.4 操作说明

- 编码使用位置编码,其他与企业管理相同.

---

### 2.6 物料-物料管理

#### 2.6.1 介绍

- 物料管理中物料的维护:添加,编辑,删除的操作记录
- 物料谱系 Genealogy, 请导航到 Genealogy

#### 2.6.2 界面说明

- 同企业管理

#### 2.6.3 字段说明

- 编码:物料编码
- 名称:物料名称
- 其他字段见 schema 设计

#### 2.6.4 操作说明

- 编码使用物料编码,其他与企业管理相同.

### 2.7 物料-危险防范

#### 2.7.1 介绍

- 物料管理中危险防范的维护:添加,编辑,删除的操作记录

#### 2.7.2 界面说明

- 主界面![图片](/pics/审计追溯_物料_危险防范.png)

#### 2.7.3 字段说明

- 编码:危险防范编码
- 描述:危险防范描述
- 其他字段见 schema 设计

#### 2.7.4 操作说明

- 编码使用危险防范编码,其他与企业管理相同.

### 2.8 物料-取样规则

#### 2.8.1 介绍

- 物料管理中取样规则的维护:添加,编辑,删除的操作记录

#### 2.8.2 界面说明

- 主界面![图片](/pics/审计追溯_物料_取样规则.png)

#### 2.8.3 字段说明

- 编码:取样规则编码
- 描述:取样规则描述
- 其他字段见 schema 设计

#### 2.8.4 操作说明

- 编码使用取样规则编码,其他与企业管理相同.

---

### 2.9 处方管理

#### 2.9.1 介绍

- 处方管理中处方的维护:添加,编辑,删除的操作记录

#### 2.9.2 界面说明

- 主界面![图片](/pics/审计追溯_处方管理.png)

#### 2.9.3 字段说明

- 编码:处方编码
- 名称:处方名称
- 版本:处方版本
- 其他字段见 schema 设计

#### 2.9.4 操作说明

- 编码使用处方编码,其他与企业管理相同.
  - 明细查询使用编码+版本

---

### 2.10 标签-标签管理

#### 2.10.1 介绍

- 标签管理中标签模板的维护:添加,编辑,删除的操作记录

#### 2.10.2 界面说明

- 主界面![图片](/pics/审计追溯_标签_标签管理.png)

#### 2.10.3 字段说明

- 编码:标签编码
- 名称:标签名称
- 组别:标签所属分组类型
- 其他字段见 schema 设计

#### 2.10.4 操作说明

- 编码使用标签编码,其他与企业管理相同.

### 2.11 标签-标签打印

#### 2.11.1 介绍

- 业务功能中使用标签模板打印标签的操作记录

#### 2.11.2 界面说明

- 主界面![图片](/pics/审计追溯_标签_标签打印.png)

#### 2.11.3 字段说明

- 编码:标签编码
- 名称:标签名称
- 组别:标签所属分组类型
- 序列号: 关键的标签内容序号(一般是序列号,但不全是)
- 其他字段见 schema 设计

#### 2.11.4 操作说明

- 编码使用标签编码,其他与企业管理相同.

---

### 2.12 批次-物流批次

#### 2.12.1 介绍

- 物料流转批次相关的信息:批次管理,接收,托盘,容器,调整,包装等信息;字段如下:

#### 2.12.2 界面说明

- 主界面![图片](/pics/审计追溯_批次_物流批次.png)

#### 2.12.3 字段说明

- 批次号:物料批次号
- 生产批次:产出批次
- 单号:接收单号
- 托盘数量
- 托盘编号
- 初始库位
- 当前库位
- 容器编码: ID
- 容器序号: 顺序号
- 事务类型: 含批次接收事务
- 标签
- 包装方式
- 二次包装方式
- 其他字段见 schema 设计

#### 2.12.4 操作说明

- 查询条件使用批次号,其他与企业管理相同.

### 2.13 批次-生产批次

#### 2.13.1 介绍

- 该批次相关的生产操作记录.包括产出声明相关的批次操作记录

#### 2.13.2 界面说明

- 主界面![图片](/pics/审计追溯_批次_生产批次.png)

#### 2.13.3 字段说明

- 批次号:接收批次
- 生产批次:产出批次
- 物料编码: 接收的物料和产出的产品编码
- 工单号:批次对应的工单号
- 工单版本:批次对应的工单号的版本
- 数量:批次数量
- 设备类别
- 设备编码
- 容器编号
- 其他字段见 schema 设计

#### 2.13.4 操作说明

- 查询条件使用批次号,其他与企业管理相同.

---

### 2.14 工单

#### 2.14.1 介绍

- 工单维护,以及工单执行过程中相关的批次,混料,设备等信息

#### 2.14.2 界面说明

- 主界面![图片](/pics/审计追溯_工单管理.png)

#### 2.14.3 字段说明

- 批次号:接收批次
- 生产批次:产出批次
- 物料编码: 工单要生产的产品编码
- 工单号:批次对应的工单号
- 工单版本:批次对应的工单号的版本
- 数量:批次数量
- 设备类别
- 设备编码
- 容器编号
- 其他字段见 schema 设计

#### 2.14.4 操作说明

- 查询条件使用工单号,其他与企业管理相同.
  - 明细查询使用工单号+版本

---

### 2.15 衡器-衡器管理

#### 2.15.1 介绍

- 衡器的维护:添加,编辑,删除的操作记录

#### 2.15.2 界面说明

- 同企业管理

#### 2.15.3 字段说明

- 其他字段见 schema 设计

#### 2.15.4 操作说明

- 编码使用衡器编码,其他与企业管理相同.

### 2.16 衡器-衡器使用

#### 2.16.1 介绍

- 衡器使用: 除了衡器管理之外的信息,主要包含校准记录+称量记录:

#### 2.16.2 界面说明

- 主界面![图片](/pics/审计追溯_衡器使用.png)

#### 2.16.3 字段说明

- 编码:衡器编码
- 名称:衡器名称
- 其他字段见 schema 设计

#### 2.16.4 操作说明

- 主界面-概览(界面左部)
  - 排序
    - 默认排序:按照修改时间降序
    - 字段范围:编码,名称,最近修改
  - 筛选
    - 默认筛选:全选状态
    - 字段范围:编码,名称,最近修改
    - 支持全选,全不选，反选，下拉框（存在删除）
- 主界面-明细(界面右部)
  - tab页切换查看校准历史及称量历史
  - 校准历史
      - 排序
        - 默认排序:按操作时间降序
        - 字段范围:操作时间,校准时间，操作人
      - 筛选
        - 默认筛选:无
        - 字段范围:操作时间,校准时间，操作人
        - 支持全选,全不选，反选
  - 称量历史
      - 排序
        - 默认排序:按操作时间降序
        - 字段范围:操作时间,工单号，批次号，操作员，物料编码
      - 筛选
        - 默认筛选:无
        - 字段范围:操作时间,工单号，批次号，操作员，物料编码
        - 支持全选,全不选，反选


### 2.17 设备-设备管理

#### 2.17.1 介绍

- 设备属性,类别,配置,接口等的维护记录

#### 2.17.2 界面说明

- 主界面![图片](/pics/审计追溯_设备_设备管理.png)

#### 2.17.3 字段说明

- 设备类别
- 设备属性
- 设备接口
- 接口参数
- 其他字段见 schema 设计

#### 2.17.4 操作说明

- 编码使用设备编码,其他与企业管理相同.

### 2.18 设备-设备使用

#### 2.18.1 介绍

- 设备管理之外的设备使用记录信息,包括设备相关的混料信息

#### 2.18.2 界面说明

- 主界面![图片](/pics/审计追溯_设备_设备使用.png)

#### 2.18.3 字段说明

- 设备类别
- 轮转计数: RotationCount
- 设备状态: StatusAfter
- 批次号
- 生产批次号
- 物料编码
- 容器编号
- 工单号
- 工单版本
- 其他字段见 schema 设计

#### 2.18.4 操作说明

- 编码使用设备编码,其他与企业管理相同.

---

### 2.19 MI 管理

#### 2.19.1 介绍

- MI 管理的操作记录信息
- MI 执行产生的批记录请导航到 MI Review

#### 2.19.2 界面说明

- 主界面![图片](/pics/审计追溯_MI管理.png)

#### 2.19.3 字段说明

    - MI 编码
    - MI 版本
    - MI 状态

- 其他字段见 schema 设计

#### 2.19.4 操作说明

- 编码使用 MI 编码,其他与企业管理相同.
  - 明细查询使用 MI 编码+版本

---

### 2.20 库存-库位管理

#### 2.20.1 介绍

- 库位的维护操作记录

#### 2.20.2 界面说明

- 同企业管理

#### 2.20.3 字段说明

- 编码:库位编码
- 名称:库位名称
- 其他字段见 schema 设计

#### 2.20.4 操作说明

- 编码使用库位编码,其他与企业管理相同.

### 2.21 库存-库位使用

#### 2.21.1 介绍

- 库位使用相关的接收,托盘,容器,调整,设备等操作记录信息

#### 2.21.2 界面说明

- 主界面![图片](/pics/审计追溯_库存_库位使用.png)

#### 2.21.3 字段说明

- 批次号
- 生产批次
- 单号
- 供应商编码
- 托盘数量
- 托盘编号
- 初始库位
- 当前库位
- 容器编码: ID
- 容器序号: 顺序号
- 事务类型: 含批次接收事务
- 设备类别
- 设备编码
- 其他字段见 schema 设计

#### 2.21.4 操作说明

- 编码使用库位编码,其他与企业管理相同.

### 2.22 库存-库区管理

#### 2.22.1 介绍

- 库区的维护操作记录

#### 2.22.2 界面说明

- 同企业管理

#### 2.22.3 字段说明

- 编码:库区编码
- 名称:库区名称
- 其他字段见 schema 设计

#### 2.22.4 操作说明

- 编码使用库区编码,其他与企业管理相同.

### 2.23 库存-库区管理

#### 2.23.1 介绍

- 库区使用相关的接收,托盘,容器,调整,设备等操作记录信息

#### 2.23.2 界面说明

- 同库位使用

#### 2.23.3 字段说明

- 同库位使用

#### 2.23.4 操作说明

- 编码使用库区编码,其他与企业管理相同.

---

### 2.24 系统-系统管理

#### 2.24.1 介绍

- 系统参数配置等操作记录信息

#### 2.24.2 界面说明

- 主界面![图片](/pics/审计追溯_系统_系统管理.png)

#### 2.24.3 字段说明

- 参数编码:所维护的参数编码

#### 2.24.4 操作说明

- 编码使用参数编码,其他与企业管理相同.

### 2.25 系统-访问控制

#### 2.24.1 介绍

- 访问控制相关的追溯信息,用户,用户组,权限,签名的维护,系统的登录访问信息等

#### 2.24.2 界面说明

- 主界面![图片](/pics/审计追溯_系统_访问控制.png)

#### 2.24.3 字段说明

- 无
- ~~调整的用户组~~
- ~~调整的签名~~
- ~~调整的功能~~

#### 2.24.4 操作说明

- 查询条件使用用户名,其他与企业管理相同.

### 2.25 系统-使用记录

#### 2.25.1 介绍

- 记录并统计各用户的管理操作记录和所产生的信息记录（包括但不限每个模块的增、删、改等及其相关信息）

#### 2.25.2 界面说明

- 主界面![图片](/pics/审计追溯_系统_使用记录.png)

#### 2.25.3 字段说明

- 同访问控制

#### 2.25.4 操作说明

- 同访问控制

---

### 2.3 操作与签名

- 一条需要签名的操作记录,对应一条签名记录: 签名记录里 info 为 JSON 对象,可以含多人签名的信息

### 2.4 查询谱系

- 所选单元格为批次号/工单号时,可以跳转到谱系查询,并以所选批次号/工单号为默认条件
- 即有工单号和批次号的追溯模块才可以进行跳转查询

---

## 3. 物料的谱系信息(Genealogy)

### 3.1 谱系的目的

- 耗料与产品的工单或批次关联关系:`耗用关系`,`产出关系`

### 3.2 批次-工单连接关系

#### 3.2.1 示例如下

- ![pic](/pics/审计追溯_工单批次关系.png)

#### 3.2.2 说明:

- 批次耗用:
  - 批次可以被一个或多个工单使用(批次 A,B)
  - 一个工单可以使用一个或多个批次(工单 1, 工单 2)
- 批次产出:
  - 一个批次只能由一个唯一的工单产出(批次 D, E, F)
  - 一个工单可以产出一个或多个批次(工单 1, 工单 2)
- 一个工单所耗用的批次与产出的批次可以相同

#### 3.2.3 关系产生时间点

- 耗用关系: 物料称量后,将所耗用的物料信息添加到谱系表:批次号,物料编码,工单号,工单版本,链接类型
- 产出关系: 生产声明后, 将所产出的产品信息添加到谱系表:工单号,工单版本,生产批次号,产品编码,链接类型

### 3.3 显示方式:

- 图示,示例如下
  - ![pic](/pics/审计追溯_谱系图示.png)
- 主界面如下:
  - ![pic](/pics/审计追溯_谱系主界面.png)
- 搜索历史界面及相关说明如下:
  - ![pic](/pics/审计追溯_谱系搜索历史.png)
- 图示排列界面及相关说明如下:
  - ![pic](/pics/审计追溯_谱系图示排列.png)
- 表格,示例如下
  - ![pic](/pics/审计追溯_谱系表格展示.png)
- 图示的元件
  - 元件类型及内容
    1. 工单:工单号,工单状态,料号
    2. 批次:批次号,批次状态,料号
    3. 接收:单号,接收状态,批次号,料号
    4. 发货:发货单号,发货状态,批次号,料号
  - 元件属性,示例如下:
    - ![pic](/pics/审计追溯_谱系元件属性.png)
    - 说明:
    1. 工单:工单号,工单状态,工单类型,料号,物料名称,制造批次号
    2. 批次:批次号,批次状态,料号,物料名称,接收时间,过期时间,限制使用日期,质检状态,质检编号,质检日期,决定日期,专论,引用,供应商编码,供应商批次
    3. 接收:单号,接收状态,次序号,厂区,区域,处理类型,接收日期,批次号,料号,物料名称,供应商编码,供应商名称,供应商批次,接收数量
    4. 发货:发货单号,发货状态,批次号,料号,创建日期,发货地址,运输方式,国家,计划时间,发货时间,客户编码,客户名称,客户地址,客户国家,电话,传真,邮件地址,承运商
  - 元件及状态颜色信息如下:
    - ![pic](/pics/审计追溯_谱系图示及状态颜色.png)
    - 说明:
    1.  系统支持这 4 种形状,目前版本不支持形状的配置
    2.  状态颜色系统内置,目前版本不支持配置

### 3.4 操作及设定:

#### 3.4.1 UI 示例如下:

- ![pic](/pics/审计追溯_谱系搜索.png)

#### 3.4.2 操作说明:

- 搜索:开始一个新的查询
- 停止:停止正在执行的查询
- 清除:清除已展示的所有结果
- 打印:将查询结果打印
- 排列:按照设定的规则排列图示,包含`向下`,`向上`,`向左`,`向右`
- 缩放:缩放百分比设定,包含`25`,`50`,`75`,`100`,`125`,`150`

#### 3.4.3 按工单查询:

- 查询条件: 使用工单号
- 查询步骤
  - S1:定位当前工单:【查询工单表 by WO group by creation date】 UNION 【查询谱系表 by WO】
  - S2:耗用信息
    - S2.1 在谱系表查询 WO 所耗用的物料批次信息（一个或多个批次）
    - S2.2 使用 S2 得到的批次号与料号,查询批次信息,及批次物料信息
  - S3:产出信息
    - S3.1 在谱系表查询 WO 所产出的产品批次信息（一个或多个）
    - S3.2 使用 S4 得到的批次号与料号,查询批次信息,及批次物料信息
  - S4: 所耗用批次的其他用途: 用 S2 得到的批次,查询这些批次`相应工单`的产出信息; 一个工单产出的批次,可供多个工单耗用
  - S5: 接收信息:使用 S2 中得到的批次号与物料号,查询批次的接收信息.

#### 3.4.4 按批次查询:

- 查询条件
  1. 使用批次号
  2. 先制定料号,在指定批次号:
- 查询步骤
  - S1:定位当前批次:
    - S1.1 【查询批次表 by 批次号 + 料号 group by creation date】 UNION 【查询谱系表 by 批次号 + 料号 OR 产出批次号 + 料号】
    - S1.2 使用 S1.1 得到的批次号,查询批次信息,及批次物料信息
  - S2: 产出信息: 所耗用批次的其他用途: 用 S1.1 得到的批次号,查询这些批次`相应工单`的产出信息; 一个工单产出的批次,可供多个工单耗用
  - S3: 接收信息:使用 S1.1 中得到的批次号与物料号,查询批次的接收信息.
  - S4: 耗用信息
    - S4.1 在谱系表查询该批次被哪些 WO 所耗用（一个多个）
    - S4.2 在工单表查询这些 WO 的常规信息
    - S4.3 在谱系表查询这些 WO 的产出批次信息
    - S4.4 使用 S4.3 得到的批次号与料号,查询批次信息,及批次物料信息
    - S4.5 使用 S4.3 得到的批次号,查询批次信息被哪些工单耗用

#### 3.4.5 查询级别

- 范围: 1,2,ALL(默认)

#### 3.4.6 查询方向:

- 向前,向后,前后(默认)
- 说明:
  - 按批次查询-向前:
    - Level1: 链接使用了该批次的工单
    - Level2: 在 L1 中的工单,产生了哪些批次
    - ALL: 按照 L1 和 L2 的链接方法,链接所有相关工单和批次
  - 按批次查询-向后:
    - Level1: 链接产生该批次的工单,或初始的接收信息
    - Level2: 在 L1 中的批次,链接使用了该批次的工单
    - ALL: 按照 L1 和 L2 的链接方法,链接所有相关工单和批次,及接收信息
  - 按工单查询-向前:
    - Level1: 链接该工单产生了哪些批次
    - Level2: 在 L1 中的批次,链接使用了该批次的工单
    - ALL: 按照 L1 和 L2 的链接方法,链接所有相关工单和批次
  - 按工单查询-向后:
    - Level1: 链接该工单使用了哪些批次
    - Level2: 在 L1 中的批次,产生该批次的工单,或初始的接收信息
    - ALL: 按照 L1 和 L2 的链接方法,链接所有相关工单和批次,及接收信息

---

### 4. 导航操作:

- 扩展查询,示例如下
  - ![pic](/pics/审计追溯_谱系扩展搜索.png)
- 显示追溯 ,示例如下
  - ![pic](/pics/审计追溯_谱系显示追溯.png)
- 称量明细,示例如下
  - ![pic](/pics/审计追溯_谱系称量明细.png)
  - 说明:所选元素为工单时,该功能才可用
- 查看批记录,示例如下
  - ![pic](/pics/审计追溯_谱系查看批记录.png)
- 表格界面中的导航与图示中的导航是相同的.

---

### 5. 数据范围

- 未归档数据 + 已归档数据

---

### 6. 时间范围

- 提供起始时间的设定及显示功能, 以该时间为起始时间,以查询的当前时间为结束时间,即时间范围区间为[设定的起始时间,提交查询的时间]

---

### 7. 图形颜色设定

- 暂不实现该配置
- 使用 EBRConfig 表,propertyKey 为 GENEALOGY_SETTINGS, data 为 JSON 格式的配置项:

```JSON
{
  "GENEALOGY_SETTINGS": {
    "Receipt_Shape": " 5",
    "Receipt_Untreated_Color": " 16777215",
    "Receipt_Received_Color": " 16711680",
    "Receipt_Labelled_Color": " 1145609",
    "Receipt_StockingInProgress_Color": " 16711935",
    "Receipt_Stocked_Color": " 6919673",
    "Receipt_Canceled_Color": " 255",

    "Lot_Shape": " 50",
    "Lot_Refused_Color": " 255",
    "Lot_Approved_Color": " 16711680",
    "Lot_CondApproved_Color": " 16776960",
    "Lot_Test_Color": " 6919673",
    "Lot_Quarantine_Color": " 65280",
    "Lot_Hold_Color": " 65535",

    "WO_Shape": " 0",
    "WO_Created_Color": " 16777215",
    "WO_Untreated_Color": " 12632256",
    "WO_Launched_Color": " 12648447",
    "WO_PreparationInProgress_Color": " 65280",
    "WO_PreparationTerminated_Color": " 4836354",
    "WO_Reconcilied_Color": " 1145609",
    "WO_ReadyForManufacturing_Color": " 16157901",
    "WO_ManufacturingInProgress_Color": " 16711935",
    "WO_ManufacturingTerminated_Color": " 16711680",
    "WO_Closured_Color": " 6579300",
    "WO_Canceled_Color": " 255",

    "Packing_Shape": " 4",
    "Packing_Created_Color": " 16777215",
    "Packing_Ready_To_Complete_Color": " 4836354",
    "Packing_Completed_Color": " 16711680",
    "Packing_Ready_To_Pack_Color": " 16711935",
    "Packing_Packed_Color": " 6919673",
    "Packing_Ready_To_Load_Color": " 16157901",
    "Packing_Loaded_Color": " 12648447",
    "Packing_Cancel_Color": " 255",
    "Packing_Closed_Color": " 6579300"
  }
}

```
