# Go Basics

All Go programs are made of packages. The program start running from the main package.

### Import

importing can be in bracket form or separate form.

```go
package main

# bracket form
import (
	"fmt"
	"math/rand"
)

# seperate form
import "fmt"
import "math/rand"

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}
```



### Export

In go, a name is exported if it begins with a capital letter. Pizza is exported but pizza is not.

```
package main

import (
	"fmt"
	"math"
)

# wrong
func main() {
	fmt.Println(math.pi)
}

#correct
func main() {
	fmt.Println(math.Pi)
}

```



### Functions

A function can take zero or more arguments. `add` takes two parameters of type `int`.

Notice that the type comes *after* the variable name.

```
func add(x int, y int) int {
	return x + y
}
```



If two parameters share a type, we can write a short hand.

```
func add(x, y int) int {
	return x + y
}
```



A function can return multiple results:

```
func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a,b := swap("hello", "world")
	fmt.Println(a, b)
}
```



The return value may be named. If so, they are treated as variables defined at the top of the function.

The names should be used to document the return values.

A `return` statement without arguments returns the named return values. This is known as a "naked" return. Bad readability.

```
import "fmt"

func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

func main() {
	fmt.Println(split(17))
}

// prints 7 10
```



### Variables

The `var` declares a list of variables; as in function argument lists, the type is last.

```
var c, python, java bool
func main() {
	var i int
	fmt.Println(i, c, python, java)
}
```

`var` declaration can include initializers, one per variable. If there exist initializers, the type of the variable can be omitted, it will be auto deduced.

```
var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}
```



Inside a function, `:=` can be used in place of a `var` declaration with implicit type. It is not available outside a function. 

```
package main

import "fmt"

func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```



### Basic Types

```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```

 ```
 package main
 
 import (
 	"fmt"
 	"math/cmplx"
 )
 
 var (
 	ToBe   bool       = false
 	MaxInt uint64     = 1<<64 - 1
 	z      complex128 = cmplx.Sqrt(-5 + 12i)
 )
 
 func main() {
 	fmt.Printf("Type: %T Value: %v\n", ToBe, ToBe)
 	fmt.Printf("Type: %T Value: %v\n", MaxInt, MaxInt)
 	fmt.Printf("Type: %T Value: %v\n", z, z)
 }
 ```



### Zero Values

Variables declared without an explicit initial value are given their *zero value*.

The zero value is:

- `0` for numeric types,
- `false` for the boolean type, and
- `""` (the empty string) for strings.



### Type Conversions

The expression `T(v)` converts the value `v` to the type `T`.

``` 
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)

i := 42
f := float64(i)
u := uint(f)
```



### Type inference

When declaring a variable without specifying an explicit type, the type of the variable is inferred from the value of the right hand side.

```
var i int
j := i // j is an int

# or simply
i := 42           // int
a := i 			  // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```



### Constants

Constants are declared like variables, but with the `const` keyword.

Constants can be character, string, boolean, or numeric values.

Constants cannot be declared using the `:=` syntax.

```
package main

import "fmt"

const Pi = 3.14

func main() {
	const World = "world"
	fmt.Println("Hello", World)
	fmt.Println("Happy", Pi, "Day")

	const Truth = true
	fmt.Println("Go rules?", Truth)
}
```



### Numeric Constants

Numeric constants are high-precision *values*.

An untyped constant takes the type needed by its context.

```
package main

import "fmt"

const (
	// Create a huge number by shifting a 1 bit left 100 places.
	// In other words, the binary number that is 1 followed by 100 zeroes.
	Big = 1 << 100
	// Shift it right again 99 places, so we end up with 1<<1, or 2.
	Small = Big >> 99
)

func needInt(x int) int { return x*10 + 1 }
func needFloat(x float64) float64 {
	return x * 0.1
}

func main() {
	fmt.Println(needInt(Small))
	fmt.Println(needFloat(Small))
	fmt.Println(needFloat(Big))
}
```



### Control Flow

##### For

The basic `for` loop has three components separated by semicolons:

- the init statement: executed before the first iteration
- the condition expression: evaluated before every iteration
- the post statement: executed at the end of every iteration

```
func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
}

```

The init and post statements are optional.

```
func main() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
}

```

At that point you can drop the semicolons: C's `while` is spelled `for` in Go.

```
func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}
```

If you omit the loop condition it loops forever, so an infinite loop is compactly expressed.

```
func main() {
	for {
	}
}
```

##### If

Go's `if` statements are like its `for` loops; the expression need not be surrounded by parentheses `( )` but the braces `{ }` are required.

```
func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}
```

Like `for`, the `if` statement can start with a short statement to execute before the condition. Variables declared inside an `if` short statement are also available inside any of the `else` blocks.

```
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}
```

##### Switch

A `switch` statement is a shorter way to write a sequence of `if - else` statements. It runs the first case whose value is equal to the condition expression. No need to break like in other languages.

```
func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}
```

##### Defer

A defer statement defers the execution of a function until the surrounding function returns.

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

```
func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order.