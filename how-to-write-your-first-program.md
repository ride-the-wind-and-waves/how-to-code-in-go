## 如何在Go中编写第一个程序

### 由Gopher Guides撰写

>”Hello，World“ 程序是计算机编程中的一种经典且悠久的传统。 这是面向初学者的简单而完整的第一个程序，也是确保正确配置环境的好方法。

>本教程将引导您完成在Go中创建该程序的过程。 但为了使程序更有趣，您将修改传统的 ”Hello，World！“ 程序，以便它询问用户的姓名。 然后，您将在问候语中使用该名称。 学习完本教程后，您将拥有一个可用于交互的程序，如下所示：

输出：

```bash   
    > "Please enter your name."
    > Sammy
    > "Hello, Sammy! I'm Go!"
```
### 先决条件

>在开始本教程之前，您将需要在计算机上设置本地Go开发环境。 您可以按照以下教程之一参照自己的操作系统进行设置：

- [如何在 MacOS 搭建本地开发环境](https://github.com/xueyuanjun/how-to-code-in-go/blob/master/macos.md)
- [如何在 Ubuntu 搭建本地开发环境](https://github.com/xueyuanjun/how-to-code-in-go/blob/master/ubuntu.md)
- [如何在 Windows10 搭建本地开发环境](https://github.com/xueyuanjun/how-to-code-in-go/blob/master/windows.md)

### 步骤1 - 编写基本的 "Hello World" 程序
>编写 "Hello，World" 程序，打开命令行文本编辑器（例如nano）并创建一个新文件：

` nano hello.go `

>一旦文本文件在终端窗口中打开，您将输入程序：

```go
package main

import  "fmt"

func main() {
    fmt.Println("Hello, World!")
}

```
>让您拆分一下这段代码的不同组成部分。
>package是一个Go关键字，用于定义此文件所属的包。 每个文件夹只能有一个包，每个.go文件必须在其文件顶部声明相同的包名称。 这段示例代码属于main包。

>import是一个Go关键字，它告诉Go编译器要在此文件中使用哪些其他包。 在这里，您可以导入标准库自带的fmt包。 fmt包提供了格式化和打印功能，这些功能在开发时将非常有用。

>fmt.Println是fmt包中的Go函数，它告诉计算机在屏幕上打印一些文本。

>在fmt.Println函数后面加上引号引起的一系列字符，例如“ Hello，World！”。 引号内的任何字符都称为字符串。 程序运行时，fmt.Println函数会将此字符串打印到屏幕上。

>通过键入CTRL + X保存并退出nano，当提示您保存文件时，按Y。

>现在可以尝试运行您的程序了。

### 步骤2 — 运行Go程序

>当您的程序“ Hello，World！”完成编写，就可以开始运行该程序了。 您将使用go命令，在后面接刚创建的文件名称。

>go run hello.go

>程序将执行并显示以下输出：

输出：

``Hello, World!``

>Ok，让您探索一下实际上发生的事情。

>Go程序需要在运行之前进行编译。 当您使用go run调用该文件时（在本例中为hello.go），go命令将编译应用程序，然后运行所生成的二进制文件。 对于用编译型语言所编写的程序，编译器将获取程序的源代码，并生成另一种较低级别的代码（例如机器代码）以生成可执行程序。

>Go应用程序需要一个主程序包和一个 main() 函数作为该应用程序的入口点。 main函数不接受任何参数，也不返回任何值。 相反，它告诉Go编译器这个包应编译为可执行包。

>编译后，通过在main包中输入main() 函数来执行代码。 它通过调用fmt.Println函数执行fmt.Println("Hello，World!")。然后 ，”Hello,World！“字符串的值将传递给该函数。 在此示例中，字符串 ”Hello，World！“ 也称为参数，因为它是传递给方法的值。

>而 Hello，World两侧的引号！ 则不会打印到屏幕上，是因为您使用它来告诉Go编译器字符串开始和结束的位置。

>在此步骤中，您已经创建了一个有效的“Hello，World！” Go程序。 在下一步中，您将探索如何使程序更具交互性。

### 步骤3 — 提示用户输入

>每次运行程序时，它都会产生相同的输出。 在此步骤中，您可以将其添加到程序中，用来提示用户输入其名称。 然后，您将在输出中使用它们的名称。

>无需修改现有程序，而是使用nano编辑器创建一个名为greeting.go的新程序：

``nano greeting.go``

>首先，添加以下代码，提示用户输入其名称：

```go
package main

import (
    "fmt"
)

func main() {
    fmt.Println("Please enter your name.")
}
```

>再次使用fmt.Println函数将文本打印到屏幕上。

>另起一行存储用户的输入：

greeting.go
```go

package main

import (
    "fmt"
)

func main() {
    fmt.Println("Please enter your name.")
    var name string
}

```
>var name string 使用var关键字创建出一个新的变量。 而且这个被命名的变量name是个字符串类型。

>然后，另起一行捕获用户的输入：
                                    greeting.go
```go
package main

import (
    "fmt"
)

func main() {
    fmt.Println("Please enter your name.")
    var name string
    fmt.Scanln(&name)
}
```

>fmt.Scanln方法告诉计算机等待键盘输入以新行或（\ n）字符结尾的输入。 这将暂停程序，使用户可以输入所需的任何文本。 当用户按键盘上的ENTER键时，程序将继续。 然后捕获所有按键，包括ENTER按键，并将其转换为字符串。

>您想在程序的输出中使用这些字符，因此可以通过将它们写入名为name的字符串变量中来保存这些字符。 Go将该字符串存储在计算机内存中，直到程序运行完毕。

>最后，在代码中另起一行继续打印输出：

greeting.go
```go
package main

import (
    "fmt"
)

func main() {
    fmt.Println("Please enter your name.")
    var name string
    fmt.Scanln(&name)
    fmt.Printf("Hi, %s! I'm Go!", name)
}
```

>这次，您不再使用fmt.Println方法，而是使用fmt.Printf。 fmt.Printf函数采用字符串，并使用特殊的打印动词（％s）将name的值注入字符串中。 这样做是因为Go不支持字符串插值，这将允许您获取分配给变量的值并将其放入字符串中。  

>通过按CTRL + X保存并退出nano，然后在提示保存文件时按Y。

>现在运行程序。 系统将提示您输入名称，您输入并按下ENTER键。 输出结果也许与您期望的不完全相同：

```bash
    >"Please enter your name."
    >Sammy
    >"Hi, Sammy! I'm Go!"
```

>“Hi, Sammy! I'm Go!” Sammy后面有一个换行符。

>该程序捕获了您所有的按键，包括您按下以告知程序继续的ENTER键。 在字符串中，按Enter键将创建一个特殊字符，该字符将创建新行。 该程序的输出完全按照您的指示执行。 它会显示您输入的文本，包括新行。 这不是您期望的输出，但是您可以使用其他功能对其进行修复。

>在编辑器中打开 文件greeting.go

``nano greeting.go``

>在程序中找到以下代码：

```go
...
fmt.Scanln(&name)
...
```

>在其后添加以下代码：

greeting.go
```go
name = strings.TrimSpace(name)
```

>使用Go的标准库字符串包中的TrimSpace函数对您使用fmt.Scanln捕获的字符串进行处理。 strings.TrimSpace函数从字符串的开头和结尾删除所有空格字符，包括换行符。 在这种情况下，它将在按ENTER键时创建的字符串末尾删除换行符。

greeting.go

```go
import (
    "fmt"
)
```

>添加以下代码以导入字符串包：

greeting.go
```go
import (
    "fmt"
    "strings"
)
```

>您的完整代码现在将包含以下内容：

greeting.go
```go
package main

import (
        "fmt"
        "strings"
)

func main() {
    fmt.Println("Please enter your name.")
    var name string
    fmt.Scanln(&name)
    fmt.Printf("Hi, %s! I'm Go!", name)
    name = strings.TrimSpace(name)
}
```

>保存并退出nano。 按CTRL + X，然后在提示保存文件时按Y。

>再次运行程序：
``go run greeting.go``

>这次，在输入名称并按Enter之后，您将获得预期的输出：

输出：

```bash
    >"Please enter your name."
    >Sammy
    >"Hi, Sammy! I'm Go!"
```

>现在，您有了一个Go程序，该程序可以接受用户的输入并将其打印回屏幕。

### 结论

>在本教程中，您写了一个“ Hello，World！”。 从用户处获取输入，处理结果并显示输出的程序。 现在您已经可以使用一个基本程序，请尝试进一步扩展程序。 例如，询问用户喜欢的颜色，然后让程序说用户喜欢的颜色是红色。 您甚至可以尝试使用相同的技术来创建一个简单的Mad-Lib程序。