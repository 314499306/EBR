# 物料管理

---

## 1. 基础数据

### 1.1 取样规则:

- 操作限制:
  - 编辑限制: 出厂内置的规则是只读的,用户自定义的规则允许编辑
  - 删除限制: 被物料使用的取样规则不允许被删除
- 取样规则明细:
  1. [从,到] 为闭区间
  2. 第一条规则:从 1 开始,即 [1,M]
  3. 最后一条规则: 以无限大为右区间,即[N,]
  4. 每个区间的取样数量不能大于左右区间差值的绝对值.
  5. 每个区间的左区间默认值为该区间的起始值
- 初始取样规则及说明:
  - ![图片](/pics/物料_取样规则.png)
  - ![图片](/pics/物料_取样规则图示.png)

---

### 1.2 危险防范信息:

- 详见 schema 设计
- 删除限制: 被物料使用的条目不允许被删除,异常信息"该危险防范信息已被物料使用"

---

## 2. 物料管理

### 2.1 界面说明

- 物料管理主界面
  - 取消`库存量`的显示

### 2.2 字段说明

- 库存量:数据库不在保存该值,从库存表统计汇总
  - 此处物料库存,只统计实际情况,不考虑使用场景,即不考虑物料的批次状态
- 物料信息维护中取样量单位与库存单位一致；且取样量单位不可改变
- 其他详见 schema 设计

### 2.3 业务说明

- 物料状态:
  - ![物料_物料状态](/pics/物料_物料状态.png)
- 默认状态: 可编辑
- 只有可编辑状态的物料才允许编辑,并且提交编辑结果时,验证该物料状态是否是可编辑状态
- 已验证的物料不允许删除
- 物料列表默认显示可编辑和已验证的物料.
- 会修改物料状态的操作,需提示用户进行确认.

- 有效期:
  - ![物料_LDU](/pics/物料_LDU.png)
- 效价 1 单位:
  - ![物料_效价1单位](/pics/物料_效价1单位.png)
- 物料有效期,质检有效期,物料使用提前期,LDU 警报:这 4 个有效期统一使用`物料有效期`的单位

- 存储库区与库位
  - 托盘:
    - 默认托盘库区:含以下库位的库区:非不限容量且库位存储类型为`托盘`存储的库位
    - 备用托盘库区:含以下库位的库区:不限容量且存储类型匹配的库位
    - 默认库区与备用库区`不能相同`
  - 容器
    - 默认容器库区:含以下库位的库区:非不限容量且库位存储类型为`容器`存储的库位
    - 备用容器库区:含以下库位的库区:不限容量且存储类型匹配的库位
    - 默认库区与备用库区`不能相同`
  - 取样
    - 取样库区:含以下库位的库区:不限容量的库位
    - 取样初始库位:所有不限容量的库位,不只是上面设定的区域库区中满足要求的库位.
- 标签:标签类型,移出`取样信息组`,移到`标签分组`.
- 批记录标签:批记录标签数,移出`取样信息组`,移到`标签分组`.

### 2.4 操作说明

- 新增:添加新物料
- 编辑:编辑现有物料
- 复制:使用现有物料,快速创建新物料,物料状态为"可编辑"
- 刷新:刷新数据
- 验证:验证物料,修改物料状态到"已验证"
- 取消验证: 取消验证物料,修改物料状态到"可编辑"
- 生效:激活物料,修改物料状态到"可编辑"
- 失效:删除物料,修改物料状态到"失效的"
- 明细查看:查看指定物料的明细信息;查看方式如下:
  - 双击指定物料,以只读方式显示物料明细
  - 指定物料,点击【查看】按钮,以只读方式显示物料明细

### 2.5 操作限制:

- 已删除检查(暂不实现):除新增,激活之外的操作:需要在操作前,验证数据项是否已被删除,如果已被删除,需提示用户:"该物料刚被删除了".
- 同时编辑检查(暂不实现)除新增和查询之外的操作:需要在操作前, 验证该物料正在被别的用户编辑,如果是,则不允许更新和删除类型的操作,并提示用户:"该物料正在被另一个用户编辑,请稍后再试".
- 在新增操作时: 需要验证物料编码的唯一性,若已存在,需提示用户"该物料已存在,请重新输入"
- 库存单位的编辑限制:满足以下条件时,才允许编辑库存单位:
  1.  该物料没有任何处方使用
  2.  该物料没有被任何工单使用
  3.  该物料没有库存
  4.  该物料没有被"已取消"状态之外的物料接收单使用.
- 称量模式的限制:
  1.  如果物料时可称量的:必须为物料指定唯一的默认称量模式,若不满足,需提示用户"请指定默认称量模式"
- 取消验证的限制:
  1.  该物料没有任何处方使用,对应的异常信息"物料已被处方使用"
  2.  该物料没有被任何工单使用,对应的异常信息"物料已被工单使用"
  3.  该物料没有库存,对应的异常信息"物料尚有库存"
  4.  该物料没有被"已取消"状态之外的物料接收单使用,对应的异常信息"物料已被接收单使用"
- 删除的限制:
  1.  该物料没有任何处方使用,对应的异常信息"物料已被处方使用"
  2.  该物料没有被任何工单使用,对应的异常信息"物料已被工单使用"
  3.  该物料没有库存,对应的异常信息"物料尚有库存"
  4.  该物料没有被"已取消"状态之外的物料接收单使用,对应的异常信息"物料已被接收单使用"

## 3. 物料管理中的取样规则与批次状态

### 3.1 介绍

- 批次状态:用来管理接收批次与生产声明批次的初始批次状态
  - 只使用批次状态:检验,批准,带条件批准,测试,驳回
    - 批次失效使用批次接收的功能入口
- 取样规则:在批次状态为`检验`时,取样规则生效
- 批次接收和生产声明中与添加批次,托盘,容器相关的操作,使用物料上的批次状态和取样规则来设置批次取样状态,托盘状态,容器取样状态.

## 4. 失效物料说明

- 失效的物料的生命周期尚未结束
  - 失效的物料可以重新激活
  - 物料的使用过程中,涉及验证资源是否被物料使用时,应包含失效物料
  - 失效的物料在清理数据(暂不实现)时进行删除,此时物料声明周期结束