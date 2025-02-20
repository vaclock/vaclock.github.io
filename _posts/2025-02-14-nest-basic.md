---
title: Nest基础知识
tags: 后端 Nest
---

Nest 作为 nodejs 最流行的后端框架，而且有很多优秀的设计理念，但是看完文档，再不经常使用 Nest 进行开发的情况下，过段时间马上就又忘记了里面一些基础知识与一些内置功能的基础用法，这里将所有非业务知识的基础用法做一个记录

> **声明**: 本文只涉及 Nest 基础知识，不涉及后端架构体系 以及 Nest 业务解决方案
> 参考来源: [NestJS官方文档](https://docs.nestjs.com/)

<!--more-->

## Nest架构

### 平台无关

Nest 框架本身提供了最基础的`HttpServer`与`AbstractHttpAdapter`, 你可以自定义一个`extends AbstractHttpAdapter implements HttpServer`的适配器

1. `HttpServer`：框架的核心接口
2. `AbstractHttpAdapter`：HTTP适配器的抽象基类
3. `FastifyAdapter`：基于 `Fastify` 的 HTTP 适配器, Nest 内部目前实现`express`,`fastify`, `socket.io`, `ws` 适配器

### **AOP**

AOP（面向切面编程）是一种用于分离关注点的编程思想。在Web应用中，通常有不同的接口请求，每个接口对应一个`Controller`模块，而`Controller`中处理的逻辑一般是与业务相关的具体操作。但是，针对不同的`Controller`，可能会有一些公共逻辑需要处理，比如权限判断、日志记录、异常捕获等。将这些公共逻辑直接放在`Controller`、`Service`或`Repository`中不太合适，因为这样会破坏模块的**单一职责原则**，增加不必要的耦合。因此，我们需要将这些逻辑从业务模块中抽离出来，单独构造一层来处理这些横切关注点，这就是AOP的作用。

在NestJS中，AOP可以通过不同类型的拦截器（Interceptors）来实现，Nest提供了五种关键的工具来处理这些横切关注点：**Middleware**、**Interceptor**、**Guard**、**Pipe**和**Filter**。

1. **Middleware**：用于处理请求生命周期中的前置和后置逻辑，可以在请求到达路由处理之前和响应返回之前执行一些操作。比如，可以用`Middleware`来进行请求日志记录、权限检查等操作。

2. **Interceptor**：拦截器用于对请求和响应进行预处理和后处理。例如，日志记录、性能监控、修改请求/响应数据等。拦截器提供了更强大的功能，可以修改请求、响应，或者进行方法调用的拦截。

3. **Guard**：用于授权（授权控制），例如验证请求是否符合某些条件（如权限、认证等），是用来处理应用安全性的一种机制。通常在路由处理之前进行权限控制。

4. **Pipe**：用于输入验证和数据转换。`Pipe`可以在请求到达业务逻辑之前，验证和转换输入数据，确保数据符合预期的格式和要求。

5. **Filter**：用于捕获异常并返回友好的错误响应。异常过滤器帮助你捕获未处理的异常，并将它们转换为HTTP响应，避免暴露详细的错误信息给用户，增强系统的安全性和稳定性。

### **IOC**

IOC（控制反转）是一种设计思想。在后端开发中，我们有多个模块，如处理路由请求的`Controller`，处理业务逻辑的`Service`，以及执行数据库操作的`Repository`等，每个模块都有明确的职责，并且它们之间通常是相互依赖的。如果直接在`Controller`中通过`new`实例化`Service`，就会导致模块之间耦合度过高，这不仅使得代码的维护变得困难，也让测试变得复杂。为了避免这种紧密耦合，我们需要一个工具来管理模块之间的依赖，这就是**IOC容器**的作用。Nest框架通过**依赖注入（DI）**来实现**控制反转（IOC）**，当然，`IOC`还有很多其他的实现方式，如`ServiceLoader`等，你可以使用Nest的装饰器，如`@Injectable()`，并通过构造函数注入的方式，自动管理模块的依赖关系。

```js
// 模拟数据库
class Repository {
  constructor() {
    this.data = [{ id: 1, name: 'John' }, { id: 2, name: 'Jane' }]
  }
  read() {
    return this.data
  }
}

// 模拟业务逻辑
class Service {
  constructor(private repository: Repository) {}
  findById(id: string) {
    const data = this.repository.read()
    return data.find(item => item.id === id)
  }
}

// 模拟路由处理逻辑
class Controller {
  constructor(private service: Service) {}

  execute() {
    return this.service.findById('1')
  }
}

// 模拟IOC容器
class IocContainer {
  constructor() {
    this.services = {}
  }
  register(name, dependencies, implementation) {
    this.services[name] = {
      dependencies,
      implementation
    }
  }
  resolve(name) {
    const { dependencies, implementation } = this.services[name]
    return new implementation(...dependencies.map(dep => this.resolve(dep)))
  }
}

// 使用IOC容器管理依赖
// controller -> service -> repository
const container = new IoCContainer()
container.register('repository', [], Repository)
container.register('service', ['repository'], Service)
container.register('controller', ['service'], Controller)

// 执行逻辑，所有controller的直接依赖与间接依赖都通过IOC容器自动注入到controller中
container.resolve('controller').execute()

```

### **DI**

## 专有名词解释

## 数据传输方式

> 5种常见的数据传输方式

1. urlParams: `http://api/users/1`
2. query: `http://api/users?id=1`
3. form-urlencoded: 数据放到了body中, 请求方需要encoded, 并且指定`content-type` 为 `application/x-www-form-urlencoded`, 格式为`key=value&key2=value2`
4. form-data: 数据在body中, 请求方不需要encoded, 并且指定`content-type`为`multipart/form-data` 每个数据之前通过`--------xxxxx`分割
5. json: 数据在body中, 请求方指定`content-type`为`application/json`, 格式为`{"key": "value"}`
