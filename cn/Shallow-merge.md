# 拷贝与合并

### 深拷贝 和 浅拷贝 映射

Mapster 默认递归映射对象是深拷贝，如果不想使用深拷贝，可以通过调用 `ShallowCopyForSameType` 方法设置为浅拷贝：

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .ShallowCopyForSameType(true);
```
### 拷贝映射和合并映射

Mapster 默认将映射所有成员，甚至包含空值的源成员到目标成员。

使用 `IgnoreNullValues` 方法可以实现只映射不为空的成员：

```csharp
TypeAdapterConfig<TSource, TDestination>
    .NewConfig()
    .IgnoreNullValues(true);
```