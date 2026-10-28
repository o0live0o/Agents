# C# 编码规范

## 优先级

- 编辑已有文件时，跟随该文件的既有风格；与本规范冲突时以文件为准
- 例外：任务本身就是风格重构时，以本规范为目标风格，而不是跟随旧风格
- 文件无既有风格时使用以下规则；全新组件用广泛接受的通用实践
- 风格与命名统一改动一律独立提交，不夹带功能改动

## 格式

- Allman 大括号：左大括号另起一行
- 所有语句块都加大括号，单行 `if` / `for` / `foreach` 也要加
- 4 空格缩进，禁止 Tab
- 每行只写一条语句、只做一个声明
- 方法与属性定义之间一个空行；其余位置不出现连续空行，行内不留多余空格
- 命名空间用文件作用域声明：`namespace Foo;`
- `using` 指令（using directive）放在 `namespace` 声明之外，按字母排序，`System.*` 置顶

```csharp
if (condition)
{
    DoSomething();
}
```

## 命名

- 私有字段用 `lowerCamelCase`，不加 `_` 前缀
- 能加 `readonly` 就加；构造之后仍需赋值时保持普通字段，不要为此改成属性
- 常量字段与常量局部变量用 `PascalCasing`；interop 常量须与被调用代码的名称和值完全一致

## 类型与成员

- 静态成员通过类名调用：`ClassName.StaticMember`，不要用派生类名限定
- 参数或局部变量与字段同名时，用 `this.` 限定字段
- 类型引用和方法调用都用语言关键字，不用 BCL 类型名：`int` / `string` / `float`，`int.Parse` 而非 `Int32.Parse`
- `var` 只在右侧类型显而易见时使用，典型是字面量、`new`、显式转换；`foreach` 的元素类型不算显而易见
- LINQ 查询变量与结果序列用 `var`

```csharp
// 可以：new 让类型显而易见
var stream = new FileStream(path, FileMode.Open);

// 不行：返回类型不明显
var stream = OpenStandardInput();

// 正确：用语言关键字
int count = int.Parse(text);

// 禁止：使用 BCL 类型名
Int32 count = Int32.Parse(text);
```

## 语法与语言特性

- 优先使用当前 C# 版本的新语法，避免过时的语言构造
- 集合用集合表达式初始化：`string[] vowels = ["a", "e", "i", "o", "u"];`
- 变量类型与创建类型一致时用目标类型 `new()`：`ExampleClass item = new();`
- 对象创建用对象初始值设定项，不要创建后逐行给属性赋值
- 必须赋值的属性用 `required` 修饰，不靠构造函数强制
- 内部回调和一次性函数用 `Func<>` / `Action<>`，不新定义委托类型
- 公开 API、事件，或函数本身有业务语义时，用命名委托并加 XML 注释
- 后续不需要移除的事件处理用 lambda 定义
- 释放资源用 `using` 声明（using declaration）或 `using` 语句，不手写 try-finally 调 `Dispose`
- 主构造函数参数：record 用 PascalCasing，class / struct 用 camelCase
- 比较条件用 `&&` / `||`，不用 `&` / `|`，以利用短路求值；位运算仍用 `&` / `|`

## 异常处理

- 只捕获能正确处理的异常，禁止无筛选器地捕获 `System.Exception`
- 用具体的异常类型，并提供有意义的错误消息

## 异步

- I/O 密集操作用 `async` / `await`，不要同步阻塞
- 有同步上下文时（UI、旧 ASP.NET）对库代码用 `ConfigureAwait(false)`，ASP.NET Core 与控制台应用不需要

## 字符串

- 短字符串拼接用内插，且用基于表达式的形式：`$"{student.Last} Score: {student.score}"`
- 循环中大量追加用 `StringBuilder`
- 含转义字符或跨多行时优先原始字符串字面量 `"""..."""`

## 注释

- 简要说明用单行 `//`，长篇解释不要用 `/* */`
- 注释独占一行，不写在代码行末尾
- 注释以大写字母开头、句号结尾，`//` 与正文之间留一个空格
- 公共类、方法、字段用 XML 注释

## 编辑既有代码

- 只改任务涉及的代码，不顺手整理相邻代码的格式；风格重构任务除外
- 风格重构时按本规范统一目标范围，行为保持不变，并与功能改动分开提交
- 有外部契约的名字不改：序列化、`DataContract`、反射、ORM 映射、数据绑定、P/Invoke 结构体、已发布公共 API
- 目标文件既有字段都带 `_` 且只是小改时，新字段跟随 `_lowerCamelCase`，保持文件内一致
