---
tags:
  - go
  - overview
  - cheatsheet
  - concurrency
---

# Go
>[!INFO]- References
> [Go Tour](https://go.dev/tour/welcome/1)

## Types
* bool
	* `true` or `false`
	* Default: `false`
* string
	* Default: `""`
* int
	* int, int8, int16, int32, int64
	* int usually matches the sytem (32-bit or 64-bit)
	* Default: `0`
* uint
	* uint, uint8, uint16, uint32, uint64
	* uint usually matches the system(32-bit or 64-bit)
	* Default: `0`
* byte
	* Alias for uint8
* rune
	* Alias for int32
	* Represents a Unicode code point
* float
	* float32 float64
	* Default: `0`
* complex
	* complex64 complex128
	* Default: `0`

### Type Conversion
`T(v)` : Converts value `v` to type `T`

### Generic Types
**Singly-Linked List Example**

```go
type List[T any] struct {
	next *List[T]
	val  T
}
```

## Packages
```go
import "fmt"
import "math"
```

```go
import (
	"fmt"
	"math"
	"strings"
)
```

## Creating an Executable
```go
package main

func main() {
}
```

## Variables
* `var` statement declares a list of variables
* type is last argument

> [!WARNING]
> If you have an unused variable, the Go build will fail.

### Example
```go
package main

import "fmt"

// Package Level Scope
var c, python, java bool

func main() {
	// Function Level Scope
	var i int
	fmt.Println(i, c, python, java)
}
```

### Initializing Variables
#### Var Statement
Can initialize each variable in list

```go
var i, j int = 1, 2
```

Can omit type if initializer is present

```go
var c, python, java = true, false, "no!"
```

> [!NOTE]+
> You cannot change the type of variable after initialization and you cannot simply delete variable after declaration.

#### Short Variable Declarations
**Within a *function***, you can use short assignment statement (`:=`) in place of `var` 

```go
package main

import "fmt"

func main() {
	// Assignment
	var i, j int = 1, 2
	
	// Short Assignment
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

> [!WARNING]+
> Short assignments are not available outside of function scope.

Can infer typing from initialization

```go
var i int
j := i // j is an int
```

#### Constants
```go
const PI = 3.14
```

Must be declared using `const`, cannot use `:=`

Numeric constants are *high-precision values*.

## Flow Control
### Basics
#### For
```go
sum := 0
for i := 0; i < 10; i++ {  // Must end with open curly brace
	sum += i
}
fmt.Println(sum)
```

#### While
```go
sum := 1
for sum < 1000 {  // Must end with open curly brace
	sum += sum
}
fmt.Println(sum)
```

**Forever Loop**

```go
for {
}
```

#### If
```go
func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}
```

#### If-Else
```go
if x < 0 {
	fmt.Println(x)
} else {  // else must be at end of close curly brace
	fmt.Println(y)
}
```

```go
if x < 0 {
	fmt.Println(x)
} else if y < 0 {  // else must be at end of close curly brace
	fmt.Println(y)
} else {
	fmt.Print(x)
	fmt.Println(y)
}
```

#### Switch
```go
switch os {
case "darwin":
	fmt.Println("OS X.")
case "linux":
	fmt.Println("Linux")
default:
	fmt.Println("%s.\n", os)
}
```

**No Condition**

```go
switch {
case t.Hour() <  12:
	fmt.Println("Good morning!")
case t.Hour() < 17:
	fmt.Println("Good afternoon.")
default:
	fmt.Println("Good evening.")
}
```

### Defer
Defers the execution of a funcction until the surround function returns.

```go
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

**Output**

> hello
> world
> 

#### Stacking Defers
Deferred functions calls are pushed onto a stack **(LIFO)**

```go
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 4; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

**Output**

> counting
> done
> 3
> 2
> 1
> 0
> 

## Pointers
`*T` is a pointer to a `T` value. Its zero value is `nil`.
`var p *int`

`&` operator generates  a pointer
```go
i := 42
p = &i

fmt.Println(*p) // read i through pointer p
*p = 21 // set i through pointer p
```

Go has no pointer arithmetic.

## Functions
```go
func function_name([parameter list]) [return_types]
{
	...
}
```

### Function Closures
A closure is a function value that references varaibles from outside its body.
```go
package main

