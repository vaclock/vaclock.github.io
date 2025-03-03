---
title: 设计模式
tags: 设计模式 C++
---


在日常工作中，优秀的代码通常会遵循`SOLID`原则，在c++中，简直将设计模式的优点体现到了极致，尤其是在观看了 李建忠 老师的视频之后，在这里对知识进行记录

> 参考来源: [设计模式](https://www.bilibili.com/video/BV1Yr4y157Ci)

<!--more-->

## 面向对象设计原则

### 为什么需要面向对象设计？

面向对象设计的核心优势在于**抵御变化**。软件开发中，变化是复用的天敌，而良好的面向对象设计能够：

- **宏观层面**：适应软件变化，将变化影响最小化
- **微观层面**：强调类的"责任"，使新增类型不影响原有实现

### 对象的本质

- **实现层面**：封装代码和数据
- **规格层面**：一系列可被使用的公共接口
- **概念层面**：拥有责任的抽象

### SOLID原则

> 以下原则中的demo由于简洁性, 无法体现原则的威力, 后续需要记录业务级别项目的代码

#### 1. 单一职责原则 (Single Responsibility Principle, SRP)

**核心**：一个类应该仅有一个引起它变化的原因

##### TypeScript示例

```typescript
// Bad Case: 一个类承担多种职责
class Employee {
  calculatePay(): number { /* 计算薪资 */ return 0; }
  reportHours(): number { /* 报告工时 */ return 0; }
  saveEmployee(): void { /* 保存员工信息到数据库 */ }
}

// Good Case: 职责分离
class Employee {
  id: number;
  name: string;
  position: string;
}

class PayCalculator {
  calculatePay(employee: Employee): number { /* 计算薪资 */ return 0; }
}

class TimeReporter {
  reportHours(employee: Employee): number { /* 报告工时 */ return 0; }
}

class EmployeeRepository {
  save(employee: Employee): void { /* 保存员工信息 */ }
}
```

##### C++示例

```cpp
// Bad Case
class Report {
public:
    void generateReport(const Data& data) { /* 生成报告 */ }
    void formatReport() { /* 格式化报告 */ }
    void printReport() { /* 打印报告 */ }
    void saveToFile(const std::string& filename) { /* 保存到文件 */ }
};

// Good Case
class ReportGenerator {
public:
    Report generate(const Data& data) { /* 生成报告 */ return Report(); }
};

class ReportFormatter {
public:
    FormattedReport format(const Report& report) { /* 格式化 */ return FormattedReport(); }
};

class ReportPrinter {
public:
    void print(const FormattedReport& report) { /* 打印 */ }
};

class ReportStorage {
public:
    void save(const Report& report, const std::string& filename) { /* 保存 */ }
};
```

#### 2. 开放封闭原则 (Open-Closed Principle, OCP)

**核心**：对扩展开放，对修改封闭

##### TypeScript示例

```typescript
// Bad Case: 添加新形状需要修改计算器类
class AreaCalculator {
  calculateArea(shape: any): number {
    if (shape.type === 'circle') {
      return Math.PI * shape.radius * shape.radius;
    } else if (shape.type === 'rectangle') {
      return shape.width * shape.height;
    }
    return 0;
  }
}

// Good Case: 添加新形状只需创建新类
interface Shape {
  calculateArea(): number;
}

class Circle implements Shape {
  constructor(private radius: number) {}
  
  calculateArea(): number {
    return Math.PI * this.radius * this.radius;
  }
}

class Rectangle implements Shape {
  constructor(private width: number, private height: number) {}
  
  calculateArea(): number {
    return this.width * this.height;
  }
}

class AreaCalculator {
  calculateArea(shape: Shape): number {
    return shape.calculateArea();
  }
}

// 扩展新形状不需要修改现有代码
class Triangle implements Shape {
  constructor(private base: number, private height: number) {}
  
  calculateArea(): number {
    return (this.base * this.height) / 2;
  }
}
```

##### C++示例

```cpp
// Bad Case
class PaymentProcessor {
public:
    void processPayment(const std::string& type, double amount) {
        if (type == "credit") {
            // 处理信用卡支付
        } else if (type == "debit") {
            // 处理借记卡支付
        } else if (type == "cash") {
            // 处理现金支付
        }
    }
};

// Good Case
class Payment {
public:
    virtual void process(double amount) = 0;
    virtual ~Payment() {}
};

class CreditCardPayment : public Payment {
public:
    void process(double amount) override {
        // 处理信用卡支付
    }
};

class DebitCardPayment : public Payment {
public:
    void process(double amount) override {
        // 处理借记卡支付
    }
};

class CashPayment : public Payment {
public:
    void process(double amount) override {
        // 处理现金支付
    }
};

// 添加新支付方式只需创建新类，无需修改现有代码
class CryptoPayment : public Payment {
public:
    void process(double amount) override {
        // 处理加密货币支付
    }
};
```

#### 3. 里氏替换原则 (Liskov Substitution Principle, LSP)

**核心**：子类必须能够替换其基类 (约束其实不仅在子类, 对父类的设计要求更为严格, 一个不合理的父类, 必然会导致LSP的发生, 父类应该提供更抽象的方法)

##### TypeScript示例

```typescript
// Bad Case: 违反LSP的继承关系
class Rectangle {
  constructor(protected width: number, protected height: number) {}
  
  setWidth(width: number): void {
    this.width = width;
  }
  
  setHeight(height: number): void {
    this.height = height;
  }
  
  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  constructor(size: number) {
    super(size, size);
  }
  
  // 破坏了Rectangle的行为
  setWidth(width: number): void {
    this.width = width;
    this.height = width;
  }
  
  setHeight(height: number): void {
    this.width = height;
    this.height = height;
  }
}

// Good Case: 使用组合而非继承
interface Shape {
  getArea(): number;
}

class Rectangle implements Shape {
  constructor(private width: number, private height: number) {}
  
  setWidth(width: number): void {
    this.width = width;
  }
  
  setHeight(height: number): void {
    this.height = height;
  }
  
  getArea(): number {
    return this.width * this.height;
  }
}

class Square implements Shape {
  constructor(private size: number) {}
  
  setSize(size: number): void {
    this.size = size;
  }
  
  getArea(): number {
    return this.size * this.size;
  }
}
```

##### C++示例

```cpp
// Bad Case
class Rectangle {
protected:
    int width;
    int height;
    
public:
    Rectangle(int w, int h) : width(w), height(h) {}
    
    virtual void setWidth(int w) { width = w; }
    virtual void setHeight(int h) { height = h; }
    
    int getWidth() const { return width; }
    int getHeight() const { return height; }
    
    int getArea() const { return width * height; }
};

// 看似合理的继承关系（数学上正方形是特殊的矩形）
class Square : public Rectangle {
public:
    Square(int size) : Rectangle(size, size) {}
    
    // 重写方法以保持正方形特性
    void setWidth(int w) override {
        width = w;
        height = w;  // 保持正方形特性，宽高相等
    }
    
    void setHeight(int h) override {
        width = h;   // 保持正方形特性，宽高相等
        height = h;
    }
};

// 使用Rectangle的代码
void processRectangle(Rectangle& r) {
    r.setWidth(5);
    r.setHeight(10);
    
    // 期望面积为50
    if (r.getArea() != 50) {
        std::cout << "Unexpected area: " << r.getArea() << std::endl;
    }
}

int main() {
    Rectangle rect(3, 4);
    processRectangle(rect);  // 正常工作，面积为50
    
    Square square(4);
    processRectangle(square);  // 违反LSP！面积为100而非50
    
    return 0;
}

// Good Case
// 更好的设计：使用接口和组合
class Shape {
public:
    virtual int getArea() const = 0;
    virtual ~Shape() = default;
};

class Rectangle : public Shape {
protected:
    int width;
    int height;
    
public:
    Rectangle(int w, int h) : width(w), height(h) {}
    
    void setWidth(int w) { width = w; }
    void setHeight(int h) { height = h; }
    
    int getWidth() const { return width; }
    int getHeight() const { return height; }
    
    int getArea() const override { return width * height; }
};

class Square : public Shape {
private:
    int side;
    
public:
    Square(int size) : side(size) {}
    
    void setSide(int s) { side = s; }
    int getSide() const { return side; }
    
    int getArea() const override { return side * side; }
};

// 使用Shape的代码
void processShape(const Shape& shape) {
    std::cout << "Area: " << shape.getArea() << std::endl;
}
```

#### 4. 接口隔离原则 (Interface Segregation Principle, ISP)

**核心**：不应强迫客户依赖它们不用的方法

##### TypeScript示例

```typescript
// Bad Case: 一个大而全的接口
interface Worker {
  work(): void;
  eat(): void;
  sleep(): void;
}

class Human implements Worker {
  work() { /* 工作 */ }
  eat() { /* 吃饭 */ }
  sleep() { /* 睡觉 */ }
}

class Robot implements Worker {
  work() { /* 工作 */ }
  eat() { throw new Error("Robots don't eat"); }  // 被迫实现不需要的方法
  sleep() { throw new Error("Robots don't sleep"); }  // 被迫实现不需要的方法
}

// Good Case: 分离的接口
interface Workable {
  work(): void;
}

interface Eatable {
  eat(): void;
}

interface Sleepable {
  sleep(): void;
}

class Human implements Workable, Eatable, Sleepable {
  work() { /* 工作 */ }
  eat() { /* 吃饭 */ }
  sleep() { /* 睡觉 */ }
}

class Robot implements Workable {
  work() { /* 工作 */ }
  // 不需要实现不相关的方法
}
```

##### C++示例

```cpp
// Bad Case
class Printer {
public:
    virtual void print() = 0;
    virtual void scan() = 0;
    virtual void fax() = 0;
    virtual void copy() = 0;
    virtual ~Printer() {}
};

class SimplePrinter : public Printer {
public:
    void print() override { /* 打印 */ }
    void scan() override { throw std::logic_error("Can't scan"); }  // 被迫实现
    void fax() override { throw std::logic_error("Can't fax"); }    // 被迫实现
    void copy() override { throw std::logic_error("Can't copy"); }  // 被迫实现
};

// Good Case
class IPrinter {
public:
    virtual void print() = 0;
    virtual ~IPrinter() {}
};

class IScanner {
public:
    virtual void scan() = 0;
    virtual ~IScanner() {}
};

class IFax {
public:
    virtual void fax() = 0;
    virtual ~IFax() {}
};

class ICopier {
public:
    virtual void copy() = 0;
    virtual ~ICopier() {}
};

class SimplePrinter : public IPrinter {
public:
    void print() override { /* 打印 */ }
};

class AllInOnePrinter : public IPrinter, public IScanner, public IFax, public ICopier {
public:
    void print() override { /* 打印 */ }
    void scan() override { /* 扫描 */ }
    void fax() override { /* 传真 */ }
    void copy() override { /* 复印 */ }
};
```

#### 5. 依赖倒置原则 (Dependency Inversion Principle, DIP)

**核心**：高层模块不应依赖低层模块，二者都应依赖于抽象

##### TypeScript示例

```typescript
// Bad Case: 高层模块依赖低层模块
class MySQLDatabase {
  connect(): void { /* 连接MySQL */ }
  query(sql: string): any { /* 查询MySQL */ return {}; }
}

class UserService {
  private database: MySQLDatabase;

  constructor() {
    this.database = new MySQLDatabase();  // 直接依赖具体实现
  }

  getUser(id: number): any {
    this.database.connect();
    return this.database.query(`SELECT * FROM users WHERE id = ${id}`);
  }
}

// Good Case: 依赖抽象
interface Database {
  connect(): void;
  query(sql: string): any;
}

class MySQLDatabase implements Database {
  connect(): void { /* 连接MySQL */ }
  query(sql: string): any { /* 查询MySQL */ return {}; }
}

class PostgreSQLDatabase implements Database {
  connect(): void { /* 连接PostgreSQL */ }
  query(sql: string): any { /* 查询PostgreSQL */ return {}; }
}

class UserService {
  private database: Database;
  
  constructor(database: Database) {
    this.database = database;  // 依赖注入，依赖抽象
  }
  
  getUser(id: number): any {
    this.database.connect();
    return this.database.query(`SELECT * FROM users WHERE id = ${id}`);
  }
}
```

##### C++示例

```cpp
// Bad Case
class LightBulb {
public:
    void turnOn() {
        // 打开灯泡
    }
    
    void turnOff() {
        // 关闭灯泡
    }
};

class Switch {
private:
    LightBulb bulb;  // 直接依赖具体实现
    bool state;
    
public:
    void toggle() {
        if (state) {
            bulb.turnOff();
            state = false;
        } else {
            bulb.turnOn();
            state = true;
        }
    }
};

// Good Case
class ISwitchable {
public:
    virtual void turnOn() = 0;
    virtual void turnOff() = 0;
    virtual ~ISwitchable() {}
};

class LightBulb : public ISwitchable {
public:
    void turnOn() override {
        // 打开灯泡
    }
    
    void turnOff() override {
        // 关闭灯泡
    }
};

class Fan : public ISwitchable {
public:
    void turnOn() override {
        // 打开风扇
    }
    
    void turnOff() override {
        // 关闭风扇
    }
};

class Switch {
private:
    ISwitchable& device;  // 依赖抽象
    bool state;
    
public:
    Switch(ISwitchable& switchable) : device(switchable), state(false) {}
    
    void toggle() {
        if (state) {
            device.turnOff();
            state = false;
        } else {
            device.turnOn();
            state = true;
        }
    }
};
```

### 组合优于继承原则

**核心**：优先使用对象组合，而不是类继承

##### TypeScript示例

```typescript
// Bad Case: 使用继承
class Animal {
  eat(): void { console.log("Eating..."); }
  sleep(): void { console.log("Sleeping..."); }
}

class Bird extends Animal {
  fly(): void { console.log("Flying..."); }
}

class Fish extends Animal {
  swim(): void { console.log("Swimming..."); }
}

// Good Case: 使用组合
class Eater {
  eat(): void { console.log("Eating..."); }
}

class Sleeper {
  sleep(): void { console.log("Sleeping..."); }
}

class Flyer {
  fly(): void { console.log("Flying..."); }
}

class Swimmer {
  swim(): void { console.log("Swimming..."); }
}

class Bird {
  private eater = new Eater();
  private sleeper = new Sleeper();
  private flyer = new Flyer();
  
  eat(): void { this.eater.eat(); }
  sleep(): void { this.sleeper.sleep(); }
  fly(): void { this.flyer.fly(); }
}

class Fish {
  private eater = new Eater();
  private sleeper = new Sleeper();
  private swimmer = new Swimmer();
  
  eat(): void { this.eater.eat(); }
  sleep(): void { this.sleeper.sleep(); }
  swim(): void { this.swimmer.swim(); }
}
```

##### C++示例

```cpp
// Bad Case: 使用继承
class Logger {
public:
    void log(const std::string& message) {
        std::cout << message << std::endl;
    }
};

class FileProcessor : public Logger {  // 继承Logger
public:
    void processFile(const std::string& filename) {
        log("Processing file: " + filename);
        // 处理文件...
    }
};

// Good Case: 使用组合
class Logger {
public:
    void log(const std::string& message) {
        std::cout << message << std::endl;
    }
};

class FileProcessor {
private:
    Logger logger;  // 组合Logger

public:
    void processFile(const std::string& filename) {
        logger.log("Processing file: " + filename);
        // 处理文件...
    }
};
```

### 总结

面向对象设计原则帮助我们创建更灵活、可维护的代码：

- **单一职责原则**：每个类只有一个变化的原因
- **开放封闭原则**：通过扩展而非修改来适应变化
- **里氏替换原则**：子类可替换父类，保持行为一致性
- **接口隔离原则**：接口应小而专一，避免强制实现不需要的方法
- **依赖倒置原则**：依赖抽象而非具体实现
- **组合优于继承**：通过组合实现代码复用，降低耦合度

遵循这些原则，我们能够构建出更能抵御变化、更易于维护和扩展的软件系统。

## 设计模式

### 模版方法

```ts
// 定义钩子函数类型
type HookFunction = (data?: any) => any;

// 模板方法基类 - 使用泛型和钩子函数增强灵活性
abstract class ProcessTemplate<TContext, TResult> {
  protected context: TContext;
  private hooks: Map<string, HookFunction> = new Map();

  constructor(initialContext: TContext) {
    this.context = initialContext;
  }

  // 模板方法 - 定义算法骨架
  public execute(): TResult {
    this.beforeProcess();

    // 初始化
    this.initialize();
    this.runHook("afterInitialize");

    // 验证
    if (!this.validate()) {
      return this.handleInvalidState();
    }
    this.runHook("afterValidation");

    // 处理主要业务逻辑
    this.preProcess();
    this.runHook("beforeMainProcess");

    const processResult = this.process();
    this.runHook("afterMainProcess", processResult);

    // 后处理
    this.postProcess(processResult);

    // 完成
    const result = this.finalize(processResult);

    return result;
  }

  // 注册钩子函数 - 允许在不修改类的情况下扩展行为
  public registerHook(hookName: string, callback: HookFunction): this {
    this.hooks.set(hookName, callback);
    return this;
  }

  // 运行钩子函数
  protected runHook(hookName: string, data?: any): any {
    const hook = this.hooks.get(hookName);
    return hook ? hook(data) : undefined;
  }

  // 生命周期方法 - 可被子类覆盖
  protected beforeProcess(): void {}
  protected afterProcess(result: TResult): void {}

  // 核心步骤 - 必须由子类实现
  protected abstract initialize(): void;
  protected abstract validate(): boolean;
  protected abstract process(): any;
  protected abstract finalize(processResult: any): TResult;

  // 可选步骤 - 有默认实现，子类可覆盖
  protected preProcess(): void {}
  protected postProcess(processResult: any): void {}
  protected handleInvalidState(): TResult {
    throw new Error("处理无效状态");
  }
}

// 示例：文档处理系统
interface DocumentContext {
  content: string;
  metadata: Record<string, any>;
  processingOptions: Record<string, any>;
  errors: string[];
}

interface ProcessedDocument {
  id: string;
  content: string;
  metadata: Record<string, any>;
  processedAt: Date;
  status: "success" | "error" | "warning";
  warnings: string[];
  errors: string[];
}

// 定义处理结果的接口
interface ProcessResult {
  content: string;
  metadata: Record<string, any>;
  warnings?: string[];
}

// 抽象文档处理器
abstract class DocumentProcessor extends ProcessTemplate<
  DocumentContext,
  ProcessedDocument
> {
  protected initialize(): void {
    console.log("初始化文档处理...");
    if (!this.context.metadata) {
      this.context.metadata = {};
    }
    if (!this.context.errors) {
      this.context.errors = [];
    }

    // 添加处理时间戳
    this.context.metadata.processingStarted = new Date();
  }

  protected validate(): boolean {
    console.log("验证文档...");
    if (!this.context.content || this.context.content.trim() === "") {
      this.context.errors.push("文档内容不能为空");
      return false;
    }
    return true;
  }

  protected preProcess(): void {
    console.log("预处理文档...");
    // 清理内容中的多余空白
    this.context.content = this.context.content.trim().replace(/\s+/g, " ");
  }

  protected finalize(processResult: ProcessResult): ProcessedDocument {
    console.log("完成文档处理...");
    return {
      id: this.generateDocumentId(),
      content: processResult.content || this.context.content,
      metadata: { ...this.context.metadata, ...processResult.metadata },
      processedAt: new Date(),
      status:
        this.context.errors.length > 0
          ? "error"
          : processResult.warnings && processResult.warnings.length > 0
          ? "warning"
          : "success",
      warnings: processResult.warnings || [],
      errors: this.context.errors,
    };
  }

  private generateDocumentId(): string {
    return `doc-${Date.now()}-${Math.floor(Math.random() * 1000)}`;
  }

  // 子类必须实现的核心处理逻辑
  protected abstract process(): ProcessResult;
}

// 具体实现：Markdown文档处理器
class MarkdownProcessor extends DocumentProcessor {
  protected process(): ProcessResult {
    console.log("处理Markdown文档...");
    const content = this.context.content;

    // 提取标题
    const titleMatch = content.match(/^#\s+(.+)$/m);
    const title = titleMatch ? titleMatch[1] : "Untitled Document";

    // 计算阅读时间
    const wordCount = content.split(/\s+/).length;
    const readingTimeMinutes = Math.ceil(wordCount / 200); // 假设平均阅读速度为每分钟200字

    // 检查是否有图片
    const hasImages = content.includes("![");

    // 提取所有链接
    const linkRegex = /\[([^\]]+)\]\(([^)]+)\)/g;
    const links: string[] = [];
    let match;
    while ((match = linkRegex.exec(content)) !== null) {
      links.push(match[2]);
    }

    // 检查潜在问题
    const warnings: string[] = [];
    if (
      links.length > 0 &&
      !this.context.processingOptions.skipLinkValidation
    ) {
      warnings.push("文档包含外部链接，请确保它们是有效的");
    }
    if (!titleMatch) {
      warnings.push("文档没有主标题");
    }

    return {
      content: content,
      metadata: {
        title,
        wordCount,
        readingTimeMinutes,
        hasImages,
        links,
      },
      warnings,
    };
  }

  // 覆盖预处理步骤，添加Markdown特定的处理
  protected preProcess(): void {
    super.preProcess();
    console.log("Markdown预处理...");

    // 标准化换行符
    this.context.content = this.context.content.replace(/\r\n/g, "\n");

    // 确保代码块有正确的格式
    this.context.content = this.context.content.replace(
      /```(\w*)\n([\s\S]*?)```/g,
      (match, lang, code) => `\`\`\`${lang}\n${code.trim()}\n\`\`\``
    );
  }

  // 添加后处理步骤
  protected postProcess(processResult: ProcessResult): void {
    console.log("Markdown后处理...");

    // 添加处理器签名
    processResult.content += `\n\n---\n*Processed by MarkdownProcessor at ${new Date().toISOString()}*`;

    // 添加额外元数据
    processResult.metadata.processor = "MarkdownProcessor";
    processResult.metadata.version = "1.0.0";
  }
}

