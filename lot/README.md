# 批次管理

## 1. 简介

### 1.1 批次来源:

- 通过外围系统导入到 EBR 系统
- EBR 系统本地创建:
  - 原料和包材的接收
  - 成品和半成品的生产声明

### 1.2 批次管理

- 在批次被使用之前,可以维护数量,注释,状态,效价,密度和有效期等信息

### 1.3 相关业务

- 批次管理与物料接收, 生产声明, 库存管理相关,
- 系统提供批次的追溯信息
- 批次接收一般会自动创建一个新的接收批次

### 1.4 简要流程

- 流程 ![pic](/pics/批次_批次管理_流程简介.png)

### 1.5 WO 与 LOT 的关系

- 工单与批次的关系:核心为耗料与产品的关系,只是通过工单这种管理方式来组织生产.
- WO: LOT 1:1 ; 一个工单只产出一个唯一批次的产品
  - 举例: WO1 耗料批次为 LOTA, LOTB, 通过 WO1, 产出批次为 LOT C; 批次追溯为正常追溯.
  - 创建工单是自动生成唯一批次号,与工单号关联.
- WO: LOT 1:N ; 一个工单可以产出多个批次的产品; 生产管理层面应该保证,同一批次的产出应该耗用同一组批次的物料; 即同一组批次原料可以用来生产多个批次的产品
  - 举例: WO1 耗料批次为 LOTA, LOTB, 通过 WO1, 产出批次为 LOT C 和 LOTD; 批次追溯为正常追溯.此为产出多个批次主产品/多个批次主副产品的情况,副产品产出通过 MI 的生产声明元件实现.
  - 本版本在创建工单时,暂不支持批次号的手动录入和列表选择,不支持开立工单时多个批次挂到同一工单

---

## 2. 批次管理:

### 2.1 界面说明

- UI 示例如下  
  批次列表:![pic](/pics/批次_批次列表.png)
  批次维护:![pic](/pics/批次_批次维护.png)
  锁定容器:![pic](/pics/批次_锁定容器.png)
  驳回容器:![pic](/pics/批次_驳回容器.png)
- 批次的状态需要颜色管理:严重程度红色>黄色>棕色,重叠时使用更严重的颜色
  - 红色-驳回
  - 棕色-超过限制使用期
  - 黄色-过期

### 2.2 字段说明(批次列表+批次维护)

- 物料信息
  - 物料编码: 来自批次表;不可编辑
  - 物料名称: 通过料号 load,来自物料表;不可编辑
  - 物料类型: 通过料号 load,来自物料表;不可编辑
  - 物料效价: 通过料号 load,来自物料表;不可编辑
  - 物料密度: 通过料号 load,来自物料表;不可编辑
- 批次信息
  - 批次密度: 来自批次表,在批次被使用之前可编辑
  - 批次效价: 来自批次表,在批次被使用之前可编辑
  - 供应商编码: 来自批次表;不可编辑
  - 供应商名称: 通过供应商 load,来自供应商表;不可编辑
  - 其他字段: 见 schema 说明
- 库存信息
  - 厂区: 通过批次号与物料号 load,来自位置表
  - 区域: 通过批次号与物料号 load,来自位置表
  - 区段: 通过批次号与物料号 load,来自位置表
  - 库区: 通过批次号与物料号 load,来自库位表
  - 库位: 通过批次号与物料号 load,来自库位表
  - 总量: 通过批次号与物料号 load,来自库位表
  - 单位: 通过料号 load,来自物料表;不可编辑

### 2.3 操作说明

#### 2.3.1 排序筛选

- 主界面-批次列表
  - 排序
    - 默认排序: 按照有效期升序
    - 字段范围: 物料编码,物料名称,批次号,批次状态,取样状态,有效期,厂区编码,区域编码,区段编码,库区编码,库位编码
  - 筛选
    - 默认筛选: 无
    - 字段范围: 物料编码,物料名称,物料类型,批次号,批次状态,取样状态,厂区编码,区域编码,区段编码,库区编码,库位编码
    - 支持扫码查询

#### 2.3.2 批次维护

