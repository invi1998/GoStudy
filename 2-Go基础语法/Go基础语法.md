# 关键字、标识符、注释，基础结构

## GO中保留关键字只有25个

| break    | default     | func   | interface | select |
| -------- | ----------- | ------ | --------- | ------ |
| case     | defer       | go     | map       | struct |
| chan     | else        | goto   | package   | switch |
| const    | fallthrough | if     | range     | type   |
| continue | for         | import | return    | var    |

## GO中36个预定义的标识符，其包括基础数据结构类型和系统内嵌函数

| append    | bool       | byte   | cap   | close   | complex |
| --------- | ---------- | ------ | ----- | ------- | ------- |
| float64   | complex128 | uint16 | copy  | false   | float32 |
| complex64 | imag       | int    | int8  | int16   | uint32  |
| int32     | int64      | iota   | len   | make    | new     |
| nil       | panic      | uint64 | print | println | real    |
| recover   | string     | TRUE   | uint  | uint8   | uintprt |

## 注释形式

- // 单行注释
- /**/ 多行注释
- 一般使用多行注释

## 基础结构

```go
//程序所属的包（表示本go文件属于main包，每一个.go文件必须要有一个package关键字，而且必须要在开头）
package main

//导入依赖包
import "fmt"

//常量定义
const NAME string = "imooc"

//全局比那辆的声明与赋值
var mainName string = "main name"

//一般类型声明
type imoocInt int

//结构的声明
type Learn struct {
	id   imoocInt
	name string
}

//声明接口(接口声明，接口名一般建议以I开头)
type Ilearn interface {
}

//函数定义
func learnImooc() {
	fmt.Print("learnImooc")
}

//main()主函数
func main() {
	fmt.Println("helle world")
	fmt.Print(mainName)
	fmt.Print(NAME)
	learnImooc()
}
```



# package、import别名，路径，"."，"_"的使用说明

## Package包的用法

- 在GO语言中，package是最基本的分发单位和工程管理中依赖关系的体现
- 每个GO语言源代码文件开头都拥有一个package声明，表示源码文件所属的代码包
- 要生成GO语言可执行程序，必须要有main的package包，而且必须在该包下面有main()函数
- 同一个路径下只能存在一个package，一个package可以拆解成多个源文件组成



包是结构化代码的一种方式：每个程序都由包（通常简称为 pkg）的概念组成，可以使用自身的包或者从其它包中导入内容。

如同其它一些编程语言中的类库或命名空间的概念，每个 Go 文件都属于且仅属于一个包。一个包可以由许多以 .go 为扩展名的源文件组成，因此文件名和包名一般来说都是不相同的。

你必须在源文件中非注释的第一行指明这个文件属于哪个包，如：package main。package main表示一个可独立执行的程序，每个 Go 应用程序都包含一个名为 main 的包。

一个应用程序可以包含不同的包，而且即使你只使用 main 包也不必把所有的代码都写在一个巨大的文件里：你可以用一些较小的文件，并且在每个文件非注释的第一行都使用 package main 来指明这些文件都属于 main 包。如果你打算编译包名不是为 main 的源文件，如 pack1，编译后产生的对象文件将会是 pack1.a 而不是可执行程序。另外要注意的是，所有的包名都应该使用小写字母。

**标准库**

在 Go 的安装文件里包含了一些可以直接使用的包，即标准库。在 Windows 下，标准库的位置在 Go 根目录下的子目录 pkg\windows_386 中；在 Linux 下，标准库在 Go 根目录下的子目录 pkg\linux_amd64 中（如果是安装的是 32 位，则在 linux_386 目录中）。一般情况下，标准包会存放在 $GOROOT/pkg/$GOOS_$GOARCH/ 目录下。

Go 的标准库包含了大量的包（如：fmt 和 os），但是你也可以创建自己的包。

如果想要构建一个程序，则包和包内的文件都必须以正确的顺序进行编译。包的依赖关系决定了其构建顺序。

