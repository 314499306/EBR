# Schema

## 1. 分支说明

- schema 分支供开发使用

## 2. 模块说明

- bussinessEnum:业务枚举类型,所有的业务枚举均存放在此处同一文件中.
- equipment:设备管理
- formula:处方管理
- inventory:库存管理
- lot:批次管理
- material:物料管理
- mi:工艺管理
- quality:质量管理
- sysDictionary:业务所用系统字典表
- weighing:称量管理
- workorder:工单管理
- auditTrace:审计追溯
- factoryMode:工厂信息
- label:标签管理
- mfgDeclaration:生产声明
- quality:质量管理

## 3. 信息处理说明:

- 同时编辑检查(暂不实现):信息不可以被多个用户同时编辑
- 已删除检查(暂不实现):使用信息时,检查该信息是否已被删除,如已删除,则不允许进行除激活之外的操作
- 信息被使用时,一般不允许进行删除

## 4. 安全说明:

- 系统设计的假设条件:

1.  企业内部用户不会攻击系统
2.  本产品的常规网络环境为企业内部生产域

- 本产品设计的主题是业务,有基本的安全性设计即可:如常规的访问控制,非明文密码等等.

## 5. 排序筛选

- 界面上需要排序与筛选的字段,在需求设计的说明中,明确说明,不应依赖具体技术而省略该部分
- 回车查询要求:明确需要筛选的字段,默认都需要回车查询要求.

## 6. 字段长度的常规要求

- unit,最大长度:4
- code/id,最大长度:30
- enterprise/site/area/loc/zone/emp code,最大长度:10
- version,最大长度:(2,0)
- seq,最大长度:(4,0)
- name,最大长度:100
- comment,最大长度:200
- float-Normal,最大长度:(10,3)
- float-Inv,最大长度:(10,6)
- float-偏差/精度,最大长度:(10,6)
- int,最大长度:(10,0)
- Date,默认显示格式与精度要求:YYYY-MM-DD HH24:MI:SS
- createUser,最大长度:30
- updateUser,最大长度:30
- barcode,最大长度:60

#### 6.1 说明

- 举例:最大长度:(10,3) 最多 10 位数字，且精度为小数点后 3 位

## 7. 符号要求

- 要求:尽量使用英文标点符号
  - 逗号统一使用英文","
  - 冒号统一使用英文":"
  - 分号统一使用英文";"
  - 句号统一使用英文"."
- 说明
- 便于双击复制,即方便断词断句.
- 避免 schema 语法错误.
- 有益于规范排版

## 8. 其他 Schema 设计注意事项

### 8.1 关系

- 模块内使用 ID 引用(使用 ID 引用,无法使用默认值)
- 模块间使用值复制
- 用于唯一索引的可使用 @relation(link: INLINE)

### 8.2 唯一索引

- 业务唯一索引使用@unique 指令标示,组合索引写在注释文档中
  - 如: code: String! @unique;
  - 如: 托盘包装方式的唯一索引: materialCode + supplierCode

### 8.3 字段默认值的设定

- 使用@default
- 如: isSampled: Boolean @default(value: false)

### 8.4 Type 系统字典表的使用

- 使用 @dictionary(type: TypeName)
- 如: simpledUnit: String @dictionary(type: EBR_UNIT)

### 8.5 Type 强制字段指令要求

```graphql
 "ID"
 id: ID! @id
 "创建时间"
 createdAt: DateTime! @createdAt
 "修改时间"
 updatedAt: DateTime! @updatedAt

 "修改时间"
 cancelDate: DateTime @systemDate
```

### 8.6 使用系统时间

```graphql
 "修改时间"
 cancelDate: DateTime @systemDate
```

### 8.7 枚举与字典

- 优先枚举,枚举不足以表达的可用字典表存储
- 有后续扩展可能的尽量使用字典

### 8.8 冗余

- 对于模块间已用,目前可将验证后及资源被占用不可编辑的信息,作为冗余信息存储到外部模块
- 冗余信息的添加遵循保守原则.

## 9. 业务PIC
| 模块 | 姓名 | 员工号 |
| ------ | ------ | ------ |
| 审计追溯,集中 | 王峰 | 183462 |
| 称量 | 金梓奕 | 183418 |
| 其他模块 | 王峰 | 183494 |