import "fmt"

func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}

func main() {
	pos, neg := adder(), adder()
	for i := 0; i < 10; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
	}
}
```

### Methods
A method is a function with a special *receiver* argument.

#### Value Receiver
Uses the values 

```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}
```

#### Pointer Receiver
Modifies the values

```go
package main

import (
	"fmt"
	"math"
)

type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}

func main() {
	v := Vertex{3, 4}
	v.Scale(10)
	fmt.Println(v.Abs())
}
```

#### Non-Struct Type Method
```go
package main

import (
	"fmt"
	"math"
)

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

func main() {
	f := MyFloat(-math.Sqrt2)
	fmt.Println(f.Abs())
}
```

### Type Parameters
`func Index[T comparable](s []T, x T) int`
*  `s` is a slice of any type `T` that fulfills the built-in constraint `comparable`.
* `x` is also a value of the same type.
* `comparable` is a useful constraint that makes it possible to use the `==` and `!=` operators on values of the type.

#### Example
```go
package main

import "fmt"

// Index returns the index of x in s, or -1 if not found.
func Index[T comparable](s []T, x T) int {
	for i, v := range s {
		// v and x are type T, which has the comparable
		// constraint, so we can use == here.
		if v == x {
			return i
		}
	}
	return -1
}

func main() {
	// Index works on a slice of ints
	si := []int{10, 20, 15, -10}
	fmt.Println(Index(si, 15))

	// Index also works on a slice of strings
	ss := []string{"foo", "bar", "baz"}
	fmt.Println(Index(ss, "hello"))
}
```

## Struct
### Type Declaration
`type NewType <T>`
`type MyFloat float64`

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	fmt.Println(v)
	v.X = 4
	fmt.Println(v.X)
}
```

### Literals
```go
package main

import "fmt"

type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)

func main() {
	fmt.Println(v1, p, v2, v3)
}
```

### Pointers to Struct
| C       | Go  |
| ------- | --- |
| (\*p).X | p.X |

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v
	p.X = 1e9
	fmt.Println(v)
}
```

## Interface
### Setting Interface
Method signatures; not implemented
```go
type Abser interface {
	Abs() float64
}
```

Interfaces are implemented implicitly by implementing its method
```go
package main

import "fmt"

type I interface {
	M()
}

type T struct {
	S string
}

// This method means type T implements the interface I,
// but we don't need to explicitly declare that it does so.
func (t T) M() {
	fmt.Println(t.S)
}

func main() {
	var i I = T{"hello"}
	i.M()
}
```

Interfaces can be thought of as a tuple of a value and a concrete type:
`(value, type)`

### Handling `nil`
```go
type I interface {
	M()
}

type T struct {
	S string
}

func (t *T) M() {
	if t == nil {
		fmt.Println("<nil>")
		return
	}
	fmt.Println(t.S)
}
```

A `nil` interface value holds neither value nor concrete type.
> [!WARNING]
> Calling a method on a `nil` interface causes a ***run-time error***.

### Empty Interface
`interface{}`

```go
var i interface{}
fmt.Printf("(%v, %T)\n", i, i)
```

**Output**

> (\<nil\>, \<nil\>)

### Type Assertions
`t := i.(T)`
`t, ok := i.(T)`

```go
var i interface{} = "hello"

s := i.(string)
fmt.Println(s)

s, ok := i.(string)
fmt.Println(s, ok)

f, ok := i.(float64)
fmt.Println(f, ok)

f = i.(float64) // panic
fmt.Println(f)
```

### Type Switches
```go
func do(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Printf("Twice %v is %v\n", v, v*2)
	case string:
		fmt.Printf("%q is %v bytes long\n", v, len(v))
	default:
		fmt.Printf("I don't know about type %T!\n", v)
	}
}
```

### Typical Interfaces
#### Stringer
A `Stringer` is a type that can describe itself as a string.
```go
type Stringer interface {
	String() string
}
```

**Example**

```go
package main

import "fmt"

type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