属于同一个包的源文件必须全部被一起编译，一个包即是编译时的一个单元，因此根据惯例，每个目录都只包含一个包。

**如果对一个包进行更改或重新编译，所有引用了这个包的客户端程序都必须全部重新编译。**

Go 中的包模型采用了显式依赖关系的机制来达到快速编译的目的，编译器会从后缀名为 .o 的对象文件（需要且只需要这个文件）中提取传递依赖类型的信息。

如果 A.go 依赖 B.go，而 B.go 又依赖 C.go：

编译 C.go, B.go, 然后是 A.go.
为了编译 A.go, 编译器读取的是 B.o 而不是 C.o.
这种机制对于编译大型的项目时可以显著地提升编译速度。

**每一段代码只会被编译一次**

一个 Go 程序是通过 import 关键字将一组包链接在一起。

import "fmt" 告诉 Go 编译器这个程序需要使用 fmt 包（的函数，或其他元素），fmt 包实现了格式化 IO（输入/输出）的函数。包名被封闭在半角双引号 "" 中。如果你打算从已编译的包中导入并加载公开声明的方法，不需要插入已编译包的源代码。

如果需要多个包，它们可以被分别导入：

```go
import "fmt"
import "os"
```


或：

```go
import "fmt"; import "os"
```


但是还有更短且更优雅的方法（被称为因式分解关键字，该方法同样适用于 const、var 和 type 的声明或定义）：

```go
import (
   "fmt"
   "os"
)
```


它甚至还可以更短的形式，但使用 gofmt 后将会被强制换行：

```go
import ("fmt"; "os")
```


当你导入多个包时，导入的顺序会按照字母排序。

如果包名不是以 . 或 / 开头，如 "fmt" 或者 "container/list"，则 Go 会在全局文件进行查找；如果包名以 ./ 开头，则 Go 会在相对目录中查找；如果包名以 / 开头（在 Windows 下也可以这样使用），则会在系统的绝对路径中查找。

导入包即等同于包含了这个包的所有的代码对象。

除了符号 _，包中所有代码对象的标识符必须是唯一的，以避免名称冲突。但是相同的标识符可以在不同的包中使用，因为可以使用包名来区分它们。

包通过下面这个被编译器强制执行的规则来决定是否将自身的代码对象暴露给外部文件：

**可见性规则**

当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 private ）。

（大写字母可以使用任何 Unicode 编码的字符，比如希腊文，不仅仅是 ASCII 码中的大写字母）。

因此，在导入一个外部包后，能够且只能够访问该包中导出的对象。

假设在包 pack1 中我们有一个变量或函数叫做 Thing（以 T 开头，所以它能够被导出），那么在当前包中导入 pack1 包，Thing 就可以像面向对象语言那样使用点标记来调用：pack1.Thing（pack1 在这里是不可以省略的）。

因此包也可以作为命名空间使用，帮助避免命名冲突（名称冲突）：两个包中的同名变量的区别在于他们的包名，例如 pack1.Thing 和 pack2.Thing。

你可以通过使用包的别名来解决包名之间的名称冲突，或者说根据你的个人喜好对包名进行重新设置，如：import fm "fmt"。下面的代码展示了如何使用包的别名：

示例 4.2 alias.go

```go
package main

import fm "fmt" // alias3

func main() {
   fm.Println("hello, world")
}
```


注意事项

如果你导入了一个包却没有使用它，则会在构建程序时引发错误，如 imported and not used: os，这正是遵循了 Go 的格言：“没有不必要的代码！“。

包的分级声明和初始化

你可以在使用 import 导入包之后定义或声明 0 个或多个常量（const）、变量（var）和类型（type），这些对象的作用域都是全局的（在本包范围内），所以可以被本包中所有的函数调用（如 gotemplate.go 源文件中的 c 和 v），然后声明一个或多个函数（func）。






# Go变量，函数，可见性规则





