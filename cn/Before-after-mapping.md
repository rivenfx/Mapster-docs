# 映射前/映射后

### 

### 在映射之前执行操作

可以使用 `BeforeMapping`方法在映射开始之前执行操作。

```csharp
TypeAdapterConfig<Foo, Bar>.ForType()
    .BeforeMapping((src, dest) => dest.Initialize());
```



### 在映射之后执行操作

可以使用 `AfterMapping`方法在每个映射之后执行操作。

例如，在每次映射之后验证对象：

```csharp
TypeAdapterConfig<Foo, Bar>.ForType()
    .AfterMapping((src, dest) => dest.Validate());
```

或者可以使用 `ForDestinationType` 方法设置所有映射到实现特定接口的类型：

```csharp
TypeAdapterConfig.GlobalSettings.ForDestinationType<IValidatable>()
    .AfterMapping(dest => dest.Validate());
```


### 在 映射前/映射后 使用 Expression 生成代码

`BeforeMapping`和 `AfterMapping `接受 `Action` 参数， `Action` 中可以有多行语句但不能动态生成代码。

而 `BeforeMappingInline` 和 `AfterMappingInline` 接受 `Expression` 作为参数，  `Expression` 可以动态编译为代码，实现懒加载等等。

因此在合适的场景下可以使用  `BeforeMappingInline` 和 `AfterMappingInline` 。

#### 单行语句

对于单行语句，你可以直接从 `BeforeMapping`和 `AfterMapping` 改为 `BeforeMappingInline`和 `AfterMappingInline`：

```csharp
TypeAdapterConfig.GlobalSettings.ForDestinationType<IValidatable>()
    .AfterMappingInline(dest => dest.Validate());
```

#### 多行语句

对于多行语句，你需要定义一个单独的方法：

```csharp
public static void Validate(Dto dto) {
    action1(dto);
    action2(dto);
    ...
}
```

然后在 `BeforeMappingInline` 或 `AfterMappingInline` 方法中调用：

```csharp
TypeAdapterConfig.GlobalSettings.ForDestinationType<IValidatable>()
    .AfterMappingInline(dest => PocoToDtoMapper.Validate(dest));
```