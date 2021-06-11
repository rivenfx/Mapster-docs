# Debugging

### Step-into debugging

    PM> Install-Package ExpressionDebugger

This plugin allows you to perform step-into debugging using Roslyn!

##### Usage

Then add following code on start up (or anywhere before mapping is compiled)

```csharp
TypeAdapterConfig.GlobalSettings.Compiler = exp => exp.CompileWithDebugInfo();
```

Now in your mapping code (only in `DEBUG` mode).

```csharp
var dto = poco.Adapt<SimplePoco, SimpleDto>(); //<--- you can step-into this function!!
```

![image](https://cloud.githubusercontent.com/assets/5763993/26521773/180427b6-431b-11e7-9188-10c01fa5ba5c.png)

##### Using internal classes or members

`private`, `protected` and `internal` aren't allowed in debug mode.

### Get mapping script

We can also see how Mapster generates mapping logic with `ToScript` method.

```
var script = poco.BuildAdapter()
                .CreateMapExpression<SimpleDto>()
                .ToScript();
```

### Visual Studio for Mac
To step-into debugging, you might need to emit file
```csharp
var opt = new ExpressionCompilationOptions { EmitFile = true };
TypeAdapterConfig.GlobalSettings.Compiler = exp => exp.CompileWithDebugInfo(opt);
...
var dto = poco.Adapt<SimplePoco, SimpleDto>(); //<-- you can step-into this function!!
```

### Do not worry about performance
In `RELEASE` mode, Roslyn compiler is actually faster than default dynamic compilation by 2x. 
Here is the result:

| Method                   |      Mean |   StdDev |    Error |      Gen 0 | Gen 1 | Gen 2 | Allocated |
| ------------------------ | --------: | -------: | -------: | ---------: | ----: | ----: | --------: |
| 'Mapster 4.1.1'          | 115.31 ms | 0.849 ms | 1.426 ms | 31000.0000 |     - |     - | 124.36 MB |
| 'Mapster 4.1.1 (Roslyn)' |  53.55 ms | 0.342 ms | 0.654 ms | 31100.0000 |     - |     - | 124.36 MB |