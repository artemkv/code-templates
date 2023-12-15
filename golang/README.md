## "Hello World"

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

## Variable declaration

```go
var x int = 42
var y = 42 // type inference
z := 42 // "simplified version" (not allowed on package level)

var (
    x int = 4
    s string = "hello"
)

const (
    AAA = iota
    BBB
)
```

## Conditional statements

```go
if x < 0 {
    return 0
}
return x // avoid else (indent error flow)

// assign and check
if v, ok := m["key"]; ok {
    fmt.Printf("Value is: %d", v)
}
```

## Loops

```go
// for loop
for i := 0; i < 5; i++ {
    fmt.Printf("Iteration %d\n", i)
}

// "while" loop:
for x < 5 {
    // do something
}

// infinite loop:
for {
    // do something
}

// for range loop
for k, v := range slice {
    fmt.Printf("%d %d\n", k, v)
}
```

## Functions

```go
// multiple return values
func calc(x int, y int) (int, int) {
	return x + y, x * y
}

// named result parameters (questionable)
func calc(x int, y int) (sum int, prod int) {
    sum = x + y
    prod = x * y
    return
}

// method
func (p Person) greet() {
    println("Hello, " + p.name)
}
```

## Pointers

```go
var aaa int = 42
var bbb *int = &aaa
aaa = 27
fmt.Printf("%v %v\n", aaa, *bbb)
```

## Collections

```go
// arrays
var arr [3]int // empty array of size 3
arr := [...]int{1, 2, 3} // auto-detect size
n := len(arr) // get length

// slices
slice := []int{1, 2, 3}
var slice []int // instead of slice := []int{}
slice := arr[:]

// slice of length 10 and capacity 10
aa := make([]int, 10) // [0 0 0 0 0 0 0 0 0 0]
aa[0] = 5 // [5 0 0 0 0 0 0 0 0 0]

// slice of length 0 and capacity 10
aa := make([]int, 0, 10) // []
aa = append(aa, 5) // [5]

// maps
m := map[string]int{
    "aaa": 1,
    "b":   2,
}

// dynamic allocation
m := make(map[string]int)
m["key"] = 5
delete(m, "key")
```

## Structs

```go
type Person struct {
    name string
    age  int
}
```

## Embedding vs nesting

```go
// embedding
type Person struct {
    name    string
    age     int
    Address // accessed directly: p.street
}

// nesting
type Person struct {
    name    string
    age     int
    address Address // accessed using 2 dots: p.address.street
}
```

## Error handling

```go
f, err := os.Open("filename.ext")
if err != nil {
    log.Fatal(err) // calls os.Exit(1)
}

// create error
err := fmt.Errorf("Invalid value %s", val)

// ignore value
if _, ok = m["aaa"]; ok {
    fmt.Printf("In the map")
}

// wrap errors
// err := ...
if err != nil {
    return fmt.Errorf("failed to do stuff: %w", err)
}

// defer
f, err := os.Open("filename.ext")
if err != nil {
    return "", err
}
defer f.Close()
```

## Interfaces

```go
type Logger interface {
    debug(string)
}

type ConsoleLogger struct{}

func (logger ConsoleLogger) debug(s string) {
    fmt.Println("DEBUG: " + s)
}

func main() {
    var logger Logger = ConsoleLogger{}
    logger.debug("Beginning of the program")
}
```

## Casting interface back to specific struct

```go
consoleLogger, ok := logger.(ConsoleLogger)
if ok {
    consoleLogger.info("Running with console logger")
}
```

## Imitate Classes, Constructors, and Getter/Setters (avoid)

```go
type Person struct {
	firstName string
}

func CreatePerson(firstName string) *Person {
	return &Person{
		firstName: firstName,
	}
}

func (p *Person) FirstName() string {
	return p.firstName
}

func (p *Person) SetFirstName(value string) {
	p.firstName = value
}
```

## Channels

```go
type Message struct {
    content string
}

// read-only channel
func readMessages(ch <-chan Message) {
    for {
        msg := <-ch
        fmt.Printf("%s", msg.content)
    }
}

// write-only channel
func writeMessages(ch chan<- Message) {
    ch <- Message {
        content: "Hello, ",
    }
    ch <- Message {
        content: "World",
    }
}

func main() {
    ch := make(chan Message)
    // or buffered channel
    // ch := make(chan Message, 10)

    go readMessages(ch)
    writeMessages(ch)
}
```

## Receiving from channel until closed

```go
func readMessages(ch <-chan Message) {
	for msg := range ch {
		fmt.Printf("%s", msg.content)
	}
    fmt.Printf("Received all messages")
}

func writeMessages(ch chan<- Message) {
    ch <- Message {
        content: "Hello, ",
    }
    ch <- Message {
        content: "World",
    }
    close(ch)
}
```

## Receiving from miltiple channels

```go
type Message struct {
    content string
}

func readMessages(ch1 <-chan Message, ch2 <-chan Message) {
    for {
        select {
        case msg1 := <-ch1:
            fmt.Printf("%s", msg1.content)

        case msg2 := <-ch2:
            fmt.Printf("%s", msg2.content)
        }
    }
}

func writeMessages(ch1 chan<- Message, ch2 chan<- Message) {
    ch1 <- Message {
        content: "Hello, ",
    }
    ch2 <- Message {
        content: "World",
    }
}

func main() {
    ch1 := make(chan Message)
    ch2 := make(chan Message)

    go readMessages(ch1, ch2)
    writeMessages(ch1, ch2)
}
```

## "Futures"

```go
c := make(chan int, 1)
go func() { c <- process() }() // async
v := <-c                       // await
```

## Mutual exclusion

```go
import "sync"

var (
    mu      sync.Mutex // guards balance
    balance int
)

func Deposit(amount int) {
    mu.Lock()
    balance = balance + amount
    mu.Unlock()
}

func Balance() int {
    mu.Lock()
    b := balance
    mu.Unlock()
    return b
}
```

## Read file line by line

```go
file, err := os.Open("/path/to/file.txt")
if err != nil {
    log.Fatal(err)
}
defer file.Close()

scanner := bufio.NewScanner(file)
// optionally, resize scanner's capacity for lines over 64K, see next example
for scanner.Scan() {
    fmt.Println(scanner.Text())
}

if err := scanner.Err(); err != nil {
    log.Fatal(err)
}
```