func main() {
	a := Person{"Arthur Dent", 42}
	z := Person{"Zaphod Beeblebrox", 9001}
	fmt.Println(a, z)
}
```

#### Errors
For error reporting, use the `error` interface.
```go
type error interface {
    Error() string
}
```

**Example**

```go
package main

import (
	"fmt"
	"time"
)

type MyError struct {
	When time.Time
	What string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s",
		e.When, e.What)
}

func run() error {
	return &MyError{
		time.Now(),
		"it didn't work",
	}
}

func main() {
	if err := run(); err != nil {
		fmt.Println(err)
	}
}
```

#### Readers
The `io` package specifies the `io.Reader` interface.

```go
func (T) Read(b [byte]) (n int, err error)
```

**Example**

```go
package main

import (
	"fmt"
	"io"
	"strings"
)

func main() {
	r := strings.NewReader("Hello, Reader!")

	b := make([]byte, 8)
	for {
		n, err := r.Read(b)
		fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
		fmt.Printf("b[:n] = %q\n", b[:n])
		if err == io.EOF {
			break
		}
	}
}
```

#### Image
The `image` package defines the `Image` interface.

```go
package image

type Image interface {
    ColorModel() color.Model
    Bounds() Rectangle
    At(x, y int) color.Color
}
```

**Example**

```go
package main

import (
	"fmt"
	"image"
)

func main() {
	m := image.NewRGBA(image.Rect(0, 0, 100, 100))
	fmt.Println(m.Bounds())
	fmt.Println(m.At(0, 0).RGBA())
}
```

## Array
### Basics
Indexing starts at 0

`var a [10]int`

**Array Literal**
`primes := [6]int{2, 3, 5, 7, 11, 13}`

### Array Functions
| Function | Description                                                                              |
| -------- | ---------------------------------------------------------------------------------------- |
| len(a)   | Returns length of array or slice                                                         |
| cap(s)   | The number of elements in the underlying array, counting from first element in the slice |
|          |                                                                                          |

### Slicing
Slices describe a section of an underlying array
**Slices do not store any data**

`a[low : high]`

**Slice Literal** builds an array literal then builds a slice that references it
`[]bool{true, true, false}`

**Slice Defaults**
`var a [10]int`

| Equivalent Slices |
| ----------------- |
| a[0:10]           |
| a[:10]            |
| a[0:]             |
| a[:]              |

**Nil Slices**
`var s []int == nil`

### Append
```go
package main

import "fmt"