- 取样: 需要取样的批次,在执行该操作后,批次的取样状态: 等待取样-> 已取样
- 重取样: 对需要取样且已经取样的批次,在执行该操作后,批次的取样状态: 已取样->待重取样
- 批次状态修改: 批次状态的修改会更新该批次下所有容器的状态,~~任何时候都可以进行批次状态的修改~~
  - 只使用批次状态:检验,批准,带条件批准,测试,锁定
- 批次过期时间的修改: 批次状态的修改会更新该批次下所有容器的过期时间
- 等待取样的批次:批次状态维护时,只能进行备注修改和取样操作.
- 不需取样的批次:批次状态维护时,只能进行分析,分析日期,限制使用期,有效期,批次状态,药典,备注的维护
- 需要取样且已取样的批次:批次状态维护时,只能进行`密度`,`效价分析`,分析日期,限制使用期,有效期,批次状态,药典,备注的维护

#### 2.3.3 锁定容器

- 按照容器编码,进行处理
- 检验容器编码是否存在,是否属于所选批次
- 将容器的"状态"更新为"锁定"状态
- 将容器的"取样状态"更新为"已取样";
- 如果该容器所属托盘,已完成取样,则将托盘状态更新为"待入库"
- 如果一个批次下的容器全部为锁定状态,那么该批次的状态也会被更新为锁定状态:
- 已锁定的容器,不可以再次锁定

#### 2.3.4 锁定批次

- 按照批次+料号进行更新,见 UI; 另外批次锁定后
- 更新批次的"批次状态"到"锁定"状态;
- 更新批次的"取样状态"到"已取样"
- 更新非驳回批次的"取样时间"到"当前时间"
- 更新"决定日期"为操作日期
- 更新"备注"信息
- 将该批次的所有容器(已经驳回和锁定的容器除外)的"状态"更新为"锁定"状态
- 更新该批次的所有的容器的"取样状态": "等待取样"->"已取样";
- 更新该批次的所有的托盘的"状态": "等待取样"->"等待入库";
- 更新`批次接收的`"是否已取样"到"True"

#### 2.3.5 驳回容器

- 按照容器编码,进行更新,见 UI
- 检验容器编码是否存在,是否属于所选批次
- 将容器的"状态"更新为"驳回"状态
- 更新该的容器的"取样状态"为"已取样";
- 单个容器的驳回,系统不会自动更新相关批次的剩余总数
- 标记该容器被单独驳回
- 如果该容器所属托盘,已完成取样,则将托盘状态更新为"待入库"
- 已驳回的容器,不可以再次驳回
- 如果一个批次下的容器全部被驳回,该批次的状态`不会`被更新为驳回状态:

#### 2.3.6 驳回批次

- 更新批次的"批次状态"到"驳回"状态;
- 更新批次的"取样状态"到"已取样"
- 更新"决定日期"为操作日期
- 更新非驳回批次的"取样时间"到"当前时间"
- 将该批次的所有容器的"状态"更新为"驳回"状态
- 取消该批次下容器的 "单独驳回"标记
- 更新该批次的所有的容器的"取样状态": "等待取样"->"已取样";
- 更新该批次的所有的托盘的"状态": "等待取样"->"等待入库";
- 更新`批次接收的`"是否已取样"到"True"

#### 2.3.7 概览

- 查看物流的汇总库存情况
- 链接到[库存查询-库存查询-总览]

#### 2.3.8 明细

- 按库位查看托盘和容器的库存情况
- 链接到[库存查询-库存查询-明细]

#### 2.3.9 明细-实体明细

- 链接到[库存查询-库存查询-明细-实体明细]

#### 2.3.10 打印

- 链接到[库存查询-库存查询-打印]

### 2.3 状态控制说明

- 批次状态控制,见下图:  
  ![pic](/pics/批次_批次管理_状态控制.png)
