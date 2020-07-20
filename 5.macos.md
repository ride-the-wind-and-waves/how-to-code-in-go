## 如何在 MacOS 搭建本地开发环境

### 由Gopher Guides撰写

> GO 是一门编程语言，诞生对现有编程语言失望的谷歌手中。目前开发人员经常要面对的是，选择一种执行效率高但编译时间长的语言，或者选择一种易于编程但在生产中运行效率低下的语言。Go 语言旨在同时具备所有三个功能：快速编译，简便易用，并在生产中高效执行。

> 尽管Go是一种通用的编程语言，可用于不同平台的程序开发，特别适合网络/分布式系统程序，并赢得了“云语言”的美誉。 它专注于帮助现代程序员，使用强大的工具集做更多事情，消除关于格式化的争论，使格式成为语言规范的一部分，以及通过编译为单个二进制文件，易于部署。Go语言易于学习，只包含很少的关键字，对于初学者和经验丰富的开发人员而言，它都是不错的选择。

> 本教程将指导您在本地macOS机器上安装Go，并通过命令行设置编程工作区等。

### 先决条件

> 您需要一台能连接网络并具有管理权限的macOS计算机

### 步骤1 - 打开终端

> 我们将根据命令行完成大部分安装和设置，这是与计算机交互的非图形方式。 也就是说，您无需点击按钮，而是输入文字并接收计算机的反馈。 命令行（也称为shell程序）可以帮助您修改和自动化您每天在计算机上执行的许多任务，是软件开发人员必不可少的工具。

> macOS终端是可用于访问命令行UI程序。 跟其他程序一样，您可以通过以下方式找到终端。进入Finder，打开“应用程序”文件夹，然后进入实用程序文件夹。 从此处，像其他任何应用程序一样，双击终端以将其打开。 或者，您可以通过使用Spotlight，按下CMD和SPACE键，通过在框中输入它来找到终端。