func main() {
	var s []int
	printSlice(s) // len=0 cap=0 []

	// append works on nil slices.
	s = append(s, 0)
	printSlice(s) // len=1 cap=1 [0]

	// The slice grows as needed.
	s = append(s, 1)
	printSlice(s) // len=2 cap=2 [0 1]

	// We can add more than one element at a time.
	s = append(s, 2, 3, 4)
	printSlice(s) // len=5 cap=6 [0 1 2 3 4]
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

### Dynamically-Sized Arrays
`a := make([]int, 0, 5)`
Creates an array of `len(a) == 0` and `cap(a) == 5`

`a := make([]int, 5)`
reates an array of `len(a) == 5` and `cap(a) == 5`

### 2D-Array
```go
board := [][]string{
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
	[]string{"_", "_", "_"},
}
```

## Range
```go
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow { // idx & value
		fmt.Printf("2**%d = %d\n", i, v)
	}
	
	for _, v := range pow { // omit idx
		fmt.Printf("%d\n", v)
	}
	
	for i := range pow { // omit value
		fmt.Printf("2**%d\n", i)
	}
}
```

## Map
```go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}
```

### Map Literals
```go
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m = map[string]Vertex{
	"Bell Labs": Vertex{
		40.68433, -74.39967,
	},
	"Google": Vertex{
		37.42202, -122.08408,
	},
}

/*
var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}
*/

func main() {
	fmt.Println(m)
}
```

### Map Functions
| Function            | Description                                                                                                     |
| ------------------- | --------------------------------------------------------------------------------------------------------------- |
| `m[key] = elem`     | Set key to element                                                                                              |
| `elem = m[key]`     | Get element from map                                                                                            |
| `delete(m, key)`    | Delete element                                                                                                  |
| `elem, ok = m[key]` | Test that key is present. If key is in m, `ok` is `true`, else `false`. (Can be done in short declaration form) |

## Concurrency
### Goroutines
A *goroutine* is a lightweight thread managed by the Go runtime.

`go f(x, y, z)` starts a new goroutine running `f(x, y, z)`

#### Example
```go
package main

import (
	"fmt"
	"time"
)

func say(s string) {
	for i := 0; i < 5; i++ {
		time.Sleep(100 * time.Millisecond)
		fmt.Println(s)
	}
}

func main() {
	go say("world")
	say("hello")
}
```

### Channels
#### Basics
Channels are a typed conduit through which you can send and receive values with the channel operator, `<-`.

```go
ch <- v // Send v to channel ch
v := <-ch // Receive from ch, and assign to v.
```

By default, *sends* and *receives* **block** until the other side is ready. This allows goroutines to synchronize without explicit locks or condition variables.

**Initialization**
`ch := make(chan int)`

**Example**

```go
package main

import "fmt"

func sum(s []int, c chan int) {
	sum := 0
	for _, v := range s {
		sum += v
	}
	c <- sum // send sum to c
}

func main() {
	s := []int{7, 2, 8, -9, 4, 0}

	c := make(chan int)
	go sum(s[:len(s)/2], c)
	go sum(s[len(s)/2:], c)
	x, y := <-c, <-c // receive from c

	fmt.Println(x, y, x+y)
}
```

#### Buffer
Channels can be *buffered*, which only blocks sends when the buffer is full and blocks receives when the buffer is empty.
`ch := make(chan int, 100)`

#### Close
`close(ch)`

Receivers can test if a channel is closed by assigning a second parameter
`v, ok := <-ch`

#### Range
Repeatedly receives values until the channel is closed.
```go
ch := make(chan int, 10)
go fibonacci(cap(c), ch)
for i := range ch {
	fmt.Println(i)
}
```

### Select
`select` lets a goroutine wait on multiple communication operations.
Chooses randomly if multiple are ready.

```go
package main

import "fmt"

func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
		case c <- x:
			x, y = y, x+y
		case <-quit:
			fmt.Println("quit")
			return
		}
	}
}

func main() {
	c := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	fibonacci(c, quit)
}
```

The `default` case runs if no other case is ready.
Can be used to send or receive without blocking.

```go
select {
case i := <-c:
    // use i
default:
    // receiving from c would block
}
```

### Mutex
Can use the `sync` package for Mutexes
  * `Lock`
  * `Unlock`

```go
import "sync"

// SafeCounter is safe to use concurrently.
type SafeCounter struct {
	mu sync.Mutex
	v  map[string]int
}

// Inc increments the counter for the given key.
func (c *SafeCounter) Inc(key string) {
	c.mu.Lock()
	// Lock so only one goroutine at a time can access the map c.v.
	c.v[key]++
	c.mu.Unlock()
}

// Value returns the current value of the counter for the given key.
func (c *SafeCounter) Value(key string) int {
	c.mu.Lock()
	// Lock so only one goroutine at a time can access the map c.v.
	defer c.mu.Unlock()
	return c.v[key]
}
```

## fmt (-> [doc](https://pkg.go.dev/fmt))
Formatted I/O with functions analogous to C's printf and scanf.

### Verbs
#### General
| Symbol | Definition           |
| ------ | -------------------- |
| %v     | Default format       |
| \%\%   | Literal percent sign |

#### Boolean
| Symbol | Definition                     |
| ------ | ------------------------------ |
| %t     | the word *"true"* or *"false"* |

#### Integer
| Symbol | Definition                                  |
| ------ | ------------------------------------------- |
| %b     | Base 2                                      |
| %c     | Character represented by Unicode code point |
| %d     | Base 10                                     |
| %o     | Base 8                                      |
| %x/%X  | Base 16 (lowercase/uppercase)               |
| $U     | Unicode format: U+1234                      |

#### Float
| Symbol | Definition                                  |
| ------ | ------------------------------------------- |
| %e/%E  | Scientific Notation (lowercase/uppercase e) |
| %f/%F  | Decimal point                               |
| %x/%X  | Hexadecimal Notation e.g. -0x1.23abcp+20    |
| %g/%G       | %e/%E for large exponents, %f/%F otherwise                                             |

#### String/Slice of Bytes
| Symbol | Definition                                           |
| ------ | ---------------------------------------------------- |
| %s     | uninterpreted bytes of string or slice               |
| %q     | a double-quoted string safely escaped with Go syntax |
| %x/%X  | Base 16 (lower/uppercase), two chars per byte        |

#### Defaults
| Type                     | Symbol |
| ------------------------ | ------ |
| bool                     | %t     |
| int, int8, etc.          | %d     |
| uint, uint8, etc.        | %d     |
| float32, complex64, etc. | %g     |
| string                   | %s     |

#### Options
| Symbol | Definition                       |
| ------ | -------------------------------- |
| %f     | Default width, default precision |
| %9f    | width 9, default precision       |
| %.2f   | default width, precision 2       |
| %9.2f  | width 9, precision 2             |
| %9.f   | width 9, precision 0             |
|        |                                  |
| %[2]d  | Get 2nd argument               |

### Printing/Writing
* `Printf` outputs to the standard output stream (`stdout`)
* `Fprintf` goes to a file handle (`FILE*`)
* `Sprintf` goes to a buffer you allocated. (`char*`)
* `Println` adds a newline to the end

#### Example
```go
package main

import "fmt"

func main() {
	fmt.Printf("%d\n", 11)
	
	// Equivalent to fmt.Sprintf("%6.2f", 12.0)
	fmt.Printf("%[3]*.[2]*[1]f\n", 12.0, 2, 6)
	fmt.Printf("%[2]d %[1]d", 11, 12)
}
```

**Output**

> 11
> 12.00
> 11 12

### Scanning
* `Scan`, `Scanf`, and `Scanln` read from `os.Stdin`
* `Fscan`, `Fscanf`, `Fscanln` read from a specified `io.Reader`
* `Sscan`, `Sscanf`, and `Sscanln` read from an argument string

#### Example
```go
package main

import "fmt"

func main() {
	var s string
	var i int

	fmt.Sscanf(" 1234567 ", "%5s%d", &s, &i)
	fmt.Printf("%q\n", s)
	fmt.Println(i)
}
```

**Output**

> "12345"
> 67

## sync (-> [doc](https://pkg.go.dev/sync))
Provides basic synchronization primitives such as #mutex.
  * Other than `Once` and `WaitGroup` types, most are intended for use by low-level library routines.
  * Higher-level synchronization is better done via channels and communication.

### Types
#### [Once](https://pkg.go.dev/sync#Once)
Once is an object that will perform exactly one action.
  * A Once must not be copied after first use.

##### Example
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var once sync.Once
	onceBody := func() {
		fmt.Println("Only once")
	}
	done := make(chan bool)
	for i := 0; i < 10; i++ {
		go func() {
			once.Do(onceBody)
			done <- true
		}()
	}
	for i := 0; i < 10; i++ {
		<-done
	}
}
```

#### [WaitGroup]()
Waits for a collection of goroutines to finish.
  * The main goroutine `Adds` to set the number of goroutines to wait for.
  * Each goroutine runs and calls `Done` when finished.
  * A WaitGroup must not be copied after first use.

##### Example
```go
package main

