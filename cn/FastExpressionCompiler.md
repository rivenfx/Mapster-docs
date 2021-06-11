# FastExpressionCompiler

Need more speed? Let's compile with [FastExpressionCompiler](https://github.com/dadhi/FastExpressionCompiler).

Getting the package

     PM> Install-Package FastExpressionCompiler

Then add following code on start up

```csharp
TypeAdapterConfig.GlobalSettings.Compiler = exp => exp.CompileFast();
```

That's it. Now your code will enjoy performance boost. Here is result.

| Method                |      Mean |   StdDev |    Error |      Gen 0 | Gen 1 | Gen 2 | Allocated |
| --------------------- | --------: | -------: | -------: | ---------: | ----: | ----: | --------: |
| 'Mapster 4.1.1'       | 115.31 ms | 0.849 ms | 1.426 ms | 31000.0000 |     - |     - | 124.36 MB |
| 'Mapster 4.1.1 (FEC)' |  54.70 ms | 1.023 ms | 1.546 ms | 29600.0000 |     - |     - | 118.26 MB |