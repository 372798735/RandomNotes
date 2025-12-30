# 一、typeScript

## 1、Omit

含义：Omit是typeScript内置的工具类型，用于从一个类型中排除指定的属性，返回一个不包含这些属性的新类型，或者重新定义排除属性的类型

```typeScript
Omit<T,K>
// T: 要操作的原始类型
// K:要排除的属性名(可以是单个属性或多个属性的联合类型)

/** 例子 */
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
}
type UserBasic = Omit<User, 'password' | 'email'>;
// 结果: { id: number; name: string; }
```