import (
	"sync"
)

type httpPkg struct{}

func (httpPkg) Get(url string) {}

var http httpPkg

func main() {
	var wg sync.WaitGroup
	var urls = []string{
		"http://www.golang.org/",
		"http://www.google.com/",
		"http://www.example.com/",
	}
	for _, url := range urls {
		// Increment the WaitGroup counter.
		wg.Add(1)
		// Launch a goroutine to fetch the URL.
		go func(url string) {
			// Decrement the counter when the goroutine completes.
			defer wg.Done()
			// Fetch the URL.
			http.Get(url)
		}(url)
	}
	// Wait for all HTTP fetches to complete.
	wg.Wait()
}
```

#### [Cond](https://pkg.go.dev/sync#Cond)
Condition variable

| Functions                      | Description                                                                                   |
| ------------------------------ | --------------------------------------------------------------------------------------------- |
| `func NewCond(l Locker) *Cond` | Returns a new Cond with Locker `l`                                                            |
| `func (c *Cond) Broadcast()`   | Wakes all goroutines waiting on `c`                                                           |
| `func (c *Cond) Signal()`      | Signal wakes one goroutine waiting on `c`, if there is any                                    |
| `func (c *Cond) Wait()`        | Unlocks Locker and suspends calling goroutine. Waits for `Broadcast` or `Signal` to be awoken |

#### [Locker](https://pkg.go.dev/sync#Locker)
An object that can be locked or unlocked.

```go
type Locker interface {
	Lock()
	Unlock()
}
```

#### [Map](https://pkg.go.dev/sync#Map)
A thread-safe version of map.

## strconv (-> [doc](https://pkg.go.dev/strconv))
Implements conversions to and from string.

### Numeric Conversion
#### String to Int
`i, err := strconv.Atoi("-42")`

#### Parsers
| Type    | Function                           | Arguments                             |
| ------- | ---------------------------------- | ------------------------------------- |
| `bool`  | `strconv.ParseBool("true")`        | `s string`                            |
| `float` | `strconv.ParseFloat("3.1415", 64)` | `s string`, `bitSize int`             |
| `int`   | `strconv.ParseInt("-42", 10, 64)`  | `s string`, `base int`, `bitSize int` |
| `uint`  | `strconv.ParseUint("42", 10, 64)`  | `s string`, `base int`, `bitSize int` |

The parse functions return the widest type (float64, int64, and uint64), but if the size argument specifies a narrower width the result can be converted to that narrower type without data loss.

### String Conversion
#### Int to String
`s := strconv.Itoa(-42)`

#### Formatters
| Type    | Function                                   | Arguments                                        |
| ------- | ------------------------------------------ | ------------------------------------------------ |
| `bool`  | `strconv.FormatBool(true)`                 | `b bool`                                         |
| `float` | `strconv.FormatFloat(3.1415, 'E', -1, 64)` | `f float`, `fmt byte`, `prec int`, `bitSize int` |
| `int`   | `strconv.FormatInt(-42, 16)`               | `i int`, `base int`                              |
| `uint`  | `strconv.FormatUint(42, 16)`               | `u uint`, `base int`                             |

## strings (-> [doc](https://pkg.go.dev/strings#pkg-overview))
Implements simple functions to manipulate UTF-8 encoded strings.

### [Fields](https://pkg.go.dev/strings#Fields)
`func Fields(s string) []string`
Splits the string s around each instance of one or more consecutive white space characters.
  * As defined by unicode.IsSpace. 
  * Returns a slice of substrings of s or an empty slice if s contains only white space.

#### Example
```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Printf("Fields are: %q", strings.Fields("  foo bar  baz   "))
}
```

**Output**

> Fields are: \["foo" "bar" "baz"\]

### [FieldsFunc](https://pkg.go.dev/strings#FieldsFunc)
`func FieldsFunc(s string, f func(rune) bool) []string`
Splits the string s at each run of Unicode code points c satisfying f(c).
  * Returns an array of slices of s.
  * If all code points in s satisfy f(c) or the string is empty, an empty slice is returned.

#### Example
```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	f := func(c rune) bool {
		return !unicode.IsLetter(c) && !unicode.IsNumber(c)
	}
	fmt.Printf("Fields are: %q", strings.FieldsFunc("  foo1;bar2,baz3...", f))
}
```

**Output**

> Fields are: \["foo1" "bar2" "baz3"\]

### [ToLower](https://pkg.go.dev/strings#ToLower)
`func ToLower(s string) string`
Returns s with all Unicode letters mapped to their lower case.

### [ToUpper](https://pkg.go.dev/strings#ToUpper)
`func ToUpper(s string) string`
Returns s with all Unicode letters mapped to their upper case.

### [ContainsAny](https://pkg.go.dev/strings#ContainsAny)
`func ContainsAny(s, chars string) bool`
Returns `True` if any Unicode code points in `chars` are within `s`.
## sort (-> [doc](https://pkg.go.dev/sort))
Provides primitives for sorting slices and user-defined collections.

### Basic Sort
For a given struct `T`, you need to define a type for an array or collection of `T` called `S`. 
  * `type T struct { ...`
  * `type S []T`
A basic sort uses 3 methods defined for a given struct:
  * `func (a S) Len() int`
  * `func (a S) Swap(i, j int)`
  * `func (a S) Less (i, j int) bool`

#### Simple Method
```go
a := []T{
	...
}

sort.Slice(a, func(i, j int) bool {
	return a[i].Value > a[j].Value
})
```

#### Example
```go
package main

import (
	"fmt"
	"sort"
)

type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("%s: %d", p.Name, p.Age)
}

