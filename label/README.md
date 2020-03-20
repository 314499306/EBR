# 标签管理

---

## 1. 打印机支持产品范围

- 产品支持 ZPL 指令集的打印机

---

## 2. 标签模板设计

- 使用第三方工具设计,如 zebra designer,设计完成后生成 ZPL 指令集

---

## 3. 标签组与标签维护

- 产品支持的标签组为出厂时已设计的组别,不支持标签组的扩展(可工程上定制).
- 按标签组维护标签的样式:可以为标签组提供不同的样式(样式的设计使用第三方工具),即支持样式扩展.
- 标签组的可用数据: 系统按照标签组提供样本字段. 超出该样本范围的业务字段,系统不提供支持.

---
## 4. (暂不实现)替代方案
- 使用报表工具中间件实现模板的设计
- 存储和识别复用当前方案.

## 5. (暂不实现)优化
- 标模板管理中设置标示:标准模板/默认模板;
- 在使用模板时提供两种类型的选项
  - 标准:默认选项,使用模板管理中标示为标准的模板
  - 自定义:提供下拉列表,从该标签类型的可用模板列表中选择.