// 具体实现：HTML文档处理器
class HTMLProcessor extends DocumentProcessor {
  protected process(): ProcessResult {
    console.log("处理HTML文档...");
    let content = this.context.content;

    // 提取标题
    const titleMatch = content.match(/<title>([^<]+)<\/title>/i);
    const title = titleMatch ? titleMatch[1] : "Untitled Document";

    // 提取所有图片
    const imgRegex = /<img[^>]+src="([^"]+)"[^>]*>/g;
    const images: string[] = [];
    let imgMatch;
    while ((imgMatch = imgRegex.exec(content)) !== null) {
      images.push(imgMatch[1]);
    }

    // 检查是否有表单
    const hasForms = content.includes("<form");

    // 检查是否有JavaScript
    const hasScripts = content.includes("<script");

    // 检查潜在问题
    const warnings: string[] = [];
    if (hasScripts && !this.context.processingOptions.allowScripts) {
      warnings.push("文档包含JavaScript，这可能存在安全风险");
      // 如果不允许脚本，则移除它们
      content = content.replace(
        /<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi,
        ""
      );
    }

    if (hasForms && !this.context.processingOptions.allowForms) {
      warnings.push("文档包含表单，请确保它们是安全的");
    }

    return {
      content,
      metadata: {
        title,
        images,
        hasForms,
        hasScripts,
      },
      warnings,
    };
  }

  // HTML特定的验证
  protected validate(): boolean {
    if (!super.validate()) return false;

    console.log("HTML特定验证...");

    // 检查HTML是否有基本结构
    if (
      !this.context.content.includes("<html") ||
      !this.context.content.includes("</html>")
    ) {
      this.context.errors.push("HTML文档缺少基本的<html>标签");
      return false;
    }

    return true;
  }

  // 添加HTML特定的后处理
  protected postProcess(processResult: ProcessResult): void {
    console.log("HTML后处理...");

    // 添加处理器签名作为HTML注释
    processResult.content = processResult.content.replace(
      "</body>",
      `  <!-- Processed by HTMLProcessor at ${new Date().toISOString()} -->\n</body>`
    );

    // 如果配置了自动添加响应式元标签
    if (this.context.processingOptions.addResponsiveMeta) {
      const viewportMeta =
        '<meta name="viewport" content="width=device-width, initial-scale=1.0">';
      if (!processResult.content.includes('name="viewport"')) {
        processResult.content = processResult.content.replace(
          "<head>",
          `<head>\n  ${viewportMeta}`
        );
      }
    }
  }
}