- 批次状态的变换图如下:  
  ![pic](/pics/批次_批次管理_状态转换.png)
  - #表示如果相关批次已经被一个"生产声明"声明为"条件批准",那么该批次只能在批记录复核批准后才能被批准.
  - 批次与容器的状态管理规则:
    - "驳回"之外的批次状态的修改: 批次状态的修改的同时,会更新该批次下的容器状态,但不应影响该批次下已被`单独`"驳回"和"锁定"的容器
    - "驳回": 驳回批次,应驳回该批次下所有的容器,包含已`单独`被锁定"的容器; 整批驳回会清除容器的`单独驳回`标记.
    - 已被`单独`"驳回"和"锁定"的容器`状态`: 只有驳回批次和编辑容器状态的操作才能更新容器的状态信息

### 2.4 取样管理

- EBR 中的取样管理:
  - EBR 进行管理:  
    ![pic](/pics/批次_批次管理_取样管理.png)
- 批次状态控制,见下图:
  - EBR 不进行管理:  
    ![pic](/pics/批次_批次管理_取样管理_无管理.png)
- 常规取样点:  
  ![pic](/pics/批次_批次管理_取样管理_常规取样点.png)
- 批次的取样量与取样规则由物料主档定义:  
  ![pic](/pics/批次_批次管理_取样管理_取样设定.png)
- 批次与容器取样:  
  ![pic](/pics/批次_批次管理_批次与容器取样.png)

### 2.5 限制使用日期与有效期的说明

- ![pic](/pics/批次_批次管理_LDU与有效期.png)

### 2.6 批次状态与容器状态

- 容器状态:
  - 一般由所属批次的状态决定
  - 容器可以被单独锁定或驳回  
    ![pic](/pics/批次_批次管理_容器状态.png)
- 批次锁定与容器锁定的关系, 见下图  
  ![pic](/pics/批次_批次管理_批次与容器锁定.png)
- 批次驳回与容器驳回的关系, 见下图  
  ![pic](/pics/批次_批次管理_批次与容器驳回.png)
- 批次状态与容器状态的关系, 见下图  
  ![pic](/pics/批次_批次管理_批次与容器状态.png)

---

## 3. 批次接收

### 3.1 介绍

- 按批次接收物料.
- 批次接收的生命周期: ![pic](/pics/批次_接料生命周期.png)
- `单次接收的数量限制: 托盘包装:托盘数最大支持1000,容器包装:容器数最大支持1000,超出则提示用户调整.`

### 3.2 界面说明

#### 3.2.2 界面说明-主界面

- 批次接收主界面: ![pic](/pics/批次_批次接收.png)
- 主界面显示说明:默认显示未处理的批次

#### 3.2.2 界面说明-接收明细

- 托盘包装接收:![pic](/pics/批次_批次接收明细.png)
- 容器包装接收:![pic](/pics/批次_批次接收明细_容器.png)
- 初始界面使用托盘接包装收的界面,包装类型选择容器后,切换到容器包装接收界面

### 3.3 字段说明

#### 3.3.1 字段说明-(列表+明细)

- 接收信息:
  - 厂区:系统中已维护的厂区,默认值为当前工站的工作中心的位置所属厂区
  - 区域:系统中已维护的区域,默认值为当前工站的工作中心的位置所属区域
  - 区段:系统中已维护的区段
  - 库区:系统中已维护的库区
  - 类型: 接收的事务类型,字典类型`RECEIPT_TYPE`,该类型不同工厂有扩展的可能,因此使用字典表,不使用枚举;创建后不可编辑
  - 订单号: 接收的订单号; 由系统自动生成或由用户手动输入;可重复,一个单号可多批次接收;创建后不可编辑
    - 系统自动生成规则: RO+8 位数字序列
  - 序号: 接收的次序号,每个(`类型`+`订单号`)内递增序列,长度 5 为,使用 0 来左补齐,起始值为`00001`;系统自动生成,不可编辑
  - 是否本地接收: 是 EBR 系统本地接收,还是外部系统导入;系统自动生成,不可编辑
  - 接收状态: 枚举类型`ReceiptStatus`;初始状态为`未处理`; 核准后,状态改为`已接收`;系统自动转换;不可编辑
  - 计划接收日期: 默认为当前时间,创建后可编辑,但接收发生后不可编辑
  - 接收日期: 实际接收日期
  - 是否已取样: Y/N, 取样是否完成;;不可编辑
    - 依据该物料的基础数据中设置的取样规则.如果设置为每个容器都不需要取样,那么就是"是"
    - 如果需要取样,在取样全部完成后,更新为"是", 否则为"否"
  - 包装类型: 托盘/容器,下拉列表选择其一
