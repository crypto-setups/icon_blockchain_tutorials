# One way to have key-value mappings 

Coming from Soldity I struggled a bit to find out how I could use key-value mappings, or a hashmap, I guess. Sometimes the contract would compile and optimizing 
the JAR would work just fine. 

But when trying to use it in a transaction the blockchain would throw an unknown error. The transaction status would be something like:
`Fail - UnknownFailure`

When checking the stack trace something like this, would show:

`FRAME[2] org.aion.avm.core.DAppExecutor i.UncaughtException: java.lang.NullPointerException at 
org.aion.avm.core.persistence.LoadedDApp.handleUncaughtException(LoadedDApp.java:360) at 
org.aion.avm.core.persistence.LoadedDApp.callMethod(LoadedDApp.java:314) at 
org.aion.avm.core.DAppExecutor.call(DAppExecutor.java:85) at 
foundation.icon.ee.score.AvmExecutor.runCommon(AvmExecutor.java:115) at 
foundation.icon.ee.score.AvmExecutor.runExternal(AvmExecutor.java:74) at 
foundation.icon.ee.score.AvmExecutor.run(AvmExecutor.java:59) at 
foundation.icon.ee.score.TransactionExecutor.handleInvoke(TransactionExecutor.java:146) at 
foundation.icon.ee.ipc.EEProxy.handleInvoke(EEProxy.java:316) at 
foundation.icon.ee.ipc.EEProxy.doHandleMessages(EEProxy.java:126) at 
foundation.icon.ee.ipc.EEProxy.handleMessages(EEProxy.java:112) at 
foundation.icon.ee.score.TransactionExecutor.connectAndRunLoop(TransactionExecutor.java:98) at 
foundation.icon.ee.score.TransactionExecutor.connectAndRunLoop(TransactionExecutor.java:90) at 
foundation.icon.ee.ipc.ExecutorManager.lambda$runExecutor$0(ExecutorManager.java:73) at 
java.base/java.lang.Thread.run(Thread.java:829) Caused by: java.lang.NullPointerException at u.C.avm_gt(Unknown Source) at 
java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method) at 
java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) at 
java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) at 
java.base/java.lang.reflect.Method.invoke(Method.java:566) at 
org.aion.avm.core.persistence.LoadedDApp.callMethod(LoadedDApp.java:304) ... 12 more`

No bueno!

# Possible Solution

The way I solved this is by using the VarDb as followed:

```java
import score.VarDB;
```


Make a function that returns a VarDB. A VarDB will have a getter and setter build in (among other things). 


```java

// map id to address
private final VarDB<Address> mapIdToAddress (BigInteger _id) {
  // create a custom mapping by using the _id param
  return Context.newVarDB("idMap" + _id.toString(),Address.class);
}

```

Keep in mind that this is a function that returns a VarDB. If you want to create a key-value pair now you can do as followed:

```java
mapIdToAddress(BigInteger.valueOf(3), Context.getCaller());
```

Now you have mapped 3 -> the address of the caller.

You can now for example create a external function that returns the address of each id number, like so:

```java
@External(readonly=true)
public Address getAddressOfId(BigInteger _id) {
  return mapIdToAddress(_id).get();
}  
```

I believe that if you use the getter of a VarDB to get a "unknown" key, the getter will return null. 
This will compile and deploy, but will give a error similiar as above when trying to execute in a transaction!

You can handle this by using `getOrDefault()`, so for example:

```java
@External(readonly=true)
public Address getAddressOfId(BigInteger _id) {
  return mapIdToAddress(_id).getOrDefault(some address);
}  
```

If the Id you tried to get is unknown (not yet 'set') it will return the default value that is set as `some address`.


That's it! A clean way to use mappings or a hashmap on the ICON Blockchain!

Feel free to comment or ask questions!

* special thanks to one of the Andies, for showing me this solution a while back!
