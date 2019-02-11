### Step-into debugging

    PM> Install-Package ExpressionDebugger

This plugin allow you to perform step-into debugging!

##### Usage

Then add following code on start up (or anywhere before mapping is compiled)

    var opt = new ExpressionCompilationOptions { IsRelease = !Debugger.IsAttached };
    TypeAdapterConfig.GlobalSettings.Compiler = exp => exp.CompileWithDebugInfo(opt);

Now on your mapping code (only in `DEBUG` mode).

    var dto = poco.Adapt<SimplePoco, SimpleDto>(); <--- you can step-into this function!!

![image](https://cloud.githubusercontent.com/assets/5763993/26521773/180427b6-431b-11e7-9188-10c01fa5ba5c.png)

##### Using internal classes or members

`private`, `protected` and `internal` don't allow in debug mode.

### Get mapping script

We can also see how Mapster generate mapping logic with `ToScript` method.

```
var script = poco.BuildAdapter()
                .CreateMapExpression<SimpleDto>()
                .ToScript();
```

### Visual Studio for Mac
To step-into debugging, you might need to emit file
```CSharp
var opt = new ExpressionCompilationOptions { IsRelease = !Debugger.IsAttached, EmitFile = true };
TypeAdapterConfig.GlobalSettings.Compiler = exp => exp.CompileWithDebugInfo(opt);
...
var dto = poco.Adapt<SimplePoco, SimpleDto>(); //<-- you can step-into this function!!
```