- 供应商信息:
  - 供应商编码: 该批次供应商的编码;创建后不可编辑
  - 供应商批号: 供应商的批次号,外部批号;用户手动输入/外部数据导入;创建后可编辑,但接收发生后不可编辑,
  - 供应商名称: 通过编码 load,不可编辑
- 批次信息:
  - 批号: 本次接收的批次号,内部批号;由系统自动生成或由用户手动输入;创建后可编辑,但接收发生后不可编辑;不可重复
    - 系统自动生成规则: RL+8 位数字序列
    - 如果接收到的是成品,系统分配的批号与产品批号一致
  - 批次状态: 通过批号 load,不可编辑
- 物料信息:
  - 物料编码: 来自物料表中已验证的物料;列表选择;创建后不可编辑
  - 物料名称: 通过料号 load,不可编辑
  - 接收数量: 本次接收的数量;用户手动输入或外部系统导入;精度:小数点后 6 位;创建后可编辑,但接收发生后不可编辑;
  - 单位: 物料的单位,物料管理中该物料设定的库存单位;通过料号 load,不可编辑
  - 托盘数/容器数
    - 包装类型为托盘时,统计托盘数,包装类型为容器时,统计容器数
    - 系统按照接收的托盘/容器统计,统计数据,不可编辑
    - 注意,按容器包装的接收,其托盘为系统虚拟托盘
- 其他字段说明见 schema 设计

#### 3.3.2 字段说明-包装接收

- 是否满装: Y/N,不可编辑
- 托盘/容器编号: 接收的托盘/容器编号;系统自动生成:托盘:P+7 为数字序号;容器:C+7 为数字序号;不可编辑
- 类型: 接收的托盘/容器的类型,不可编辑
- 原始库位: 接收的托盘/容器的初始库位,该库位在物料管理模块中的物料上设定;不可编辑
- 取样库位:
  - 接收的托盘/容器的取样库位;
  - 手动设定该物料的取样库位, ~~可用的库位为所有由该物料上所设定的取样库区控制~~ 可用的库位为所有不限容量的库位;
  - 不做库存库位的特性验证;
  - 可以在核准前修改;
  - 由系统按照取样的存放规则提供默认值,除"库位需要和物料的存储要求匹配"要求外,其他规则与目标库位的规则相同;
    - 如果按照规则找不到可存储的库位,则选择取样库区中第一个不限容量库位(系统的取样库区都应该有一个不限容量存储库位)
- 目标库位: 接收的托盘/容器的目标库位; 可以在核准前修改;
  - 可选库位范围:不限容量的库位或存储类型相同的库位
  - 选择库位后的校验:
    - 遵循单批次约束:如果库位上(通过库位号获取库位表中该库位的设定)设定了"单一批次",则同一库位不允许混批,一个库位只能属于一个批次,没有被其他批次占用的库位才可用
    - 库位需要和物料的存储要求匹配,需提示用户是否执行该操作.
    - 库位需是可分配的,不可分配的库位会被忽略
    - 库存单位为 kg 时,库位需符合重量和高度要求,不符合的会被忽略
  - 校验结果:![pic](/pics/批次_库位校验.png)
  - 由系统按照存放规则提供默认值,规则如下:
    - 可用范围: 物料上设定的存放库区中的库位列表:托盘接收:托盘的默认库区及备用库区;容器接收:容器的默认库区及备用库区
    - 遵循可用空间的最小分解规则: 匹配可容纳整个接收的物料的,空间最小的一个库位
      在默认库区和备用库区中,优先选择可以存储整个接收的物料且剩余空间最小的那个,目的是减少库位的占用数;
    - 遵循最大可用空间的原则:
      在默认区和备用库区中,优先选择最大存储能力的库位.目的是减少存储的分散程度,用最少的地点存储物料; 适用范围为需要`多个库位`时
  - 包装完成后,如果该库位为非不限容量存储库位,则增加该库位的预留容量(以 P1 为单位),以备后面入库使用. 入库完成后扣除预留