![image](https://github.com/Huangjunlin/how-to-code-in-go/blob/dev/image/macOS%20Terminal.png)
macOS Terminal

> 还有很多终端命令需要学习，它可以帮助您做更强大的事情。 文章“ [Linux终端简介](https://www.digitalocean.com/community/tutorials/introduction-to-the-linux-terminal)” 可以使您更好地适应Linux终端，类似macOS终端。

> 现在，您可以打开终端，下载并安装Xcode，这是安装Go所需的一整套开发人员工具。

### 步骤2 - 安装Xcode

> Xcode是包含集成开发环境的macOS的软件开发工具集。 您可以通过以下命令检查Xcode是否已经安装。

通过在“终端”窗口中键入以下命令已安装：
```bash   
    > xcode-select -p
```
以下输出表示已安装Xcode：
```bash   
    > /Library/Developer/CommandLineTools
```

> 如果收到错误，请在打开链接 [在AppStore中安装Xcode](https://itunes.apple.com/us/app/xcode/id497799835?mt=12&ign-mpt=uo％3D2) 并接受默认选项。

> 安装成功了Xcode后，请回到“终端”窗口。 接下来，您需要安装Xcode的命令行工具程序，您可以通过键入以下命令进行安装：
```bash   
    > xcode-select --install
```

> 至此，Xcode及其命令行工具程序已完成安装，接下来我们准备安装软件包管理器 Homebrew。

### 步骤3 -  安装和配置Homebrew

> 尽管Mac OS的终端具有其他linux终端的许多功能，但他不附带软件包管理的功能。软件包管理器是软件工具的集合，这些工具可用于自动化安装，包括初始软件安装，升级和配置，并根据需要删除软件。它们将软件安装在统一的位置，并以常用格式维护系统上的所有软件包。Homebrew为macOS提供了免费的开源软件包管理系统，可简化macOS上软件的安装。

要安装Homebrew，请在“终端”窗口中输入以下内容：

```bash   
    > /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

> Homebrew是使用Ruby语言开发的，因此它将修改您计算机的Ruby路径。curl命令从指定的URL中提取脚本。该脚本将说明它将执行的操作，执行过程中会暂停以提示您进行确认。 为您提供了关于脚本将对系统执行操作的大量反馈，并为您提供了验证安装过程的机会。如果过程中提示您需要输入密码，请注意，您的输入不会显示在“终端”窗口上，但会被记录下来。 输入密码后，只需按返回键。 否则，每当系统提示您确认安装时，输入字母y表示“确认”。

> 让我们看一下与curl命令关联的参数：

* -f 或者 --fail 参数是告诉终端窗口，当服务器发生错误时，不输出服务器的错误信息
* -s 或者 --silent 参数是让curl命令静默安装，不显示进度条，并与-S 或 --show-error 参数结合使用，确保curl在失败时，显示错误信息
* -l 或者 --location 参数是让curl命令重新发送请求到新到位置。如果服务器响应，该请求已经移动到其他知道的时候，可以使用该参数

> 一旦安装过程已经完成，我们需要把Homebrew的安装路径，配置到PATH环境变量的最开头处。这样可以确保macOS通过调用Homebrew安装自动选择的工具，这些工具可能会与我们正在创建的开发环境相冲突。

> 你可以使用nano命令，创建或者打开~/.bash_profile配置文件
```bash
    > nano ~/.bash_profile
```
> 在“终端”窗口中打开文件后，编写以下内容：
```bash
    > export PATH=/usr/local/bin:$PATH
```
> 要保存更改的时候，需要按住CTRL键和字母o，出现提示时，按回车键。 然后，您可以通过按住CTRL键和字母x退出nano。

> 通过在终端中执行以下操作来激活这些更改：
```bash
    > source〜/ .bash_profile
```

> 完成此操作后，对PATH环境变量所做的更改将生效

> 您可以通过输入以下命令来确保成功安装了Homebrew：
```bash
    > brew doctor
```

> 如果此时不需要更新，则终端输出将显示为：
```bash
    > Your system is ready to brew
```

> 否则，您可能会收到警告，请运行其他命令（例如brew更新），以确保您安装的Homebrew是最新的。

> 准备好Homebrew之后，即可安装Go。

### 步骤4 -  安装GO

> 您可以使用brewbr命令搜索Homebrew来搜索所有可用的软件包。就本教程而言，您将搜索与Go相关的软件包或模块：
```bash
    > brew search golang
```

> 注意：本教程不使用brew search go，因为它会返回太多结果。因为go是一个很小的词，他可以匹配许多软件包，所以使用golang作为搜索词会更准确。在互联网上搜索与Go相关的文章时，通常也是这么用。Golang一词来自Go的域名golang.org

> 终端将输出您可以安装的列表：
```bash
    > golang golang-migrate
```
> Go将出现在列表中。 继续安装：
```bash
    > brew install golang
```

> 终端窗口将为您提供有关安装Go的过程。 安装完成可能需要几分钟

> 要检查已安装的Go版本，请键入以下内容：
```bash
    > go version
```

> 这将输出当前安装的Go版本，默认情况下，它将是最新的，稳定的Go版本。

> 将来，如果要更新Go，可以运行以下命令来首先更新Homebrew，然后再更新Go。 因为您刚刚安装了最新版本，因此现在不必这样做：
```bash
    > brew update 
    > brew upgrade golang
```

> brew update将更新Homebrew本身，以确保您具有要安装的软件包的最新信息。 brew upgrade golang会将golang软件包更新为最新版本。

> 好的更新方式可以确保您的Go版本是最新的。

> 在计算机上安装Go之后，您就可以为Go项目创建工作区了。

### 步骤5 -  创建go工作区

> 现在，您已经安装了Xcode，Homebrew和Go，可以继续创建您的编程工作区。

> Go工作区的根目录将包含两个目录：

* src：包含Go源文件的目录。 源文件是您使用Go编程语言编写的文件。 Go编译器使用源文件来创建可执行的二进制文件。
* bin：该目录包含由Go工具构建和编译的可执行文件。 可执行文件是在系统上运行并执行任务的二进制文件。 这些通常是由您的源代码或其他下载的Go源代码编译的程序。

> src子目录可能包含多个版本控制存储库（例如Git，Mercurial和Bazaar）。当程序导入第三方库时，您将看到类似github.com或golang.org的目录。如果您使用的是github.com之类的代码存储库，则还将项目和源文件放在该目录下。这允许在项目中规范地导入代码。 规范导入是指引用标准软件包的导入，例如github.com/digitalocean/godo。

> 这是典型的工作空间可能看起来像：
```bash
├── bin 
│ ├── buffalo # command executable 
│ ├── dlv # command executable 
│ └── packr # command executable 
└── src 
    └── github.com 
    └── digitalocean 
    └── godo 
        ├── .git # Git reposistory metadata 
        ├── account.go # package source
        ├── account_test.go # test source
        ├── ... 
        ├── timestamp.go 
        ├── timestamp_test.go 
        └── util 
            ├── droplet.go 
            └── droplet_test.go
```

> 从1.8版本开始，Go工作区的默认目录是用户的主目录，其中带有go子目录，即$HOME/go。

> 如果您使用的Go版本早于1.8，则最好的做法是仍将$HOME/go位置用于工作空间。

> 输入以下命令，为您在工作区中创建目录结构
```bash
    > mkdir -p $HOME/go/{bin,src}
```

> -p参数告诉mkdir命令在目录中创建所有父项（即使它们当前不存在）。使用{bin，src}为mkdir命令创建一组参数，告诉它创建bin目录和src目录。

> 这将确保以下目录结构到位：
```bash
└── $HOME 
└── go
    ├── bin 
    └── src
```

> 在Go 1.8之前，需要设置一个名为$ GOPATH的本地环境变量。尽管文档不再明确要求这样做，但由于许多第三方工具仍依赖于此变量的设置，因此仍被视为一种好的做法。

> 您可以通过将$ GOPATH添加到〜/ .bash_profile中来设置它。

> 首先，使用nano或您喜欢的文本打开〜/ .bash_profile
```bash
    > nano ~/.bash_profile
```
> 通过将以下内容添加到文件来设置$ GOPATH：
```bash
export GOPATH=$HOME/go
```

> 当Go编译并安装工具时，它将放置在$ GOPATH / bin目录中。为了方便起见，通常将工作区的/ bin子目录添加到您的PATH中 〜/ .bash_profile：
```bash
export PATH=$PATH:$GOPATH/bin
```

> 您现在应该在您的〜/ .bash_profile 文件中看到以下条目：
```bash
export GOPATH=$HOME/go 
export PATH=$PATH:$GOPATH/bin
```

> 现在，你可以通过在系统上任何地方，运行所有编译或下载的Go程序

> 要更新您的shell，请发出以下命令以全局加载您刚刚创建的变量：
```bash
    > ~/.bash_profile
```
> 您可以通过使用echo命令并检查输出来验证$ PATH是否已更新：
```bash
    > echo $PATH
```

> 您应该会看到$ GOPATH / bin出现在您的HOME目录。如果以sammy身份登录，则路径中将显示/Users/sammy/go/bin。
```bash
<^>/Users/sammy/go/bin<^>:/usr/local/sbin:/usr/local/bin:/usr/bin:/b
```

> 现在，您已经创建了工作区的根目录，并且设置$ GOPATH环境变量。将来会使用以下目录结构创建项目。这个例子假设你是使用github.com作为您的存储库：
```bash
$GOPATH/src/github.com/username/project
```
> 如果您正在使用https://github.com/digitalocean/godo
项目，请将其放在以下目录中：
```bash
$GOPATH/src/github.com/digitalocean/godo
```

> 以这种方式构建项目将使go get工具可以使用项目。 这也将有助于以后的可读性。
> 您可以使用go get命令来获取godo库，以进行验证：
```bash
    > go get github.com/digitalocean/godo
```

> 通过输入以下内容，我们可以看到它已成功下载godo软件包目录：
```bash
    > ls -l $GOPATH/src/github.com/digitalocean/godo
```

> 您将收到类似于以下的输出：
```bash
-rw-r--r-- 1 sammy staff 2892 Apr 5 15:56 CHANGELOG.md 
-rw-r--r-- 1 sammy staff 1851 Apr 5 15:56 CONTRIBUTING.md
...
-rw-r--r-- 1 sammy staff 4893 Apr 5 15:56 vpcs.go 
-rw-r--r-- 1 sammy staff 4091 Apr 5 15:56 vpcs_test.go
```
> 在此步骤中，您创建了Go工作区并配置了必要的环境变量。 在下一步中，您将使用一些代码来测试工作区。

### 步骤6 -  创建一个简单程序

> 现在，您已经设置好Go工作区，现在该创建一个简单的“ Hello，World！”程序了。这将确保您的工作区正常工作，并可以让你对Go更加熟悉。

> 因为您正在创建单个Go源文件，而不是实际的项目，则无需在工作区中执行此操作。

> 在主目录中，打开一个命令行文本编辑器，例如nano，并创建一个新文件：
```bash
    > nano hello.go
```
> 在终端中打开文本文件后，输入程序代码：
```go
package main 

import "fmt" 

func main() { 
    fmt.Println("Hello, World!") 
}
```
> 通过输入控件和x键退出nano，然后在提示您保存文件时按y。

> 此代码将使用fmt包并调用Println函数，Hello, World! 作为参数。 这将把Hello，World在程序运行时打印到终端。

> 退出nano编辑器并返回shell，请运行程序：

```bash
    > go run hello.go
```

> 您刚刚创建的hello.go程序将输出：
```bash
Hello, World!
```

> 在此步骤中，您使用了基本程序来验证Go工作区是否已正确配置。

> 恭喜你！当前，您已经在本地macOS机器上设置了Go编程工作区，可以开始编码项目！