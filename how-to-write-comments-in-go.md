## 在Go中如何写注释

### 由Gopher Guides撰写

注释是存在于计算机程序中的被编译器和解释器忽略的行。在程序中包含注释可以提高代码的可读性，因为它提供了有关程序的每个部分在做什么的一些信息或解释。

根据您程序的目的，注释可以作为您自己的注释或提醒，也可以让其他程序员理解您的代码在做什么。

一般来说，在编写或更新程序时写注释是个好习惯，因为以后很容易忘记自己的思维过程，而以后写的注释从长远来看可能用处不大。

### 注释语法

Go中的注释以正双斜杠 (`//`) 开头，添加后这一行都会被注释。 在斜杠之后一般留一个空格。
通常，注释将如下所示：

```go
// This is a comment
```

注释不会执行，因此在运行程序时不会有注释的指示。源代码中的注释是供人阅读的，而不是供计算机执行的。

在“`Hello,World!`”程序中，注释如下:

hello.go

```go
package main

import (
    "fmt"
)

func main() {
    // Print “Hello, World!” to console
    fmt.Println("Hello, World!")
}
```

在`for`循环切片代码中，注释如下：

**sharks.go**

```go
package main

import (
    "fmt"
)

func main() {
    // Define sharks variable as a slice of strings
    sharks := []string{"hammerhead", "great white", "dogfish", "frilled", "bullhead", "requiem"}

    // For loop that iterates over sharks list and prints each string item
    for _, shark := range sharks {
        fmt.Println(shark)
    }
}
```

注释应该在与其注释的代码相同的缩进位置进行。也就是说，没有缩进的函数定义将有一个没有缩进的注释，接下来的每个缩进级别将有与其注释的代码对齐的注释。

例如，在如下`main`主函数中注释，在代码的每个缩进级别之后都有注释：

color.go

```go
package main

import "fmt"

const favColor string = "blue"

func main() {
    var guess string
    // Create an input loop
    for {
        // Ask the user to guess my favorite color
        fmt.Println("Guess my favorite color:")
        // Try to read a line of input from the user. Print out the error 0
        if _, err := fmt.Scanln(&guess); err != nil {
            fmt.Printf("%s\n", err)
            return
        }
        // Did they guess the correct color?
        if favColor == guess {
            // They guessed it!
            fmt.Printf("%q is my favorite color!\n", favColor)
            return
        }
        // Wrong! Have them guess again.
        fmt.Printf("Sorry, %q is not my favorite color. Guess again.\n", guess)
    }
}
```

提供注释是为了帮助程序员，无论是最初的程序员，还是在项目中使用或合作的其他人。如果注释不能与代码库一起适当地维护和更新，最好不要包含注释，而不要编写与代码相矛盾或将与代码相矛盾的注释。

在注释代码时，您应该寻求回答代码背后的原因，而不是内容或方式。 除非代码特别棘手，否则查看代码通常可以回答“什么”或“如何”，这就是为什么注释通常围绕“为什么”的原因。

### 块注释

块注释可以用来解释更复杂的代码或您不希望读者熟悉的代码。

您可以在Go中以两种方式创建块注释。 第一种是使用一组双正斜杠，在需要注释的地方添加双斜杠。

```go
// First line of a block comment
// Second line of a block comment
```

第二种是使用开始标记（`/ *`）和结束标记（`* /`）。 对于文档代码，始终使用`//`用于注释。 调试只会使用`/ * ... * /`语法，我们将在本文后面介绍。

```go
/*
Everything here
will be considered
a block comment
*/
```

在此示例中，块注释定义了`MustGet（）`函数：

function.go

```go
// MustGet will retrieve a url and return the body of the page.
// If Get encounters any errors, it will panic.
func MustGet(url string) string {
    resp, err := http.Get(url)
    if err != nil {
        panic(err)
    }

    // don't forget to close the body
    defer resp.Body.Close()
    var body []byte
    if body, err = ioutil.ReadAll(resp.Body); err != nil {
        panic(err)
    }
    return string(body)
}
```

在Go中，导出函数开头的块注释很常见；这些注释也会生成代码文档。块注释也用于操作不太直接，因此需要一个完整的解释。除了为函数编写文档外，您应该尽量避免过多注释代码，并相信其他程序员能够理解Go，除非您是为特定的读者编写的。

### 内联注释

内联注释出现在语句的同一行，在代码本身后面。与其他注释一样，它们以一组正斜杠开始。通常的习惯在前斜杠后面不需要有空格。

一般来说，内联注释是这样的:

```go
[code]  // Inline comment about the code
```

内联注释应该谨慎使用，但对于解释代码中棘手的或不明显的部分可能是有效的。如果您认为将来可能记不住自己正在编写的代码中的一行，或者您正在与可能不熟悉代码各方面的人合作，那么它们也会很有用。

例如，如果你不使用很多数学计算在你的Go程序，你或你的合作者可能不知道下面创建了一个复数，所以你需要创建一个内联注释:

```go
z := x % 2  // Get the modulus of x
```

你也可以使用内嵌注释来解释某事背后的原因，或者提供一些额外的信息，如:

```go
x := 8  // Initialize x with an arbitrary number
```

您应该只在必要时使用内联注释，并且它们可以为阅读程序的人提供有用的指导。

### 通过注释进行测试

除了使用注释作为记录代码的方法外，还可以使用开始标记(`/*`)和结束标记(`*/`)来创建块注释。这允许您注释掉在测试或调试当前创建的程序时不想执行的代码。也就是说，当您在实现新代码行之后遇到错误时，您可能需要注释掉其中的一些代码，以查看是否可以精确地解决问题。

使用`/*`和`*/`标记还可以让您在决定如何设置代码时尝试其他方法。您还可以使用块注释来注释掉失败的代码，同时继续处理代码的其他部分。

multiply.go

```go
// Function to add two numbers
func addTwoNumbers(x, y int) int {
    sum := x + y
    return sum
}

// Function to multiply two numbers
func multiplyTwoNumbers(x, y int) int {
    product := x * y
    return product
}

func main() {
    /*
        In this example, we're commenting out the addTwoNumbers
        function because it is failing, therefore preventing it from executing.
        Only the multiplyTwoNumbers function will run

        a := addTwoNumbers(3, 5)
        fmt.Println(a)

    */

    m := multiplyTwoNumbers(5, 9)
    fmt.Println(m)
}
```

> **Note**: 注释掉代码只能用于测试目的。不要在最终的程序中留下注释掉的代码片段。

使用`/*`和`*`/标记注释代码可以让您尝试不同的编程方法，还可以通过系统地注释和运行程序的某些部分来帮助您找到错误的根源。

### 结论

在您的Go程序中使用注释有助于使您的程序对人类(包括将来的您自己)更具可读性。添加适当的相关和有用的注释可以使其他人更容易与您协作编程项目，并使您的代码的价值更加明显。

在Go中正确地注释代码还允许您使用Godoc工具。Godoc是从代码中提取注释并为Go程序生成文档的工具。