- 任务状态: 托盘的的状态,枚举`PalletStatus`;不可编辑,容器包装与托盘包装均使用托盘表中的托盘状态(容器包装也会挂在系统虚拟托盘下)
- 容器个数: `托盘接收特有`, 托盘上接收的容器数;不可编辑
- 容器数量: 单个容器接收的物料数量,使用库存单位;精度:小数点后 6 位;不可编辑
  - 存在不满装容器时,该字段为空.
- 托盘数量: `托盘接收特有`, 单个托盘上所有容器接收的物料数量,使用库存单位;小数点后 6 位;不可编辑
- 容器条码: `容器接收特有`; 规则为批次号+ 4 为数字递增序列,核准后系统自动生成

### 3.4 操作说明

#### 3.4.1 排序筛选

- 主界面-接收批次列表
  - 排序
    - 默认排序: 按照批次号升序
    - 字段范围: 订单号,接收日期,批次号
      - (暂不实现)供应商编码
  - 筛选
    - 默认筛选: 未处理的接收批次
    - 字段范围: 厂区,订单号,供应商编码,物料编码,物料名称
      - (暂不实现)接收事务类型,批次接收状态,取样状态
    - 支持扫码查询
- 托盘包装接收/容器包装接收
  - 排序
    - 默认排序: 按照托盘/容器编号升序
  - 筛选 无

#### 3.4.2 创建接收单

- 添加:添加一个批次物料接收单,后续使用该计划单据进行接收,批次接收状态为`未处理`,添加后记录批次接收数据,后续按照实际接收更新接收数据.
- 创建计划的接收单,留待后续接收使用.
  - 批次接收添加操作之后,不进行包装,只确认添加.
  - 只创建接收信息,不创建批次信息(批次表不添加数据,因为接收的批次没有发生)

#### 3.4.3 失效

- 失效:只能删除`未处理`状态的`本地接收`批次, 即未进行包装的空的批次接收可以被删除,删除后不可恢复
- 删除接收单

#### 3.4.4 取消

- 取消:只能取消`本地接收`批次,取消后,不可恢复,取消后批次接收状态为`取消`
- 更新接收单的批次接收状态为`取消`
- 取消的接收单被冻结,不再继续操作.

#### 3.4.5 明细

- 明细: 显示该单的明细信息,可进行进一步接收操作

##### 3.4.5.1 包装

- 选择包装方式之后,切换到对应包装方式的界面.
- 包装,确认或维护托盘/容器包装的包装方式信息,按照包装方式,生成包装结果.
- 托盘包装
  - 结构:![pic](/pics/库存_托盘包装.png)
  - 库位调整:![pic](/pics/库存_包装_库位调整.png)
  - 托盘包装检查项:
    - 初始库位; 取样库区(如果物料需要取样);
    - 容器默认库区;
    - 容器备用库区;
    - 取样规则(不需要取样的,在物料维护中选择"不需取样")
    - 标签规则
  - 检查项维护: 跳转到该物料的维护界面,让有权限的用户进行物料维护,(待定)
  - 包装方式明细: 托盘包装方式维护(调用"托盘包装方式的明细")
  - 按照包装方式,生成本次接收的托盘列表
- 容器包装: 直接使用容器
  - 结构:![pic](/pics/库存_容器包装.png)
  - 库位调整:![pic](/pics/库存_包装_库位调整.png)
  - 包装方式明细: UI 示例如下.
    - ![pic](/pics/批次_容器包装方式明细.png)
    - 总计: 本次接收的容器个数
    - 其他字段说明: 同托盘包装方式(容器包装的明细不需要单独的数据表存储)
  - 按照所设定的容器数,生成容器列表