// ByAge implements sort.Interface for []Person based on
// the Age field.
type ByAge []Person

func (a ByAge) Len() int           { return len(a) }
func (a ByAge) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }
func (a ByAge) Less(i, j int) bool { return a[i].Age < a[j].Age }

func main() {
	people := []Person{
		{"Bob", 31},
		{"John", 42},
		{"Michael", 17},
		{"Jenny", 26},
	}

	fmt.Println(people)
	// There are two ways to sort a slice. First, one can define
	// a set of methods for the slice type, as with ByAge, and
	// call sort.Sort. In this first example we use that technique.
	sort.Sort(ByAge(people))
	fmt.Println(people)

	// The other way is to use sort.Slice with a custom Less
	// function, which can be provided as a closure. In this
	// case no methods are needed. (And if they exist, they
	// are ignored.) Here we re-sort in reverse order: compare
	// the closure with ByAge.Less.
	sort.Slice(people, func(i, j int) bool {
		return people[i].Age > people[j].Age
	})
	fmt.Println(people)
}
```

### SortKeys
Sort by a later defined key instead of a specific comparable in the struct.

```go
package main

import (
	"fmt"
	"sort"
)

// A couple of type definitions to make the units clear.
type earthMass float64
type au float64

// A Planet defines the properties of a solar system object.
type Planet struct {
	name     string
	mass     earthMass
	distance au
}

