# 第二部分 各变量的类型详解
Go语言的基本类型有：

bool

string

int、int8、int16、int32、int64

uint、uint8、uint16、uint32、uint64、uintptr //u即无符号

byte // uint8 的别名

rune // int32 的别名 代表一个 Unicode 码

float32、float64

complex64、complex128//复数类型，复数类型的值一般由浮点数表示的实数部分、加号“+”、浮点数表示的虚数部分，以及小写字母“i”组成。比如：*3.7E+1 + 5.98E-2i*
## 整数型（没什么特别的）
有一种无符号的整数类型 uintptr，没有指定具体的bit大小但是足以容纳指针。uintptr 类型只有在底层编程时才需要，特别是 Go语言和 C语言函数库或操作系统接口相交互的地方。
## 浮点类型（也没什么特别的）
通常应该优先使用 float64 类型，因为 float32 类型的累计计算误差很容易扩散，并且 float32 能精确表示的正整数并不是很大。//float64也就是C的double
## 复数类型
Go语言拥有以下两种复数类型：

complex64 (32 位实数和虚数)

complex128 (64 位实数和虚数)

内建的 real 和 imag 函数分别返回复数的实部和虚部：

var x complex128 = complex(1, 2) // 1+2i

var y complex128 = complex(3, 4) // 3+4i

fmt.Println(x\*y)                 // "(-5+10i)"

fmt.Println(real(x\*y))           // "-5"

fmt.Println(imag(x\*y))           // "10"

i代表：根号-1
## bool类型
go语言对于值之间的比较有非常严格的限制，只有两个类型相同的值才可以进行比较。

布尔值不会隐式转换为数字值0或1。

如果需要经常做类似的转换, 包装成一个函数会更方便:

func btoi(b bool) int {
    if b {    
        return 1        
    }    
    return 0    
} // 如果b为真，btoi返回1；如果为假，btoi返回0

go语言不允许将整型强制转换为布尔型
## 字符串（众所周知，字符串是重点）
go的字符串是基本类型，跟C语言的字符串不同//C语言的是数组，但是也可以通过标准索引法来获取某个特定的值，比如s[0]代表第一个*字节*//注意是字节

分两种字符串

**一种是解释字符串**：常见的双引号""括起来的，里面的转移字符会被替换，如"\n"

**一种是非解释字符串**：使用反引号\`\`括起来的，里面的转移字符不会被替换。

Go 中的字符串是根据长度限定，而非特殊字符 \0，也就是字符串最后不会跟一个\0

string的默认值为空字符""

一般的比较运算符（==、!=、<、<=、>=、>）通过在内存中按字节比较来实现字符串的对比，因此比较的结果是字符串自然编码的顺序。可以通过函数 len() 来获取字符串所占的字节长度，例如：len(str)。

获取某个字节的地址是非法的，如&s[0]，只能用&s，也不能通过&s+1定位到第二个字节

两个字符串 s1 和 s2 可以通过 s := s1 + s2 拼接在一起。s2 追加在 s1 尾部并生成一个新的字符串 s。

s := "hel" + "lo,"

s += "world!"

fmt.Println(s) //输出 “hello, world!”
### 字符串实现基于 UTF-8 编码
还有一些扩展文章

[Go语言计算字符串长度——len()和RuneCountInString()](http://c.biancheng.net/view/36.html)

[Go语言遍历字符串——获取每一个字符串元素](http://c.biancheng.net/view/37.html)

[Go语言字符串截取（获取字符串的某一段字符）](http://c.biancheng.net/view/38.html)

[Go语言修改字符串](http://c.biancheng.net/view/39.html)

[Go语言字符串拼接（连接）](http://c.biancheng.net/view/40.html)

[Go语言fmt.Sprintf（格式化输出）](http://c.biancheng.net/view/41.html)

[Go语言Base64编码——电子邮件的基础编码格式](http://c.biancheng.net/view/42.html)

**等读完本章再对以上文章内容一起做汇总，详细了解字符串**
## byte和rune
Go语言的字符有以下两种：

**一种是 uint8 类型**，或者叫 byte 型，代表了 ASCII 码的一个字符。

**另一种是 rune 类型**，代表一个 UTF-8 字符。当需要处理中文、日文或者其他复合字符时，则需要用到 rune 类型。rune 类型实际是一个 int32。

var ch byte = 'A'；**字符使用单引号括起来。**

### UTF-8 和 Unicode 有何区别？
**Unicode 是字符集**某个字在Unicode 中的编码为 20320，这是不会变化的，底层的编码就是20320，但是使用不同的规则会输出不同的结果

在**UTF\-8**的**编码规则**中，会输出“你”

中文每个字符占用 3 个字节。
## 类型转换
类型转换的格式与C语言不同，有点像使用了函数（或许就是）

例：

a := 5.1
b := int(a)

当从一个取值范围较大的转换到取值范围较小的类型时（例如将 int32 转换为 int16 或将 float32 转换为 int），会发生精度丢失（截断）的情况。//比如上面的例子，b丢失了小数点后面的数，变成5
