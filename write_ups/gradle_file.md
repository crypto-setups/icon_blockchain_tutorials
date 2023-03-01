# build.gradle - dependencies

When you get a `Fail - IllegalFormat` or `E0005:Illegal format` error when trying to deploy your contract be sure to check 
the `dependencies{ }` block in your build.gradle file.

> Check if you are using `implementation` and `compileOnly` settings correct. 

> I believe you can go wrong here if you use `implementation` where you should
be using `compileOnly`.

### implementation:
This configuration is used when you want the dependency to be available both at compile time and runtime. 
That means the dependency will be added to the classpath of your application at both times, and can be used by your application code. 
It also means that the transitive dependencies of this dependency will also be included in your application.

### compileOnly:
This configuration is used when you only need the dependency at compile time. 
This means that the dependency will be added to the classpath during compilation, but it will not be included in the final artifact. 
It also means that the transitive dependencies of this dependency will not be included in your application.

> In summary, if you want a dependency to be included in your application at runtime, use implementation. If you only need it at compile time, use compileOnly.