// By is the type of a "less" function that defines the ordering of its Planet arguments.
type By func(p1, p2 *Planet) bool

// Sort is a method on the function type, By, that sorts the argument slice according to the function.
func (by By) Sort(planets []Planet) {
	ps := &planetSorter{
		planets: planets,
		by:      by, // The Sort method's receiver is the function (closure) that defines the sort order.
	}
	sort.Sort(ps)
}

// planetSorter joins a By function and a slice of Planets to be sorted.
type planetSorter struct {
	planets []Planet
	by      func(p1, p2 *Planet) bool // Closure used in the Less method.
}

// Len is part of sort.Interface.
func (s *planetSorter) Len() int {
	return len(s.planets)
}

// Swap is part of sort.Interface.
func (s *planetSorter) Swap(i, j int) {
	s.planets[i], s.planets[j] = s.planets[j], s.planets[i]
}

// Less is part of sort.Interface. It is implemented by calling the "by" closure in the sorter.
func (s *planetSorter) Less(i, j int) bool {
	return s.by(&s.planets[i], &s.planets[j])
}

var planets = []Planet{
	{"Mercury", 0.055, 0.4},
	{"Venus", 0.815, 0.7},
	{"Earth", 1.0, 1.0},
	{"Mars", 0.107, 1.5},
}

// ExampleSortKeys demonstrates a technique for sorting a struct type using programmable sort criteria.
func main() {
	// Closures that order the Planet structure.
	name := func(p1, p2 *Planet) bool {
		return p1.name < p2.name
	}
	mass := func(p1, p2 *Planet) bool {
		return p1.mass < p2.mass
	}
	distance := func(p1, p2 *Planet) bool {
		return p1.distance < p2.distance
	}
	decreasingDistance := func(p1, p2 *Planet) bool {
		return distance(p2, p1)
	}

	// Sort the planets by the various criteria.
	By(name).Sort(planets)
	fmt.Println("By name:", planets)

	By(mass).Sort(planets)
	fmt.Println("By mass:", planets)

	By(distance).Sort(planets)
	fmt.Println("By distance:", planets)

	By(decreasingDistance).Sort(planets)
	fmt.Println("By decreasing distance:", planets)
}
```

### SortMultiKeys
Sort by multiple keys at a given time.

```go
package main