// 使用示例
function demonstrateTemplateMethod(): void {
  console.log("===== 模板方法模式超强版演示 =====\n");

  // 处理Markdown文档
  const markdownContext: DocumentContext = {
    content: `# 示例Markdown文档

这是一个示例Markdown文档，用于演示模板方法模式。

## 特性
- 简单易用
- 高度可扩展
- 支持钩子函数

[查看更多信息](https://example.com)

![示例图片](https://example.com/image.jpg)
`,
    metadata: {
      author: "设计模式爱好者",
      createdAt: new Date(),
    },
    processingOptions: {
      skipLinkValidation: false,
    },
    errors: [],
  };

  const markdownProcessor = new MarkdownProcessor(markdownContext)
    .registerHook("afterInitialize", () => console.log("钩子: 初始化后"))
    .registerHook("afterValidation", () => console.log("钩子: 验证后"))
    .registerHook("beforeMainProcess", () => console.log("钩子: 主处理前"))
    .registerHook("afterMainProcess", (data: ProcessResult) => {
      console.log("钩子: 主处理后，发现链接数:", data.metadata.links.length);
      // 可以在这里修改处理结果
      data.metadata.modifiedByHook = true;
      return data;
    });

  console.log("\n--- 处理Markdown文档 ---");
  const markdownResult = markdownProcessor.execute();
  console.log("\nMarkdown处理结果:", JSON.stringify(markdownResult, null, 2));

  // 处理HTML文档
  const htmlContext: DocumentContext = {
    content: `<!DOCTYPE html>
<html>
<head>
  <title>示例HTML文档</title>
</head>
<body>
  <h1>示例HTML文档</h1>
  <p>这是一个用于演示模板方法模式的HTML文档。</p>

  <img src="example.jpg" alt="示例图片">

  <script>
    alert('Hello, world!');
  </script>

  <form action="/submit" method="post">
    <input type="text" name="name">
    <button type="submit">提交</button>
  </form>
</body>
</html>`,
    metadata: {
      author: "设计模式爱好者",
      createdAt: new Date(),
    },
    processingOptions: {
      allowScripts: false,
      allowForms: true,
      addResponsiveMeta: true,
    },
    errors: [],
  };

  const htmlProcessor = new HTMLProcessor(htmlContext);

  console.log("\n--- 处理HTML文档 ---");
  const htmlResult = htmlProcessor.execute();
  console.log("\nHTML处理结果:", JSON.stringify(htmlResult, null, 2));
}

// 执行演示
demonstrateTemplateMethod();
```


#### 策略模式

### 观察者模式

### 装饰模式

### 桥模式

### 工厂方法

### 抽象工厂

### 构建器

### 单元模式

### 亨元模式

### 门面模式

### 代理模式

### 适配器

### 中介者

### 状态模式
