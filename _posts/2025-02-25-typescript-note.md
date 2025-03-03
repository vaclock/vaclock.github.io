---
title: TypeScript 实战
tags:  Typescript
---

TypeScript 不仅仅是在 JavaScript 基础上增加类型约束，它是一门独立的语言，拥有自己的编译器（如 tsc 或 babel）。其中，类型系统和类型运算（包含继承，如逆变和协变）是 TypeScript 的核心特色。

> [练习](https://tsch.js.org/)

<!--more-->

## 类型

## 内置高级类型

1. `PropertyKey`: 联合类型，可以作为对象属性的值类型 `string | number | symbol`
2. `Omit<T, P>`: 剔除类型`T`中某些`P`属性
