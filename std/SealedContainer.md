# A Container To make values not changable at runtime

## Example Usage

```
let val: Sealed<int> = 12; //implicit cast operator overloaded

val.Set(42); //this causes an exception

val.Unfreeze();
val.Set(42); //this changes the value correctly

```

## Definition

```
struct Sealed<T> {
    public func Set(val: T) -> none;
    public func Freeze() -> none;
    public func Unfreeze() -> none;
    
    public operator implicit(val: T) -> Sealed<T>;

    public prop IsFreezed() -> bool;
}
```