## [理解 Go 中的字典类型](https://www.digitalocean.com/community/tutorials/understanding-maps-in-go)

由 **Gopher Guides** 编写

> 大部分现代编程语言都有*字典*和*哈希*的概念，这些类型通常被用来存储**键**和**值**对应的数据。

> 在 Go 中，map 数据类型被大多数程序员认为是*字典*类型。它将键与值映射形成键值对，这在 Go 中是一种很有用的保存数据的方式。关键字 `map` 后面加上方括号 `[ ]` ，方括号里面是键的数据类型，然后后面再加上值的数据类型，最后将键值对放在花括号 `{ }` 中：

```go
map[key]value{}
```

在 Go 中通常使用 map 类型来保存相关联的数据，例如 ID 卡中的信息。map 类型的数据类似于下面：

```go
map[string]string{"name": "Sammy", "animal": "shark", "color": "blue", "location": "ocean"}
```

> 除了花括号外，map 类型数据中还有连接键值对的冒号。冒号的边做是键，在 Go 中键可以是任何可以比较的类型，例如 `strings`，`ints` 等。

> 示例中的键分别是：

- `"name"`
- `"animal"`
- `"color"`
- `"location"`

> 冒号右边的是值，值可以是任何数据类型。上述示例中的值分别是：

- `"Sammy"`
- `"shark"`
- `"blue"`
- `"ocean"`

> 跟其他类型一样，也可以将 map 类型的数据赋值给一个变量，然后将它打印出来：

```go
sammy := map[string]string{"name": "Sammy", "animal": "shark", "color": "blue", "location": "ocean"}

fmt.Println(sammy)
```

> 将打印出：

```go
map[animal:shark color:blue location:ocean name:Sammy]
```

> 打印出的键值对顺序可能有变化，因为在 Go 中，map 类型的数据是无序的。无论顺序是否变化，键值的对应关系不会改变，从而可以根据映射关系访问到指定的数据。

## 访问 Map 中的某项

> 您可以通过关联关系访问 map 中的值，因为 map 提供了一种通过键值对保存数据的方式，所以在 Go 编程中是非常重要和有用的。

> 您可以访问 `sammy["name"]` 来获取 Sammy 的姓名，sammy 变量保存了对应的键值，可以打印出：

```go
fmt.Println(sammy["name"])
```

> 结果是：

```go
Sammy
```

> map 就像数据库一样，您不必像使用切片那样使用整数来获取特定的索引值，而是将值赋给键然后根据键得到它关联的值。

> 通过调用键 `"name"` ，您得到了它的值 `"Sammy"`。

> 您也可以使用同样的方式来获取 `sammy` 中的其他值：

```go
fmt.Println(sammy["animal"])
// 返回 shark

fmt.Println(sammy["color"])
// 返回 blue

fmt.Println(sammy["location"])
// 返回 ocean
```

> 通过使用 map 数据类型中的键值对，您可以用键来搜索想要的值。

## 键和值

> 与一些其他编程语言不通，Go 没有提供任何简便的方法来直接获取map中的键和值，比如Python的 `.keys()` 方法。但是 Go 允许使用 `range` 运算符进行迭代：

```go
for key, value := range sammy {
    fmt.Printf("%q is the key for the value %q\n", key, value)
}
```

> 在 Go 中遍历一个 map类型的数据，会返回两个值。第一个是键，第一个是对应的值。Go 创建这些值的时候，会自动分配正确的数据类型。在上面的例子中，map 中键的类型是 `string`，所以 `key` 也是一个字符串类型，值也是字符串：

```go
"animal" is the key for the value "shark"
"color" is the key for the value "blue"
"location" is the key for the value "ocean"
"name" is the key for the value "Sammy"
```

> 如果只想要获取键列表，可以使用范围运算符。您可以只声明一个变量来访问键：

```go
keys := []string{}

for key := range sammy {
		keys = append(keys, key)
}
fmt.Printf("%q", keys)
```

> 上面程序先声明了一个切片来保存键。

> 输出的结果中将只有 map 中的键：

```go
["color" "location" "name" "animal"]
```

