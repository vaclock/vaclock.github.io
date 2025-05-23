---
title: React 知识记录
tags: React
---

记录 react 基础知识点，防止遗忘。

<!--more-->

## state

## 数据传递方式

### props

### ref

### context

## 生命周期

## 高阶组件

## 渲染调优

### 避免子组件的重复渲染

使用`React.memo`、`useMemo`、`useCallback`来优化

- `React.memo`: `memo(Component, areEqual?)` 返回值是一个新的组件, 当上一次 props 和下一次 props 相同时(或者 areEqual 返回为真), 不会重新渲染, React 推荐用于当属性值未改变时跳过重新渲染

- `useMemo`: `const memoizedValue = useMemo(() => returnValue, [deps]);` 只在依赖项改变时重新计算值, React 推荐用于 在重新渲染时缓存计算结果

- `useCallback`: `const memoizedCallback = useCallback(fn, [deps]);` 只在依赖项改变时重新创建回调函数
有个 hack 的方式, 因为函数组件编译后本身会转换成一个类似 `React.createElement` 的函数, 所以可以利用这点达到和 `memo` 类似的效果

详见[react 渲染优化](https://codesandbox.io/p/sandbox/reactxuan-ran-you-hua-vwx9m4)

两者不同点:

1. 比较规则不同: `memo` 使用 === 来比较两次渲染的 `props`, `useMemo` 使用 `Object.is` 比较依赖项
2. 语义不同: `memo` 用于组件, `useMemo` 用于计算值

### PurComponent

## 组件库
