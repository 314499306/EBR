# [设计]系统字典
#系统字典
存储业务中应用的字典信息
#设计
- 存储业务系统字典数据,系统字典数据在系统初始化时候内置(无特殊情况不会修改),包括数据多语言信息.
```graphql
"""
系统字典
"""
type SysDictionary implements Base {
  "字典id"
  id: ID! @unique
  "字典类型"
  type: DictionaryType!
  "编码"
  code: String!
  "本地名称,如:中文"
  name: String!
  "英文名称"
  name_en: String!
  "说明"
  description: String
  "排序号"
  orderNo: Int @default(value: 0)
  "创建时间"
  createdAt: DateTime!
  "更新时间"
  updatedAt: DateTime!
}

```
- 系统字典类型,用于区分多个类型的数据字典,等同于字典分组标记
```graphql
"""
字典类型
"""
enum DictionaryType {
   "ABCcode"
   ABCcode
   "物料效价类型2"
   PotencyType2
}
```

- 模型设计约定,如dic是需要引用字典的字段,数据类型使用```String``` 添加 指令```@dictionary```
```graphql
type testDIC {
  "ID"
  id: ID! @unique
  "更新时间"
  dic: String! @dictionary(type:ABCcode)
}
```
- 指令解释:
@dictionary(type:ABCcode)
  │                │
  │                └── 字典类型 `DictionaryType` 的枚举值
  └── 字典的指令名称


#数据添加
- ```dic```字段为```String!```,直接存储为字符串
- ```dic```的元数据来自 ```sysDictionaries```模型,根据```type```过滤后的结果.
```graphql
mutation createtestDIC{
    createtestDIC(data:{
      dic:"010204"
    }){
      id
    }
  }
```

#数据查询
- ```dic```字段为添加了```dictionary```指令,在查询后就会被翻译为字典对象
```graphql
query {
    testDICs{
      id
      dic
    }
  }
```

- 返回数据:
```graphql
{
  "data": {
    "testDICs": [
      {
        "id": "cjtzes9ej00r007155oriw14v",
        "dic": {
          "code": "code",
          "name": "翻译后的语言", //取 Accept-Language 头部中 权重最高的语言
        }
      }
    ]
  }
}
```