import (
	"fmt"
	"sort"
)

// A Change is a record of source code changes, recording user, language, and delta size.
type Change struct {
	user     string
	language string
	lines    int
}

type lessFunc func(p1, p2 *Change) bool

// multiSorter implements the Sort interface, sorting the changes within.
type multiSorter struct {
	changes []Change
	less    []lessFunc
}

// Sort sorts the argument slice according to the less functions passed to OrderedBy.
func (ms *multiSorter) Sort(changes []Change) {
	ms.changes = changes
	sort.Sort(ms)
}

// OrderedBy returns a Sorter that sorts using the less functions, in order.
// Call its Sort method to sort the data.
func OrderedBy(less ...lessFunc) *multiSorter {
	return &multiSorter{
		less: less,
	}
}

// Len is part of sort.Interface.
func (ms *multiSorter) Len() int {
	return len(ms.changes)
}

// Swap is part of sort.Interface.
func (ms *multiSorter) Swap(i, j int) {
	ms.changes[i], ms.changes[j] = ms.changes[j], ms.changes[i]
}

// Less is part of sort.Interface. It is implemented by looping along the
// less functions until it finds a comparison that discriminates between
// the two items (one is less than the other). Note that it can call the
// less functions twice per call. We could change the functions to return
// -1, 0, 1 and reduce the number of calls for greater efficiency: an
// exercise for the reader.
func (ms *multiSorter) Less(i, j int) bool {
	p, q := &ms.changes[i], &ms.changes[j]
	// Try all but the last comparison.
	var k int
	for k = 0; k < len(ms.less)-1; k++ {
		less := ms.less[k]
		switch {
		case less(p, q):
			// p < q, so we have a decision.
			return true
		case less(q, p):
			// p > q, so we have a decision.
			return false
		}
		// p == q; try the next comparison.
	}
	// All comparisons to here said "equal", so just return whatever
	// the final comparison reports.
	return ms.less[k](p, q)
}

var changes = []Change{
	{"gri", "Go", 100},
	{"ken", "C", 150},
	{"glenda", "Go", 200},
	{"rsc", "Go", 200},
	{"r", "Go", 100},
	{"ken", "Go", 200},
	{"dmr", "C", 100},
	{"r", "C", 150},
	{"gri", "Smalltalk", 80},
}

// ExampleMultiKeys demonstrates a technique for sorting a struct type using different
// sets of multiple fields in the comparison. We chain together "Less" functions, each of
// which compares a single field.
func main() {
	// Closures that order the Change structure.
	user := func(c1, c2 *Change) bool {
		return c1.user < c2.user
	}
	language := func(c1, c2 *Change) bool {
		return c1.language < c2.language
	}
	increasingLines := func(c1, c2 *Change) bool {
		return c1.lines < c2.lines
	}
	decreasingLines := func(c1, c2 *Change) bool {
		return c1.lines > c2.lines // Note: > orders downwards.
	}

	// Simple use: Sort by user.
	OrderedBy(user).Sort(changes)
	fmt.Println("By user:", changes)

	// More examples.
	OrderedBy(user, increasingLines).Sort(changes)
	fmt.Println("By user,<lines:", changes)

	OrderedBy(user, decreasingLines).Sort(changes)
	fmt.Println("By user,>lines:", changes)

	OrderedBy(language, increasingLines).Sort(changes)
	fmt.Println("By language,<lines:", changes)

	OrderedBy(language, increasingLines, user).Sort(changes)
	fmt.Println("By language,<lines,user:", changes)
}
```

### Basic Type Sorts
| Type     | Sort Function                | Sort Check Function                        |
| -------- | ---------------------------- | ------------------------------------------ |
| `float`  | `sort.Float64s(x []float64)` | `sort.Float64sAreSorted(x []float64) bool` |
| `int`    | `sort.Ints(x []int)`         | `sort.IntsAreSorted(x []int) bool`         |
| `string` | `sort.String(x []string)`    | `sort.StringsAreSorted(x []string) bool`   |