- 调整: 核准前允许该操作
  - 托盘包装:只对不满装托盘进行调整
    - 调整的对象是托盘上的物料的包装信息, 即容器的信息: 容器个数,各容器的容量;UI 示例如下.
    - ![pic](/pics/批次_批次接收_托盘调整.png)
    - ![pic](/pics/批次_批次接收_托盘调整_添加.png)
    - 调整规则:
      - 托盘上的物料的总数不变
      - 只能调整不满拍的
      - 类型不允许调整
      - 操作: 添加/编辑/删除
  - 容器包装: 调整的对象是物料的包装信息,容器的信息: 各容器的容量,不调整容器个数;UI 示例如下.
    - ![pic](/pics/批次_批次接收_容器调整.png)
    - ![pic](/pics/批次_批次接收_容器调整_编辑.png)
    - 调整规则:
      - 所有容器的汇总量不变
      - 各个容器都可以调整
      - 数量,类型和皮重均可调整
      - 操作: 编辑/初始化(初始化所有容器的容量)
- 包装完成后,批次接收状态为`未处理`,包装操作的后续操作为核准,或取消包装.

##### 3.4.5.2 核准

- 校验所有的目标库位是否已指定, 核准后,将接收的物料按照包装方式和库位信息,将托盘存入到初始库位;此时占用的是预留容量
- 核准后,系统为托盘或容器生成标签;考虑不换签的情况,系统需提供标签替换功能
- 核准后批次接收状态变为`已接收`,不可取消操作,可进行后续打标操作
- 核准后,记录批次信息到批次表:批次号,物料编码,批次 ID,批次状态,供应商编码,供应商批次号,批次取样状态,取样日期,参考,决定日期,容器数量,接收日期,效价 1,效价 2,密度,分析日期,限制使用日期,有效期,包装说明
- 核准后,记录容器信息到容器表:物料编码,批次号,批次容器个数,批次容器序号,容器类型,容器状态,容器 ID,托盘编号,托盘皮重,皮重单位,标准容量,剩余量,结束容器(FALSE),二次包装,批次已接收容器数量,批次接收容器序号,取样状态,取样数
- 核准后,记录托盘信息到托盘表:托盘编号,物料编码,批次号,接收事务类型,订单号,接收序号,托盘类型,托盘状态,是否满装,初始库位,目标库位,取样库位,皮重,皮重单位,标准容量,单位,托盘数量,托盘序号
- 核准后,记录或更新(按唯一索引更新)批次接收信息(创建接收单后,没有进行包装接收时,更新已有接收信息):厂区,区域,接收事务类型,订单号,接收序号,物料编码,接收日期,接收数量,存储类型,批次号,供应商批次号,供应商编码,接收状态,是否已取样,首托盘编号,尾托盘编号
- 核准后,按接收情况调整(限制容量的)库位的预留容量.
- 核准后,更新库存(Inv)信息:物料编码,批次号,库位号,库存量;按物料编码+批次号+库位编码插入/更新

##### 3.4.5.3 标签

- 标签打印: ![pic](/pics/批次_批次接收_标签.png)
- 首次打标:打印所有标签
  - 托盘包装的打印托盘标签和容器标签,界面`按托盘打印`可用,需指定托盘和容器的标签模板
  - 容器包装的只打印容器标签,`按容器打印`可用,需指定容器标签模板
- 打标重打:
  - 打印之后可以进行标签重打(如果需要,可以为重打的标签,设计与首次打标不同的样式, 如添加"REPRINT"字样)
  - 标签的重新打印,也可以通过[库存模块]的[托盘管理]或[容器管理]的标签重打功能实现.
- 托盘接收的标签打印: 托盘标签+ 每个托盘上各个容器的标签(含取样的容器) + ~~符合取样数的取样标签~~
- 容器接收的标签打印: 各个容器的标签(含取样的容器) + ~~符合取样数的取样标签~~
- 标签打印后,批次接收状态变为`已出标`,不可取消操作,可进行后续取样操作
- 打标后,为容器添加标签信息:标签编码,标签模板的编码

##### 3.4.5.4 取样

- 打标之后可以进行取样,先将需要取样的托盘存入从初始库位移到取样库位;取样之后才可进行入库,具体取样操作使用在[容器取样]功能.批次接收的取样为取样完成之后的核准,若取样全部完成后已更新批次的接收状态,则此处可以不操作;
- 取样后,需调整库存
  - 库位调整
  - 库存扣减,扣减取样数量.
- 不需取样的,在打标之后,批次接收状态直接到`等待入库`,不可取消操作.
- 流程如下:
  - ![pic](/pics/批次_批次接收_取样流程说明.png)

