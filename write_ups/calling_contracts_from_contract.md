# Calling a contract, from a contract

Using ICON Smart Contracts, it is easy to call methods on contracts from a contract.

### Example:

On contract A there is a method that returns the number 5:

```java
@External
public BigInteger getFive() {
	return BigInteger.valueOf(5);
}
```

On contract B you call that function as following:

```java
// in this example we store the response in a variable

BigInteger fiveFromContractA = Context.call(BigInteger.class, Address.fromString("cx...."),"getFive")
```

That's it! If you need to send parameters with the call, or send ICX as value you can do this as well.

You can read more on how to use this in the [ICON java docs](https://javadoc.io/doc/foundation.icon/javaee-api/latest/score/Context.html).
