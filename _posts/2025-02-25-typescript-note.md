---
title: TypeScript 实战
tags:  Typescript
---

TypeScript 不仅仅是在 JavaScript 基础上增加类型约束，它是一门独立的语言，拥有自己的编译器（如 tsc 或 babel）。其中，类型系统和类型运算（包含继承，如逆变和协变）是 TypeScript 的核心特色。此外，类型系统是图灵完备的，可以写各种逻辑。

> [练习](https://tsch.js.org/)

<!--more-->

## 模式提取

`infer`

## 递归

支持自己调用自己，需要传递泛型参数

## 数值计算

类型变量无法和普通int变量一样直接进行四则运算，但是由于`T['length']`返回的是T这个元组的长度，所以可以通过构造不同的数组长度，进行数值运算.

构造指定长度的数组

```ts
type BuildArray<Len extends number, Arr extends unknown[] = []> =
  Arr['length'] extends Len
  ? Arr
  : BuildArray<Len, [...Arr, unknown]>;
```

这样就构造出了一个长度为`Len`的数组, 通过对数组的提取, 来完成运算

- 加法: 构造出`N1`长度的数组，构造出`N2`长度的数组，然后合并，取`['length']`

```ts
type Add<N1 extends number, N2 extends number> = [...BuildArray<N1>, ...BuildArray<N2>]['length']
```

- 减法：TypeScript是静态的，ts的类型系统没有**类型级别**的计算能力。

```ts
type Subtract<N1 extends number, N2 extends number> = BuildArray<N1> extends
  [...arr1: BuildArray<N2>, ...arr2: infer Rest]
  ? Rest['length']
  : never;

// 如果要支持N1 < N2的情况, 则如下，只能返回字符串
type Subtract<N1 extends number, N2 extends number> =
  BuildArray<N1> extends [...arr1: BuildArray<N2>, ...arr2: infer Rest]
  ? Rest['length']
  : BuildArray<N2> extends [...arr1: BuildArray<N1>, ...arr2: infer Rest]
  ? StringToNumber<`-${Rest['length']}`>
  : never;

// 字符串转数字，但是也不支持负数
type StringToNumber<T extends string, R extends unknown[] = []> = T extends
  `${R['length']}`
  ? R['length']
  : StringToNumber<T, [...R, unknown]>;
```

- 乘法: `N1` * `N2` = `N1 + N1 + ... + N1`, 将`N1`相加`N2`次

```ts

```

## 特性

### 联合分散

## 内置高级类型

1. `PropertyKey`: 联合类型，可以作为对象属性的值类型 `string | number | symbol`
2. `Exclude`: `type Exclude<T, U> = T extends U ? never : T`
3. `Omit`: 剔除类型`T`中某些`P`属性
4. `Pick`: 选取类型`T`中某些`P`属性

## 原理
