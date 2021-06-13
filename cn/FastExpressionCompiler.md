# FastExpressionCompiler

需要更高的性能？使用  [FastExpressionCompiler](https://github.com/dadhi/FastExpressionCompiler) 作为表达式树编译器

安装 nuget 包

     PM> Install-Package FastExpressionCompiler

在配置实例上指定表达式树编译器：

```csharp
TypeAdapterConfig.GlobalSettings.Compiler = exp => exp.CompileFast();
```

替换表达式树编译器后，性能对比结果：

| Method                |      Mean |   StdDev |    Error |      Gen 0 | Gen 1 | Gen 2 | Allocated |
| --------------------- | --------: | -------: | -------: | ---------: | ----: | ----: | --------: |
| 'Mapster 4.1.1'       | 115.31 ms | 0.849 ms | 1.426 ms | 31000.0000 |     - |     - | 124.36 MB |
| 'Mapster 4.1.1 (FEC)' |  54.70 ms | 1.023 ms | 1.546 ms | 29600.0000 |     - |     - | 118.26 MB |