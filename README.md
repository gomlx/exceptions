# exceptions

Simple exceptions support for Go, using `panic()` and `recover()`.

Go's `panic(any)` will unwind the stack one frame at a time until there is a matching call to `recover()`.
This library calls the parameter passed to `panic` an _exception_, and provides simplified methods to _catch_ those exceptions. 

## Examples

* Catching an exception (any panic):

```go
    import . "github.com/gomlx/exceptions"

    ...
	exception := Try(fn)
    if exception != nil {
        ... 
    }
```

* Throwing an exception: one can simply `panic` or use the provided `Panicf`, which
  converts the given text into an error+stack:

```go
    func MyRatio(myThing, total float64) float64 {
		if total == 0 {
			Panicf("MyRatio(%f, %f) has no things (0) to calculate a ratio from", myThing, total)
        }
		return myThing/total
    }
```

* Go `panic` supports any value to be passed, which we interpret as an exception.
  `TryCatch[E any]` allows a function to be called and an exception of type `E` to be caught.
  Any other exceptions are not handled and continues to `panic`.

```go
	var x ResultType
	err := TryCatch[error](func() { x = DoSomething() })
	if err != nil {
		// Handle error ...
	}
```