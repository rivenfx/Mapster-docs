# 基于规则的映射

Mapster 在默认情况下映射会包含所有的公开字段和属性，但是可以通过 `IncludMember` 和 `IgnoreMember` 方法改变默认的映射行为。

这两个方法需要传入指令参数，指令参数为以下类型：

```csharp
public interface IMemberModel
{
    Type Type { get; }
    string Name { get; }
    object Info { get; }
    AccessModifier SetterModifier { get; }
    AccessModifier AccessModifier { get; }
    IEnumerable<object> GetCustomAttributes(bool inherit);
}

public enum MemberSide
{
    Source,
    Destination,
}
```

### 不映射字段

可以通过检查成员的类型来决定是否映射，成员的类型分为  `PropertyInfo`, `FieldInfo`, `ParameterInfo` 几种。

下面这个例子实现了 如果成员类型为 `FieldInfo` 忽略映射的效果：

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreMember((member, side) => member.Info is FieldInfo);
```

### 只允许映射指定的类型

**通过类型过滤**

只映射存在 `validTypes` 集合中的类型的成员

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreMember((member, side) => !validTypes.Contains(member.Type));
```
**通过命名空间过滤**

只映射类型命名空间以 `System` 开头的成员：

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreMember((member, side) => !member.Type.Namespace.StartsWith("System"));
```

### 映射非公开成员

如果想要映射非公开成员，可以查看下面的例子：

```csharp
    TypeAdapterConfig.GlobalSettings.Default
        .IncludeMember((member, side) => member.AccessModifier == AccessModifier.Internal 
                                         || member.AccessModifier == AccessModifier.ProtectedInternal);
```

### 只映射拥有 [DataMember] 特性标记的成员

如果想要只映射拥有 `[DataMember]` 特性标记的成员，可以查看下面的例子一样：

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IncludeMember((member, side) => member.GetCustomAttributes(true).OfType<DataMemberAttribute>().Any());
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreMember((member, side) => !member.GetCustomAttributes(true).OfType<DataMemberAttribute>().Any());
```

### 只映射公开set的属性

Mapster 默认会映射 `set` 不公开的属性，可以使用以下代码关闭此功能：

```csharp
TypeAdapterConfig.GlobalSettings.Default
    .IgnoreMember((member, side) => side == MemberSide.Destination
                                    && member.SetterModifier != AccessModifier.Public);
```