# Read-Write-Lock Container

easy concurrency handle when using a variable in parallel.

## Example Usage

Basic Read Write:
```
let val: RwLock<int> = 12; // implicit cast operator overloaded

{
  let read = val.Read(); // this returns a reference of inner RwLock
} // read gets dropped here

val.Write(42); // this changes the value correctly
```

Async Read:
```
let val: RwLock<int> = 12; // implicit cast operator overloaded

Helper::RunAsync(() -> {
  let hold = val.Read(); 
  Helper::Sleep(10, Duration::Seconds);
});

let result = val.Read(); // this will return the result immediatly
```

Async Read Write:
```
let val: RwLock<int> = 12; // implicit cast operator overloaded

Helper::RunAsync(() -> {
  let hold = val.Read(); 
  Helper::Sleep(10, Duration::Seconds);
});

val.Write(12); // this will wait until `hold` gets dropped so it is safe to write.
```

## Definition

```
struct RwLock<T> {
  public operator implicit(val: T) -> Sealed<T>;
  
  /// Return inner value. Block until its safe to read (all writes are done and out of scope)
  public func Read() -> T;
  
  /// Set inner value. Block until its safe to write (all reads and writes are done and out of scope)
  public func Write(value: T) -> none;
  
  public func SafeToRead() -> bool;
  public func SafeToWrite() -> bool;
}
```
