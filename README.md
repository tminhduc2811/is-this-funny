# is-this-funny
The answer is

## Terminal apps

- [Bubbletea](https://github.com/charmbracelet/bubbletea)
- [Lipgloss](https://github.com/charmbracelet/lipgloss)

## Golang

### Handling concurrency
- Append to same slice outside a go-routine

Approach 1

```go

MySlice = make([]*MyStruct, len(params))
for i, param := range params {
    wg.Add(1)
    go func(i int, param string) {
         defer wg.Done()
         OneOfMyStructs := getMyStruct(param)
         MySlice[i] = &OneOfMyStructs
     }(i, param)
}
```

Approach 2

```go
import "fmt"
import "sync"

type T int

func main() {
    var slice []T
    var wg sync.WaitGroup

    queue := make(chan T, 1)

    // Create our data and send it into the queue.
    wg.Add(100)
    for i := 0; i < 100; i++ {
        go func(i int) {
            // defer wg.Done()  <- will result in the last int to be missed in the receiving channel
            queue <- T(i)
        }(i)
    }

    go func() {
        // defer wg.Done() <- Never gets called since the 100 `Done()` calls are made above, resulting in the `Wait()` to continue on before this is executed
        for t := range queue {
            slice = append(slice, t)
            wg.Done()   // ** move the `Done()` call here
        }
    }()

    wg.Wait()

    // now prints off all 100 int values
    fmt.Println(slice)
}
```
## Books and Articles

### Articles
[System Design Cheat Sheet](https://vivek-singh.medium.com/system-design-cheat-sheet-318ba2e34723)

### Books
[Best Practices for Event-Driven Microservice Architecture by @jason](https://hackernoon.com/best-practices-for-event-driven-microservice-architecture-e034p21lk)

[Design Patterns: Elements of Reusable Object-Oriented Software](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented-ebook/dp/B000SEIBB8)

[Enterprise Integration Patterns: Designing, Building, and Deploying Messaging Solutions](https://www.amazon.com/Enterprise-Integration-Patterns-Designing-Deploying/dp/0321200683)

[Growing Object-Oriented Software, Guided by Tests](https://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627)

[Agile Software Development, Principles, Patterns, and Practices](https://www.amazon.com/exec/obidos/ASIN/0135974445/objectmentorinc)
