关键字、标识符、注释，基础结构

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



## import用法

- import语句可以导入源代码文件所依赖的package包

- 不得导入源代码文件中没有用到的package，否则GO语言编译器会爆编译错误

- import语法格式主要有两种，import "package1" 和 import ("package")

- 如果一个main导入其他包，包将被顺序导入

- 如果导入的包中依赖其他包（包B），会首先导入包B，然后初始化包B中的常量和变量，最后如果包B中有init()，会自动执行init();

- 所有包导入完成之后才会对main中的常量和变量进行初始化，然后执行main中的init函数（如果存在），最后执行main函数

- 如果一个包被导入多次，则该包只会被导入一次

  ![](.\img\QQ截图20220524142619.png)

  

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



## import别名，"."，"_"

- 别名操作的含义是：将导入的包命名为另一个容易记忆的别名
- 点（.）操作的含义是：点（.）标识符的包被导入后，调用该包中函数时可以省略前缀包名
- 下划线`（_）`操作的含义是：导入该包，但是不导入整个包，而是执行该包的init()函数，一次无法通过包名来调用包中的其他函数。使用下划线 `（_）`操作往往是为了注册包里的引擎，让外部可以方便的使用



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

## GO语言数值类型，字符串类型和布尔型

- 数据类型的出现时为了把数据分成所需内存大小不同的数据，编程的时候需要用到大数据的时候才需要申请大内存，就可以充分利用内存
- 布尔型的值只可以是常量true或者false，一个简单的例子 var a bool = true
- 字符串类型，string，编码统一为 utf-8



![img](https://img2018.cnblogs.com/blog/720430/201810/720430-20181026095557581-1443352750.png)

| 类型(整形)           | 描述                                                         |
| :------------------- | :----------------------------------------------------------- |
| uint                 | 32位或64位                                                   |
| uint8                | 无符号 8 位整型 (0 到 255)                                   |
| uint16               | 无符号 16 位整型 (0 到 65535)                                |
| uint32               | 无符号 32 位整型 (0 到 4294967295)                           |
| uint64               | 无符号 64 位整型 (0 到 18446744073709551615)                 |
| int                  | 32位或64位                                                   |
| int8                 | 有符号 8 位整型 (-128 到 127)                                |
| int16                | 有符号 16 位整型 (-32768 到 32767)                           |
| int32                | 有符号 32 位整型 (-2147483648 到 2147483647)                 |
| int64                | 有符号 64 位整型 (-9223372036854775808 到 9223372036854775807) |
| **其他数值类型**     | **描述**                                                     |
| byte                 | uint8的别名(type byte = uint8)                               |
| rune                 | int32的别名(type rune = int32)，表示一个unicode码            |
| uintptr              | 无符号整型，用于存放一个指针是一种无符号的整数类型，没有指定具体的bit大小但是足以容纳指针。 uintptr类型只有在底层编程是才需要，特别是Go语言和C语言函数库或操作系统接口相交互的地方。 |
| **浮点型和复数类型** | **描述**                                                     |
| float32              | IEEE-754 32位浮点型数                                        |
| float64              | IEEE-754 64位浮点型数                                        |
| complex64            | 32 位实数和虚数                                              |
| complex128           | 64 位实数和虚数                                              |

int和uint这种后面不带位数标识的数据类型，会根据你的操作系统的实际情况来决定是int32（4字节）或者int64（8字节）。它是能够动态改变的

```go
package main

import (
	"fmt"
	"unsafe"
)

func main() {
	var a uint = 1
	var b uint8 = 1
	var c uint16 = 1
	var d uint32 = 1
	var e uint64 = 1
	var f int = 1
	var g int8 = 1
	var h int16 = 1
	var i int32 = 1
	var j int64 = 1
	var k float32 = 1.1
	var m float64 = 1.1
	var n bool = true
	var p byte = 1
	var u uintptr = 0

	fmt.Printf("uint--->%d\n", unsafe.Sizeof(a))
	fmt.Printf("uint8--->%d\n", unsafe.Sizeof(b))
	fmt.Printf("uint16--->%d\n", unsafe.Sizeof(c))
	fmt.Printf("uint32--->%d\n", unsafe.Sizeof(d))
	fmt.Printf("uint64--->%d\n", unsafe.Sizeof(e))
	fmt.Printf("int--->%d\n", unsafe.Sizeof(f))
	fmt.Printf("int8--->%d\n", unsafe.Sizeof(g))
	fmt.Printf("int16--->%d\n", unsafe.Sizeof(h))
	fmt.Printf("int32--->%d\n", unsafe.Sizeof(i))
	fmt.Printf("int64--->%d\n", unsafe.Sizeof(j))
	fmt.Printf("float32--->%d\n", unsafe.Sizeof(k))
	fmt.Printf("float64--->%d\n", unsafe.Sizeof(m))
	fmt.Printf("bool--->%d\n", unsafe.Sizeof(n))
	fmt.Printf("byte--->%d\n", unsafe.Sizeof(p))
	fmt.Printf("uintptr--->%d\n", unsafe.Sizeof(u))
}

```

在64位机器下，整形数据的内存占用情况如下(指针在32位机器下是4字节，64位机器下是8字节) 1字节等于8位

> - uint--->8
> - uint8--->1  
> - uint16--->2 
> - uint32--->4 
> - uint64--->8 
> - int--->8    
> - int8--->1   
> - int16--->2  
> - int32--->4  
> - int64--->8  
> - float32--->4
> - float64--->8
> - bool--->1   
> - byte--->1   
> - uintptr--->8

## 派生类型

- 指针类型（Pointer）
- 数组类型
- 结构化类型（struct）
- Channel类型，管道类型（chan)
- 函数类型（func）
- 切片类型（slice）
- 接口类型（interface）
- Map类型（map）

## 类型零值和类型别名

- 类型零值不是空值，而是某个变量被声明后的默认值，一般情况下，值类型的默认值是0，布尔类型默认值为false，string默认值为空字符串
- GO可以对类型设置别名

