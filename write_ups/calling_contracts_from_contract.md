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
// adressOfA - is Address of contract A
// tip: raw address string "cx..." can be converted into Address object using: Address.fromString("cx....")
BigInteger fiveFromContractA = Context.call(adressOfA,"getFive")
```

That's it! If you need to send parameters with the call, or send ICX as value you can do this as well.

Elevating Context calls to Public Abstract Class static methods with casted types is a good practice. This effectively makes the code more readable, introduces strong typing and serves as a syntatic sugar shortening the contract code.

### Example

Declare public abstract class for beforementioned contract A:

```java
public abstract class IA {
  public static BigInteger getFive(Address adressOfA) {
    return (BigInteger) Context.call(adressOfA, "getFive");
  }
}
```

On contract B you use the static function as following:

```java
// import abstract class
import IA;

// adressOfA - is Address of contract A
BigInteger fiveFromContractA = IA.getFive(adressOfA);
```

You can read more on how to use this in the [ICON java docs](https://javadoc.io/doc/foundation.icon/javaee-api/latest/score/Context.html).