##### 3.4.5.5 入库清单

- 入库清单:打印待入库清单

##### 3.4.5.6 入库

- 入库: 确认入库完成,调整已预留容量,入库后,为实际库存.
- 入库后: 批次接收状态变为`已入库`,不可取消操作.
- 入库后: 相关托盘状态变为`已入库`,不可取消操作.
- 入库后: 将托盘存入到目标库位
- 入库后,按接收情况调整(限制容量的)库位的预留容量.
- 入库后,更新库存(Inv)信息:物料编码,批次号,库位号,库存量;,按物料编码+批次号+库位编码插入/更新

### 3.5 操作限制(暂不实现)

- 同时编辑检查(暂不实现):除新增外的其他操作,需验证该批次/接收是否正在被其他用户编辑,如果是,则不允许操作,并提示`该批次正在被其他用户编辑`
- 已删除检查(暂不实现):除新增外的其他操作,需验证该批次/接收是否已被删除,如果是,则不允许操作,并提示`该批次已被删除`

### 3.6 唯一索引与特性

- 唯一索引:由四部分组合:厂区+ 类型+订单号+序号
- 特性:
  - 每次接收只能接收一种料,以及该物料的接收数量,该物料可以来自供应商的批次,也可以来自生产的内部批次
  - 一个单号可以有一组不同的物料,同一单号的物料的接收,次序号递增,起始值为`00001`
  - 接收拆分: `同一批次可以多次接收`
    - 区分拆分的字段: 序号也用来链接初始单号的接收部分, 序号的前两位用于批次拆分的计数:00 开始递增,最大 99
    - 实际接收数量<预计接收数量时,发生接收拆分
    - 需要拆分的批次,在第一次接收时,按照剩余量同时为下次接收生成一笔待接收记录, 序号为 `01001`
    - 在标签打印之前,已接收部分,接收状态为"已接收", 打印后更新为"已出标"
    - 取样按照拆分之后的批次进行的

---

### 3.7 其他业务说明:

- 核准后所有字段不可修改:
- 在包装与调整完成时
  - 会在目标库位创建托盘或容器
  - 会按照将接收的批次物料在目标库位增加库存(Inv)信息
  - 计算目标库位的存储空间
  - 如果接收的物料的取样规则为"不需取样",则将"是否已取样"状态更新为 True
  - 记录接收台账
- 容器都是挂在托盘下的,容器直接接收的,挂在一个系统虚拟的托盘下,该托盘的托盘编号为容器编码

### 3.8 批次接收追溯

- 批次接收相关的追溯信息
- 追溯内容:
  - <常规字段>
  - 业务追溯字段:
    - receiptType: 接收事务类型
    - orderNumber: 订单号
    - seqNo: 次序号
    - lotNo: 批次号
    - lotStatus: 批次状态
    - materialCode: 物料编码
    - materialName: 物料名称
    - qty: 接收数量
    - unit: 单位
    - supplierCode: 供应商编码
    - supplierName: 供应商名称
    - supplierLot: 供应商批次号
    - receiptDate: 接收日期
    - palletQty: 已接收托盘数
- 排序
  - 默认排序: 按照操作时间降序
- 筛选
  - 默认筛选: 无
  - 字段范围: <常规字段>,接收事务类型,订单号,批次号,物料编码,供应商编码,供应商批次号,接收日期
- 打印
  - 按照指定格式打印筛选出的追溯内容

## 4. 批次追溯

- 批次接收相关的追溯信息
- 追溯内容:
  - <常规字段>
  - 业务追溯字段:
    - lotNo: 批次号
    - lotStatus: 批次状态
    - materialCode: 物料编码
    - materialName: 物料名称
    - workOrder: 工单编号
    - workOrderVersion: 工单版本
    - lotNoProduced: 生产批次号
    - supplierLot: 供应商批次号
    - Qty: 事务数量
    - unit: 单位
- 排序
  - 默认排序: 按照操作时间降序
- 筛选
  - 默认筛选: 无
  - 字段范围: <常规字段>,物料编码,批次号,生产批次号
- 打印
  - 按照指定格式打印筛选出的追溯内容