> 同样的，键是无序的，如果要对它们进行排序，可以使用 [`sort`](https://golang.org/pkg/sort)  包中的 `sort.Strings` 方法：

```go
sort.Strings(keys)
```

输出：

```go
["animal" "color" "location" "name"]
```

> 您可以使用相同的方式只获取映射中的值。在下个例子中，预先声明了一个切片从而避免了自动分配内存空间，使得程序加更高效：

```go
sammy := map[string]string{"name": "Sammy", "animal": "shark", "color": "blue", "location": "ocean"}

items := make([]string, len(sammy))

var i int

for _, v := range sammy {
    items[i] = v
    i++
}
fmt.Printf("%q", items)
```

> 首先，声明了一个切片来存储您的键；因为知道需要多少个子项，所以可以通过定义相同长度的切片来避免潜在的内存分配。然后声明索引变量，因为不需要键，可以使用运算符 `_` 在开始循环时忽略键的值。输出如下：

```go
["ocean" "Sammy" "shark" "blue"]
```

> 要确定 map 中有多少项，可以使用内建函数 `len` ：

```go
sammy := map[string]string{"name":"Sammy","animal":"shark","color":"blue","location": "ocean"}
fmt.Println(len(sammy))
```

> 输出结果为：

```
4
```

> 虽然 Go 没有提供直接的方法来获取键和值，但是我们只需要用几行代码就可以得到想要的结果。

## 检查是否存在

> 当查询的键不存在时，Go 中的 map 会返回一个零值。因此需要使用一种替代的方法来区分零和不存在的键。

> 查询一个已知不在 map 中的键时会返回下面的值：

```go
counts := map[string]int{}
fmt.Println(counts["sammy"])
```

> 您会看到下面的输出：

```go
0
```

> 即使在 map 中不存在键 `sammy` ，Go 依然返回了 `0` 。因为 map 中的数据类型是 `int` ，Go 会将所有的变量默认为 0，所以返回了零值 `0` 。

> 在很多情况下，这不是我们预想的并且可能导致程序出错。当在 Go 中查找一个值时，可以返回第二个*可选* 值。第二个值是 `布尔` 类型，当被找到时会返回 `true` ，否则返回 `false` ，在 Go 中这被称为 `ok` 范式。尽管可以给获取到的第二个变量任意命名，在 Go 中总是将它命名为 `ok` ：

```go
count, ok := counts["sammy"]
```

> 如果 `counts` 中存在键 `sammy` ，`ok`  就为 `true` ，否则为 `false`。

> 您可以使用 `ok`  变量来决定程序要做什么：

```go
if ok {
    fmt.Printf("Sammy has a count of %d\n", count)
} else {
    fmt.Println("Sammy was not found")
}
```

> 输出结果：

```go
Sammy was not found
```

> 在 Go 中，您可以使用 `if/else` 将变量声明和条件判断合并起来。这样就可以使用单个语句进行检测：

```go
if count, ok := counts["sammy"]; ok {
    fmt.Printf("Sammy has a count of %d\n", count)
} else {
    fmt.Println("Sammy was not found")
}
```

> 在 Go 中从 map 中查找某个值时，最好同时检查该值是否存在，以避免程序中出错。

## 修改 Map

> Map 是一种可变的数据结构，因此可以对其进行修改。在本节中我们将添加和修改 map 中的项。

### 添加和更改 Map 中的数据

> 无需使用方法或者函数，您就可以往 map 中添加键值对。您可以使用 map 变量名称，后跟方括号 `[ ]` 中写入键的名称，然后使用等号 `=`  设置新值：

```go
map[key] = value
```

> 下面代码将添加一个新的键值对到 `usernames` 中：

```go
usernames := map[string]string{"Sammy": "sammy-shark", "Jamie": "mantisshrimp54"}

usernames["Drew"] = "squidly"
fmt.Println(usernames)
```

> map 中新的键值对 `Drew:squidly` 将会被打印出来：

```go
[Drew:squidly Jamie:mantisshrimp54 Sammy:sammy-shark]
```

> 新增的键值对可能在任意位置，因为 map 的返回值是无序的。如果在后面的代码中使用 `usernames` ，它将包含该键值对。

> 您也可以使用这种格式修改 map 中某个键的值。在这种情况下，可以将一个不同的值传递给已经存在的键。

> 我们用一个命名为 `followers` 的 map 定义网络上某用户的粉丝数量。用户 `drew` 的粉丝量今天有变化，所以我们需要更新键 `drew` 对应的数值。然后我们用 `Println()` 函数检查一下该 map 是否修改成功：

```go
followers := map[string]int{"drew": 305, "mary": 428, "cindy": 918}
followers["drew"] = 342
fmt.Println(followers)
```

> `drew` 将输出更新后的值：

```go
map[cindy:918 drew:342 mary:428]
```

> 可以看到粉丝数量从 `305` 涨到了 `342` 。

> 您可以使用此方法将用户输入的键值对添加到指定的 map 中。让我们编写一个 `username.go` 的程序然后在命令行运行，该程序允许用户添加更多的名称并关联到 `usernames` 中：

​																	*usernames.go*

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    usernames := map[string]string{"Sammy": "sammy-shark", "Jamie": "mantisshrimp54"}

    for {
        fmt.Println("Enter a name:")

        var name string
        _, err := fmt.Scanln(&name)

        if err != nil {
            panic(err)
        }

        name = strings.TrimSpace(name)

        if u, ok := usernames[name]; ok {
            fmt.Printf("%q is the username of %q\n", u, name)
            continue
        }

        fmt.Printf("I don't have %v's username, what is it?\n", name)

        var username string
        _, err = fmt.Scanln(&username)

        if err != nil {
            panic(err)
        }

        username = strings.TrimSpace(username)

        usernames[name] = username

        fmt.Println("Data updated.")
    }
}
```

> 在 `usernames.go` 中，我们先定义了一个原始的 map，然后用一个 `for` 循环遍历名称，要求用户输入一个名字并且声明一个存储该名字的变量。然后检查是否出错，如果报错了程序会以 *panic* 退出。因为 `Scanln` 会捕获包括回车的所有输入内容，所以您需要从输入中删除空格；也可以使用`strings.TrimSpace` 函数来执行此操作。

> `if`  用来检测这个名字是否存在并打印出结果。如果该名字存在，则返回循环顶部；否则将告知用户输入关联的名字，然后程序检查是否有错，如果没有错误就会过滤回车符，将名字分配给名称键，然后输出有关数据已经更新的反馈。

> 让我们在命令行运行这个程序：

```shell
$ go run usernames.go
```

> 您将会看到下面的输出：

```
Enter a name:
Sammy
"sammy-shark" is the username of "Sammy"
Enter a name:
Jesse
I don't have Jesse's username, what is it?
JOctopus
Data updated.
Enter a name:
```

> 完成测试后，按 `CTRL + C` 退出程序。

> 上面展示了如何交互的去修改 map，在这个程序中，一旦您使用 `CTRL + C` 退出程序，所有的数据都将会丢失，除非您实现了一种读写文件的方法。

> 总而言之，您可以用 `map[key] = value` 的格式来添加或者修改 map 的数据。

### 删除 Map 中的数据

> 就像您可以添加键值对和修改 map 类型数据中的值一样，您也可以删除其中的数据。

> 为了删除 map 中的键值对，您可以使用内建函数 `delete()`。 第一个参数是要删除的 map 名称，第二个参数是要删除的键：

```go
delete(map, key)
```

> 让我们定义一个存储权限的 map

```go
permissions := map[int]string{1: "read", 2: "write", 4: "delete", 8: "create", 16:"modify"}
```

> 您不再需要 `modify` 权限，所以需要从 map 中将其删除。然后可以打印出这个 map 以确认它已经被删除：

```go
permissions := map[int]string{1: "read", 2: "write", 4: "delete", 8: "create", 16: "modify"}
delete(permissions, 16)
fmt.Println(permissions)
```

> 输出确认删除：

```go
map[1:read 2:write 4:delete 8:create]
```

> `delete(map, 16)` 会将键值对  `16:modify`  从 `permissions` 中删除。

> 如果想要删除 map 中的所有元素，可以直接将其设为相同类型的空值。这将会创建一个新的空映射，并且垃圾回收器会从内存中将旧的映射清除。

> 让我们来删除 `permissions` 中的所有数据：

```go
permissions = map[int]string{}
fmt.Println(permissions)
```

> 输出表明 `permissions`已经成为一个没有键值对的空映射 :

```go
map[]
```

> 由于 map 是一种可变的数据类型，因此您可以添加，修改，删除以及清空其中的元素。

## 结论

> 本教程探讨了 Go 中的 map 类型数据结构。Map 由键值对组成，并提供了一种无需依赖索引即可存储数据的方法。这使我们能够根据值的含义以及和其他数据类型的关联关系来查找想要的值。

