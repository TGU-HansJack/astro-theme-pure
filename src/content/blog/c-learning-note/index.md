---
draft:
title: '现代化C语言教程笔记 Frank讲师'
publishDate: 2025-04-03
description: '学习Frank老师C语言课程记录笔记'
language: '简体中文'
heroImage: { src: './C.png', color: '#659ad3' }
tags:
  - C
---

> **感谢Frank 为广大学生打开了进入现代化C语言的大门**


内容来自讲师Micro_Frank的免费课程，笔记具体来自所讲内容，如有侵权，联系博主

其他博主笔记：[C语言--Micro_Frank - lijalen - 博客园](https://www.cnblogs.com/lijalen/articles/18236100)
22年前经典视频：[计算机应试死板理论颠覆者，拒绝PPT教学——Frank_FuckPPT - 知乎](https://zhuanlan.zhihu.com/p/148190365) 

## 第一章

### 第一个程序：C语言执行过程

```c
#include <stdio.h>
int main()
{
    printf("Hello World!\n");

    return 0;
}
```

本程序是 C 语言的基础示例，演示了如何输出一行文本。`#include <stdio.h>` 语句包含了标准输入输出库，`int main()` 是程序的入口点，`printf("Hello World!\n");` 用于打印字符串，`return 0;` 表示程序成功结束。

### C语言故事·1

### 选用教材：C Primer Plus

**建议英文中文对照阅读**


### C语言故事·2

### 声明 定义 赋值



```c
#include <stdio.h>
int main()
{    //声明 declaration
    int number = 123;
    //定义 definition
    printf("%d\n, number");
    //赋值 assignment
    return 0;
}    //Frank_PPT 讲师在这一节还讲到了TAB键与制表符的使用
```


### 标识符

```c
identifier:
    nondigit
     identifier
    digit

nondigit:
     _ a b c d e f g h i j k l m n o p q r s t u v w x y z
     A B C D E F G H I J K L M N O P Q R S T U V W X Y Z

digit:
     0 1 2 3 4 5 6 7 8 9
```

**编译器会将大写和小写字母视为不同的字符**


```c
add
ADD
Add
aDD
```

**Microsoft 专用**

以两条下划线开头或者以一条下划线后跟一个大写字母开头的名称。

Microsoft 使用下划线和大写字母作为宏名称的开头，并使用双下划线作为 Microsoft 特定关键字名称的开头。

**有效标识符**

```c
j
count
temp1
top_of_page
skip12
LastNum
```

### 关键字


## 第二章 数据类型

### 注释



```c
//行
    注释

/*
        块
    注
    释
*/
```

### 进制

### 变量

```c
//int 变量
#include <stdio.h>

int main(){
    //integer 赋值
    int sun_flower = 9673;

    printf("阳光目前的数值：%d\n", sun_flower);

    return 0;
}
```

### 整型

**语法：定义的类型 标识符 = 值**

#### int类型在内存中存储

计算机的存储单位：

**1G = 1024MB**

**1MB = 1024KiB**

**1KiB = 1024Btyes**

**1024Btye = 8Bit**

|00000000|00000000|00000000|00000000|int|
|:-:|---|---|---|---|

**0**用于显示正负，一共可以表示 $ 2^{32} - 1 $个值



```c
#include <stdio.h>
int main()
{
    int number = 100;
    //以十进制表示
    printf("Decimal: %d\n", number);

    printf("Octal: %o\n", number);

    printf("Hexadeciaml (lowercase) : %x\n", number);

    printf("Hexadeciaml (uppercase) : %X\n", number);

    return 0;
}
```

### 浮点数

**浮点数有：2.75、3.16E7、2e-8等**

#### 计算机怎么存储浮点数的？

浮点数：符号（1）+ 小数（23）+ 指数（8） int类型32位

**One Step** : **3.14159**=> $ 314159×10^{-5} $ 先换成int类型

**Two Step** : **如何存储指数？**$ 2^{8} $ = 256 **从0到255， 偏127左边的数为负指数，偏值即为指数值。**


#### float 和 double 类型

**float 类型的范围 大约在 3.4E-38 和 3.4+38之间。**

小数一般>1 ， 所以规范化的浮点数往往大于1， **尾数**（占据剩余的位数。）对于标准的浮点表示，尾数通常以1.XXXX的形式存储，隐式地前面有一个1（称为隐式前导位）。

**所以** float尾数隐式长度为24位，double尾数隐式长度为53位。

**单精度（32位）**- **双精度（64位）**

**存储>精度 选择double ， 存储<精度 选择float**

#### 浮点数的打印输出



```c
#include <stdio.h>

int main() {

    float temperture = 36.5f;

    float humidity = 48.3f;

    float speed_of_sound = 343.5e2f;

    float lenght = 12.3f, width = 23.45f, height = 34.56f;

    printf("Temperture: %f\n", temperture);
    printf("Humidity: %f\n", humidity);
    printf("Speed_of_Sound: %f\n", speed_of_sound);
    printf("Lenght: %f x %f x %f\n", lenght width height);
//丢失精度
//double %lf , float %f
    return 0;
}
```

#### C99中对浮点数的规定



```c
#include <stdio.h>

int main () {

float num = 123.456;

printf("Using %%f: %f\n", num);

//%e %E 科学计数法格式化输出
ptintf("Using %%e: %e\n", num);
printf("Using %%E: %E\n", num);

//%a %A 十六进制浮点数 p计数法
printf("Using %%a: %a\n", num);
printf("Using %%A: %A\n", num);

return 0;
}
```

#### 浮点数的溢



```c
#include <stdio.h>
#include <float.h>//点开看看

int main() {

    float max_float = FLT_MAX;

    float overflow = max_float * 1000.0f;
    //OverFlow 上溢

    float min_float = FLT_MIN;

    float underfloat = min_float / 1000.0f;
    //UnderFlow 下溢

    printf("Maximum Float: %e\n", max_float);
    printf("OverFloat: %e\n", overflow);
    printf("Minimum Float: %e\n", min_float);
    printf("UnderFloat: %e\n", underfloat);

    return 0;
}
```

#### Nan & Infinity



```c
#include <stdio.h>
#include <float.h>
#include <math.h>

int main() {

    //正无穷大
    float positive_infinty = INIFITY;
    printf("Positive Infinity: %f\n", positive_infinty);

    //负无穷大
    float negative_infinty = -INIFITY;
    printf("Negative Infinity: %f\n", negative_infinty);

    //除以0产生的无穷大
    float num = 1.0f;
    float inifity = num / 0.0f;
    printf("0.0 / 0.0 = %f\n", nan);

    //Nano 0/0
    //float nan = 0.0f /0.0f;
    //printf("0.0f / 0.0f =%f\n", nan)

    //对负数开平方跟
    float negative_sqrt = sqrt(-1.0f);
    printf("sqrt(1.0f) = %f\n", nefative_sqrt);

    return 0;
}
```

#### 最近偶数舍入标准（银行家舍入）



```c
#include <stdio.h>

int main() {

    //四舍五入
    //IEEE 754
    //最近偶数舍入  round to nearest, ties to even
    //银行家舍入

    //3.14159
    float number = 3.14159f;
    printf("%.4f\n", number);

    //3.15
    //3.25
    return 0;
}
```

#### double,long double 科研与企业的区别



```c
#include <stdio.h>

int main() {

    //double

    //float 丢失精度

    //3D渲染

    //利息 NASA

    //浮点常量 3.14
    //3.14 默认double类型 特别注明3.14f

    return 0；

}
```


#### float和double 有效精度对比 原理与计算



```c
#include <stdio.h>

int main() {
 
    float float_num 1.0 / 3.0;
    double double_num = 1.0 / 3.0;

    printf("Float precision: %20f\n", float_num);
    printf("Double precision: %.20lf\n", double_num);
    //float 和 double 有效精度的对比
    printf("Defined max precision for double: %d\n", FLT_DIG)
    printf("Defined max precision for float: %d\n", DBL_DIG)

    //53除以log₂10  24除以log₂10  即为精确度

}
```

**float**（单精度浮点数）：通常占用 4 字节（32 位）其中1位为符号位，8位为指数位，23位为尾数（有效数字）。通常有效精度约为 7 位十进制数字。

**double**（双精度浮点数）： 通常占用 8 字节（64 位）。 1位为符号位，11位为指数位，52位为尾数。通常有效精度约为 15 到 16 位十进制数字。

浮点数的表示遵循 IEEE 754 标准。它的值可以通过以下公式表示：

**sign**: 符号位，决定正负。 **fraction**: 尾数，决定数值的精度。 **exponent**: 指数，决定数值的大小。 **bias**: 偏移量，对于 `float`为 127，对于 `double`为 1023。

银行中用**定点数 （数据库MySQL）**

### char & ASCII


```c
#include <stdio.h>

int main() {

    char mych = 'a'; //实际用int类型 存储， 将A转化成int类型 int(97)

    printf("mych: %c\n")；

    //ASCII美国信息标准代码 一般使用7bit 包括128个字符


}
```

### 转义序列 反斜杠 \

| 转义序列 | 表示                                                         |
| :------- | ------------------------------------------------------------ |
| \a       | 响铃（警报）                                                 |
| \b       | Backspace                                                    |
| \f       | 换页                                                         |
| \n       | 换行                                                         |
| \r       | 回车                                                         |
| \t       | 水平制表符                                                   |
| \v       | 垂直制表符                                                   |
| %>       | 单引号                                                       |
| :"       | 双引号                                                       |
| \\\      | 反斜杠                                                       |
| %>       | 文本问号                                                     |
| \ooo     | 八进制表示法的 ASCII 字符                                    |
| \x hh  | 十六进制表示法的 ASCII 字符                                  |
| \x hhhh  | 十六进制表示法的 Unicode 字符（如果此转义序列用于宽字符常量或 Unicode 字符串文本）。   例如，`WCHAR f = L'\x4e00'` 或 `WCHAR b[] = L"The Chinese character for one is \x4e00"`。 |

```c
清除屏幕 print("\033[2J");
移动光标 print("\033[%d;%dH", 3, 3);
```

### 布尔类型 bool类型


```c
#include <stdio.h>
#include <stdbool.h>

int main() {
    //true or false
    //转换成 1 or 0

    bool is_game_win = true;
    bool is_game_over = false;

    return 0;
}
```

### 常量const与#define宏

```c
#include <stdio.h>
#define PI 3.14

int main() {
    //常量

    const double MAX_USER = 100;

    printf("PI: %lf\n", PI);

    return 0;

}
```

### 第二章结束语

## 第三章 运算符

### 运算符的介绍

**1、算术运算符**

- 一元 [`++`（增量）](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/arithmetic-operators#increment-operator-)、[`--`（减量）](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/arithmetic-operators#decrement-operator---)、[`+`（加）](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/arithmetic-operators#unary-plus-and-minus-operators)和 [`-`（减）](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/arithmetic-operators#unary-plus-and-minus-operators)运算符
- 二元 [`*`（乘法）](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/arithmetic-operators#multiplication-operator-)、[`/`（除法）](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/arithmetic-operators#division-operator-)、[`%`（余数）](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/arithmetic-operators#remainder-operator-)、[`+`（加法）](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/arithmetic-operators#addition-operator-)和 [`-`（减法）](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/operators/arithmetic-operators#subtraction-operator--)

**2、关系运算符**

|运算符|测试的关系|
|---|---|
|**`<`**|第一个操作数小于第二个操作数|
|**`>`**|第一个操作数大于第二个操作数|
|**`<=`**|第一个操作数小于或等于第二个操作数|
|**`>=`**|第一个操作数大于或等于第二个操作数|
|**`==`**|第一个操作数等于第二个操作数|
|**`!=`**|第一个操作数不等于第二个操作数|

### 数据对象 左值和右值



```c
#include <stdio.h>
#include <stdint.h>
#include <inttype.h>
int main() {

    //数据对象
    //左值 Lvalue
    //右值 Rvalue
    //运算符
    uint32_t apple_box = 5;

    uint32_t orange_box = 5;

    printf("苹果盒子里面有 %" PRIu32 "个苹果\n", apple_box);
    printf("橘子盒子里面有 %" PRIu32 "个橘子\n", orange_box);

    uint32_t total_fruit = apple_box + orange_box;

    printf("盒子里面有 %" PRIu32 "个水果\n", total_box);
 
    return 0;

}
```

### 多重赋值

### 前缀后缀递增与递减



```c
#include <stdio.h>
#include <stdint.h>
#include <inttype.h>
int main() {

    int32_t value = 5;
    int32_t result_1;
    int32_t result_2;

    //后缀递增，先赋值，再++
    result_1 = value++;
    //前缀递减，先--，后赋值
    result_2 = --value;
    printf("After postfix increment, result_1 = %" PRIu32 ",result_2 = %" PRIu32 ", value = %" PRIu32 "\n", result_1, result_2, value);

    return 0;

}
```

### 按位移位运算符


```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

int main() {

    uint8_t num = 22;
    num >> 2;//高位补零，低位排挤

    return 0;
}
```



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

int main() {

    uint8_t num = 22;
    //num >> 2; 高位补零，低位排挤

    uint8_t num = 22;
    printf("Original number: %" PRIu8 " (binary: 00010110)\n", num);

    uint8_t left_shifted = num << 2;
    printf("Left shifted by 2: %" PRIu8 " (binary: 01011000)\n", num);

    uint8_t right_shifted = num >> 2;
    printf("Right shifted by 2: %" PRIu8 " (binary: 00000101)\n", num);

    return 0;
}
```

**乘法运算：移位运算符为什么比 直接  处理快？**


```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

int main() {

    uint8_t num = 25;

    //uint32_t result = num * 1024;
    printf("Result: %" PRIu32 "\n", result);
    ALU 负责 基本的运算

    return 0;
}
```

移位运算符比直接使用乘法运算符（*）处理快的原因主要是因为底层实现的不同。


**int类型需要注意左移可能导致溢出**

### 逻辑的真与假、C关系运算符



```c
include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

int main() {

    int a = 10;
    int b = 20;
    //xxx ? xxx : xxx 判断true or false
    bool greater = a > b;
    printf("a > b: %d\n", greater);
    printf("a > b: %s\n", greater ? "true" , "false");

    bool less = a < b;
    printf("a < b: %d\n", less);
    printf("a < b: %s\n", less ? "true" , "false");

    bool not_equal = a != b;
    printf("a != b: %d\n", not_equal);
    printf("a != b: %s\n", not_equal ? "true" , "false");

    bool equal = a == b;
    printf("a == b: %d\n", equal);
    printf("a == b: %s\n", equal ? "true" , "false");

    bool greater_or_equal = a >= b;
    printf("a >= b: %d\n", greater_or_equal);
    printf("a >= b: %s\n", greater_or_equal ? "true" , "false");

    bool less_or_equal = a <= b;
    printf("a <= b: %d\n", less_or_equal );
    printf("a <= b: %s\n", less_or_equal ? "true" , "false");

    return 0;
}
```

### 条件表达式运算符



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

int main() {

    int score = 89;

    printf("你的成绩等级：%s\n", score >= 60 ? "及格" : "不及格");

    return 0;
}
```



```c
#include 
int main() { 
    int score = 85; 
    // 你可以根据需要修改这个分数 
    const char* result = (score >= 90) ? "优秀" : (score >= 85) ? "一般" : (score >= 60) ? "合格" : "不及格"; printf("%s\n", result); 

    return 0; 
}
```

### 按位运算符 & ^ |

|运算符|描述|
|---|---|
|**`&`**|按位“与”运算符将其第一操作数的每个位与其第二操作数的相应位进行比较。 如果两个位均为 1，则对应的结果位将设置为 1。 否则，将对应的结果位设置为 0。|
|**`^`**|按位“异或”运算符将其第一操作数的每个位与其第二操作数的相应位进行比较。 如果一个位是 0，另一个位是 1，则相应的结果位将设置为 1。 否则，将对应的结果位设置为 0。|
|**`|`**||

**& 按位与**


```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

int main() {

    int a = 12;
    int b = 25;

    // & 二进制 位位比较 都是1才输出1 比如
    printf("%d\n", 12 & 25);
    //将特定位清零、检查某一位是否为1

    return 0;
}
```

**| 按位与或**



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

int main() {

    int a = 2;
    int b = 5;

    // | 二进制 位位比较 有1就输出1 比如0111 
    printf("%d\n", a | b);
    //设置特定位、组合标志位

    return 0;
}
```

**^ 按位异或**



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

int main() {

    int a = 2;
    int b = 10;

    // ^ 二进制 位位比较 有1 0才输出1 比如1000 
    printf("%d\n", a ^ b);
    //逻辑异或 XOR操作
    //翻转特定位、交换两个变量值、检查不同

    return 0;
}
```

### 按位取反 ~

### 掩码与电路遥控LED灯练习



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

void print_binary(uint8_t num);

int main() {

    uint8_t status = 0b00001100;    //初始状态

    printf("Initial status: 0b");
    print_binary(status);
    printf("\t(Binary)\n");

    status = status & 0b11111011;    //mask掩码 异或 控制
    printf(("Fanal status: 0b");
    printf("\t(Binary)\n");
    return 0;
}

void print_binary(uint8_t num);
    for (int index = 7; index >= 0; index--) {
        printf("%d", (num >> index) & 1);
    }
}
```

### 逻辑运算符 && ||

|运算符|描述|
|---|---|
|**`&&`**|如果两个操作数具有非零值，则逻辑“与”运算符产生值 1。 如果其中一个操作数等于 0，则结果为 0。 如果逻辑“与”运算的第一个操作数等于 0，则不会计算第二个操作数。|
|`\|\|`|逻辑“或”运算符对其操作数执行“与或”运算。 如果两个操作数的值均为 0，则结果为 0。 如果其中一个操作数具有非零值，则结果为 1。 如果逻辑“或”运算的第一个操作数具有非零值，则不会计算第二个操作数。|

### 复合赋值运算符

**复合赋值运算符的操作数只能是整型和浮点型**



```c
#include <stdio.h>
/*
struct BigStruct {
    //..
    //..
};

void uodate(BigStruct& bs) {
    BigStruct temp = someExp();

    bs = bs + temp;

    bs += temp;

}
*/


int main() {

    int base_number = 8;
    int add_number = 2;
    int sub_number = 3;
    int mul_number = 2;
    int div_number = 5;
    int mod_number = 4;

    base_number += add_number;
    //In-place Modification 原地修改

    base_number -=
    base_number *=
    base_number /=
    base_number %=
    base_number <<=
    base_number >>=
    base_number &=
    base_number |=
    base_number ^=

    return 0;

}
```

### 逗号运算符



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

int main() {

    uint32_t a = 1, b = 2, c = 4;

    uint32_t result = (a += 1, b -= 1, c += 3);

    printf("a = %d, b = %d, c = %d, result = %d", a, b, c, result);

    return 0;

}
```

**Microsoft Learn**



```c
// cpp_comma_operator.cpp
#include <stdio.h>
int main () {
   int i = 10, b = 20, c= 30;
   i = b, c;
   printf("%i\n", i);

   i = (b, c);
   printf("%i\n", i);
}
```

**Output：**



```c
20
30
```




```c
func_one( x, y + 2, z );
func_two( (x--, y + 2), z );
```

在上面的对 `func_one` 的函数调用中，会传递以逗号分隔的三个参数：`x`、`y + 2` 和 `z`。 在对 `func_two` 的函数调用中，圆括号强制编译器将第一个逗号解释为顺序计算运算符。 此函数调用将两个参数传递给 `func_two`。 第一个参数是顺序计算运算 `(x--, y + 2)` 的结果，具有表达式 `y + 2` 的值和类型；第二个参数为 `z`。



### 计算的优先级和顺序

|符号 ^1^|操作类型|结合性|
|---|---|---|
|`[` `]` `(` `)` `.` `->`<br/>`++` `--`（后缀）|表达式|从左到右|
|**`sizeof`** `&` `*` `+` `-` `~` `!`<br/>`++` `--`（前缀）|一元|从右到左|
|_typecasts_|一元|从右到左|
|`*` `/` `%`|乘法|从左到右|
|`+` `-`|加法|从左到右|
|`<<` `>>`|按位移动|从左到右|
|`<` `>` `<=` `>=`|关系|从左到右|
|`==` `!=`|相等|从左到右|
|`&`|按位“与”|从左到右|
|`^`|按位“异或”|从左到右|
|`\|`|按位“与或”|从左到右|
|`&&`|逻辑“与”|从左到右|
|`\|\|`|逻辑“或”|从左到右|
|`? :`|条件表达式|从右到左|
|`=` `*=` `/=` `%=`<br/>`+=` `-=` `<<=` `>>=` `&=`<br/>`^=` `\|=`|简单和复合赋值 ^2^|从右到左|
|`,`|顺序计算|从左到右|

^1^ 运算符按优先级的降序顺序列出。 如果多个运算符出现在同一行或一个组中，则它们具有相同的优先级。

^2^ 所有简单的和复合的赋值运算符都有相同的优先级。

表达式可以包含优先级相同的多个运算符。 当多个具有相同级别的这类运算符出现在表达式中时，计算将根据该运算符的结合性按从右到左或从左至右的顺序来执行。 计算的方向不影响在相同级别包括多个乘法 (`*`)、加法 (`+`) 或二进制按位（`&`、`|` 或 `^`）运算符的表达式的结果。 语言未定义运算的顺序。 如果编译器可以保证一致的结果，则编译器可以按任意顺序随意计算此类表达式。

只有顺序计算 (`,`)、逻辑“与”(`&&`)、逻辑“或” (`||`)、条件表达式 (`? :`) 和函数调用运算符构成序列点，因此，确保对其操作数的计算采用特定顺序。 函数调用运算符是一组紧跟函数标识符的圆括号。 确保顺序计算运算符 (`,`) 按从左到右的顺序计算其操作数。 （函数调用中的逗号运算符与顺序计算运算符不同，不提供任何此类保证。）有关详细信息，请参阅[序列点](https://learn.microsoft.com/zh-cn/cpp/c-language/c-sequence-points?view=msvc-170)。

逻辑运算符还确保按从左至右的顺序计算操作数。 但是，它们会计算确定表达式结果所需的最小数目的操作数。 这称作“短路”计算。 因此，无法计算表达式的一些操作数。 例如，在下面的表达式中

`x && y++`

仅当 `y++` 为 true（非零）时，才计算第二操作数 (`x`)。 因此，如果 `y` 为 false (0)，则 `x` 不增加。

**示例**

以下列表显示编译器如何自动绑定多个示例表达式：

展开表

|表达式|自动绑定|
|---|---|
|`a & b \|\|c`|`(a & b) \|\|c`|
|`a = b \|\|c`|`a = (b \|\|c)`|
|`q && r \|\|s--`|`(q && r) \|\|s--`|

在第一个表达式中，按位“与”运算符 (`&`) 的优先级高于逻辑“或”运算符 (`||`) 的优先级，因此，`a & b` 构成了逻辑“或”运算的第一操作数。

在第二个表达式中，逻辑“或”运算符 (`||`) 的优先级高于简单赋值运算符 (`=`) 的优先级，因此，`b || c` 在赋值中分组为右操作数。 请注意，赋给 `a` 的值为 0 或 1。

第三个表达式显示可能会生成意外结果的格式正确的表达式。 逻辑“与”运算符 (`&&`) 的优先级高于逻辑“或”运算符 (`||`) 的优先级，因此，将 `q && r` 分组为操作数。 由于逻辑运算符确保按从左到右的顺序计算操作数，因此 `q && r` 先于 `s--` 被计算。 但是，如果 `q && r` 计算的结果为非零值，则不计算 `s--`，并且 `s` 不会减少。 如果 `s` 未减少会导致程序出现问题，则 `s--` 应显示为表达式的第一操作数，或者在单独的运算中应减少 `s`。

以下表达式是非法的并会在编译时生成诊断消息：

|非法表达式|默认分组|
|---|---|
|`p == 0 ? p += 1: p += 2`|`( p == 0 ? p += 1 : p ) += 2`|

在此表达式中，相等运算符 (`==`) 的优先级最高，因此，将 `p == 0` 分组为操作数。 条件表达式运算符 (`? :`) 具有下一个最高级别的优先级。 其第一操作数是 `p == 0`，第二操作数是 `p += 1`。 但是，条件表达式运算符的最后一个操作数被视为 `p` 而不是 `p += 2`，因为与复合赋值运算符相比，`p` 的匹配项将更紧密地绑定到条件表达式运算符。 由于 `+= 2` 没有左操作数，因此发生语法错误。 您应使用括号以防止此类错误发生并生成可读性更高的代码。 例如，可以按如下所示使用括号来更正和阐明前面的示例：

`( p == 0 ) ? ( p += 1 ) : ( p += 2 )`

**请参阅**

[C 运算符](https://learn.microsoft.com/zh-cn/cpp/c-language/c-operators?view=msvc-170)


```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

int main() {

    int32_t result;

    result = a * b + c << d >e ? b : c * sizeof(++e) / sizeof(int32_t);

    printf(Result: %" PRId32 ", result);

    return 0;
}
```

> 一元操作符号中，& - + 等前缀 *为解引用运算符

`&按位与`与 `前缀&`不一样，**后者优先级高！**

## 第三章 分支与控制

### 决策控制

如果天气晴朗 ——选出去玩；

逻辑 && || ；

人是有决策能力的，逻辑的判断让程序去·选择！

前提是：让程序去判断 ”天气状态“ 是否为true ， false

---

**比如 一个天气应用程序：**

温度< 30° 温度> 30°

---

编程语言：

- 要应用到实际环境
- 灵活性，随机
- 不可能全面，若有完美 定是谎言

### if语句和if else



```c
#include <stdio.h>
#include <stdint.h>
#include <inttype.h>
#include <stdbool.h>

int main() {

    uint32_t number = 100;

    //if 语句
    if (number > 10) {
        printf("这个数大于10！\n");
        printf("这个数大于10！\n");

    }

    if (number >=100) {
        printf("这个数大于等于100！\n");


    } 
    else {
        printf("这个数小于100！\n");

    }


    return 0;
}
```

### 逻辑与或的短路行为



```c
#include <stdio.h>
#include <stdint.h>
#include <inttype.h>
#include <stdbool.h>

int main() {

    bool is_weather_sunny = false;
    bool is_venue_available = true;

    if (is_weather_sunny && is_venue_available) {
        printf("活动如期举行!\n");
        }
    else {
        printf("活动不能如期举行!\n")；
    if (!is_weather_sunny) {
        printf("原因：天气不晴朗！\n")
        }
    if (!is_vueue_available) {
        printf("原因：没有场地!\n")
        }

    return 0;
}
```



```c
//自动贩卖机
//只支持硬币
#include <stdio.h>
#include <stdbool.h>
#include <inttypes.h>

int main(void)

{

    const uint32_t PRICE = 3;    //drink's price

    uint32_t balance = 0;        //now total_coin

    uint32_t coin;                //every coin in it

    puts("drink's price is $5 please!");

    while (balance < PRICE)    //在投够数额前
    {
        puts("don't!");    //拒绝交易

        scanf_s("%" PRIu32, &coin);


        if (coin == 1 || coin == 2 || coin == 5) //确定金额类型
        {
        balance += coin;    //赋值 加等

        printf("你已经投入 $%" PRIu32 "\n", balance);
        }
        else {

        printf("sorry!我们不接受 $%" PRIu32 "的硬币\n", coin);
        }
    }

    if (balance > PRICE) {
        printf("找零：%" PRIu32 "\n", balance - PRICE);
    }
    return 0;
}
```



```c
#include <stdio.h>
#include <stdint.h>
#include <stdbool.h>
#include <inttypes.h>

int main(void)
{

        //玩家进入不同条件的房间

    uint32_t coins= 15;
    bool is_vip = true;
    bool have_special_tool = false;

    if (is_vip) {
        puts("vip Enter!\n");
    }
    else {
        puts("not_vip Exit!\n");
    }

    if (coins >= 10 || have_special_tool) {
        puts("Enter!\n")
    }
    else {
        puts("Exit!\n");
    }
    return 0;
}
```



```c
#include <stdio.h>
#include <stdbool.h>
#include <inttypes.h>

int main(void)
{
    const uint32_t all_laps = 100;

    uint32_t current_laps = 0;

    puts("开始!");

    while (current_laps < all_laps)
    {
        current_laps++;

        printf("完成第 %"PRIu32"圈。\n", current_laps);
    }
    return 0;
}
```


```c
//自动贩卖机
//只支持硬币
#include <stdio.h>
#include <stdbool.h>
#include <inttypes.h>

int main(void)

{

    const uint32_t price = 10;    //drink's price

    uint32_t balance = 0;        //now total_coin

    uint32_t coin;                //every coin in it

    puts("drink's price is $5 please!");

    while (balance < price)    //在投够数额前 只有在够的时候才开始循环
    {
        puts("don't!");    //拒绝交易

        //模拟投币
        scanf_s("%" priu32, &coin);


        if (coin == 1 || coin == 2 || coin == 5) //确定金额类型
        { 
        balance += coin;    //赋值 加等 计数

        printf("你已经投入 $%" priu32 "\n", balance);
        }
        else {

        printf("sorry!我们不接受 $%" priu32 "的硬币\n", coin);
        }
    }

    if (balance > price) {
        printf("找零：%" priu32 "\n", balance - price);
    }
    return 0;
}

//一定要写printf 可以看到效果。

//遇到循环问题
//不要在循环中多次运用，来避免不必要的错误


```



```c
//写一个计算机求和


#include <stdio.h>
#include <inttypes.h>

int main(void)
{
    uint32_t sum = 0;    //设置初始数字和为0

    uint32_t number = 1;    //第一次number的值

    while (number != 0)    //设置0为退出循环的方法
    {
        scanf_s("%" PRIu32, &number);    //scanf重新赋值number，第一次number值实际没有用到。
        sum += number;
    }

    printf("所求的和sum是 %" PRIu32 "\n", sum);

    return 0;
}






```



```c
#include <stdio.h>
#include <inttypes.h>
#include <stdbool.h>
#include <stdlib.h>
#include <limits.h>
#include <errno.h>

int main()
{
    uint32_t sum = 0;    //设置初始数字和为0

    char input[50];    //我们将用户输入的字符转换成数字

    char* end;

    puts("请输入一系列数字，用回车隔开，我们计算他们的和,输出q结束");

    while (true)    //设置0为退出循环的方法
    {
        errno = 0;

        puts("输入一个数字：");

        scanf_s(" %49s", &input, 50);

        if (input[0] == 'q' && input[1] == '\0') {
            break;
        }

        long number = strtol(input, &end, 10);
        //将输入的字符转换成数字，并且加到总和sum中
        //0的ASCII为48
        if (end == input || *end !='\0') {
            printf("无效输入，请输入一个数字或是字符q\n");

        }
        else if (errno == ERANGE || number < 0 || number > UINT32_MAX){
            printf("数字超出范围！请输入小于或等于 %u 的正整数\n", UINT32_MAX);
        }
        else {
            sum += (uint32_t)number;
        }

    }

    printf("所求的和sum是 %" PRIu32 "\n", sum);

    return 0;
}
```


```c
#include <stdio.h>
#include <inttypes.h>
#include <stdbool.h>

int main()
{
    uint32_t sum = 0;    //设置初始数字和为0

    uint32_t number;    //第一次number的值

    puts("请输入一系列数字，用回车隔开，我们计算他们的和,输出0结束");

    while (true)    //设置0为退出循环的方法
    {

        scanf_s(" % " PRIu32, &number);

        if (number == 0) {
            break;
        }

        sum += number;
    }

    printf("所求的和sum是 %" PRIu32 "\n", sum);

    return 0;
}
```



```c
//卫语句的使用：租车案例
#include <stdio.h>
#include <stdint.h>
#include <stdbool.h>
#include <inttypes.h>

void check_car_rent(uint8_t age, uint8_t driving_exp_years);

int main(void) 
{
    check_car_rent(22, 3);

    return 0;
}

void check_car_rent(uint8_t age, uint8_t driving_exp_years) {
//卫语句
    if (age < 21) {
        puts("不符合条件，年龄不足！");
        return 0;
    }

    if (driving_exp_years < 1) {
        puts("不符合条件，驾驶年龄不足!");
        return 0;
    }
}
```



```c
//简化逻辑表达式
#include <stdbool.h>
#include <inttypes.h>

int main(void) 
{
    //逻辑表达式应该简化！
    bool is_weekend = true;
    bool has_enter = true;

    if (!has_enter) {
        return 0;
    }

    return 0;
}
```

### 状态机：管理复杂状态的转换



```c
#include <stdbool.h>
#include <inttypes.h>

int main(void)
{
    //状态机：管理复杂状态的转换，使用switch case

    uint8_t traffic_state = 0;

    switch (traffic_state)
    {
    case 0:
        puts("red");
        traffic_state = 1;
        break;
    case 1:
        puts("yellow");
        traffic_state = 2;
        break;
    case 2:
        puts("green");
        traffic_state = 0;
        break;
    default:
        puts("???");
        break;
    }

    return 0;
}
```

### switch-case 与 if-else 的区别



```c
//switch-case 与 if-else 的区别
#include <stdio.h>
#include <stdbool.h>
#include <inttypes.h>

int mian(void)
{


    return 0;
}
```

### 循环在生活中的作用



```c
//循环在生活中的作用


#include <stdio.h>
#include <stdbool.h>
#include <inttypes.h>

int main(void)
{
    int32_t number;

    scanf_s("请输入: %d %d %d", &number, &number, &number);//在实际开发中不用scanf
    //not write \n in it
    printf("已经扫描到你输入的数字为: %d %d %d\n", number, number, number);

    return 0;
}

```

### do-while 和 while 的区别



```c
//do-while 和 while 的区别

#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

int main(void)
{
    uint32_t total_laps = 10;

    uint32_t current_laps = 0;

    puts("罚跑开始！");

    do {

        current_laps++;
        printf("跑步者完成第%" PRIu32 "圈\n", current_laps);
    } while (current_laps < total_laps);

    return 0;
}



```

### do-while 的实际作用



```c
//do-while 的实际作用

#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdbool.h>

int main(void)
{

    uint32_t choice;

    do 
    {

        puts("****主菜单****");
        puts("1. 新游戏");
        puts("2. 载入游戏");
        puts("3. 退出");
        scanf_s("%" PRIu32, &choice);

        switch (choice) 
        {
        case 1:
            puts("****创建新游戏！");
            break;
        case 2:
            puts("****载入存档！");
            break;
        case 3:
            puts("****退出游戏！");
            break;
        }

    } while (choice != 3);

    return 0;

}



```

### 随机数猜数游戏案例



```c
//随机数猜数游戏案例
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <time.h>
#include <stdlib.h>

int main(void)
{
    uint32_t secret_num, guess, status;
    char buffer[50];

    srand(time(NULL));
    //生成1-100的随机数
    secret_num = rand() % 100 + 1;

    puts("猜猜嘛！");

    do {
        puts("请输入你猜的数：");

        fgets(buffer, sizeof(buffer), stdin);
        status = sscanf_s(buffer, "%d", &guess);
        //卫语句
        if (status != 1) {
                puts("输入无效！");
                continue;
        }

        if (guess < secret_num) {
            puts("太小了！");
        }
        else if (guess > secret_num) {
            puts("太大了！");
        }

    } while (guess != secret_num);

    printf("恭喜你，猜对了！");

    return 0;
}

```

### continue语句



```c
//continue语句
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>

int main(void) {

    uint32_t sum = 0;
    int32_t number;

    puts("请输入一系列的正整数,可以输入负数,输入0结束");

    while (1) {
        puts("请输入一个数字");

        scanf_s("%d", &number);

        if (number == 0) {
            break;
        }
        if (number < 0) {
            continue;    //continue 直接跳过这次循环
        }
        sum += number;//计数
    }
    printf("计算结果为：%" PRIu32 "\n", sum);

    return 0;

}
```

### continue与break的联用条件判断



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
int main() {
    uint32_t number;
    
    puts("请输入数字（0-100），输入-1后结束程序");
    
    while (1) {
        puts("请输入一个数字：");
        scanf_s("%d", &number);
        //卫语句！检查条件结束
        if (number == -1) {
            break;
        }
        if (number < 0 || number > 100) {
            continue;//跳过本次循环的剩余部分。
        }
        sum += number;
    }
    
    return 0;
}
```

### 初探for循环



```c
//初探for循环
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>

int main(void)
{
    //for循环
    const uint32_t total_laps = 10;    //目标圈数

    //uint32_t current_lap = 0;    初始化当前圈数

    puts("跑步者开始跑步");

    for (uint32_t current_lap = 0;current_lap <= total_laps;current_lap++) {
        printf("跑步者完成第 %" PRIu32 " 圈\n", current_lap);
    }

    return 0;

}
```

### 整数平方求和



```c
//整数平方求和
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>

int main(void)
{
     /*
        目的为了计算从1到n（n由玩家输入）的所有整数的平方和

     */
    uint32_t number;

    uint32_t sum_of_squares = 0;

    puts("输入整数n");

    scanf_s("%u", &number);

    for (uint32_t index = 1; index <= number; index++) {
        sum_of_squares += index * index;
    }

    printf("%" PRIu32 "\n", sum_of_squares);
    return 0;
}
```

### 倒数五个数



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>

int main(void) {

    /*
        星光大道
        倒数五个数
    */

    uint32_t start_number;
    
    puts("Please enter a positive integer");
    scanf_s("%" SCNu32, &start_number);

    puts("The countdown begins");
    for (uint32_t index_number = start_number; index_number > 0; index_number--)
    {
        printf("%" SCNu32 "\n", index_number);
    }
    puts("The countdown stop");
    return 0;
}
```



```c
//扩展sleep
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <windows.h>

int main(void) 
{

    /*
        星光大道
        倒数五个数
    */

    uint32_t start_number;
    
    puts("Please enter a positive integer");
    scanf_s("%" SCNu32, &start_number);

    puts("The countdown begins");
    for (uint32_t index_number = start_number; index_number > 0; index_number--)
    {
        printf("%" SCNu32 "\n", index_number);
      Sleep(1000);
    }
    puts("The countdown stop");
    return 0;
}
```

### 阶乘



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <windows.h>
//阶乘

int main(void) {

    uint32_t number;
    uint32_t factorial = 1;

    puts("Please enter a positive integer :");
    scanf_s("%" SCNu32, &number);

    for (uint32_t index = 1; index <= number; index++) {
        factorial *= index;
    }

    printf("%" PRIu32 "! = %" PRIu32 "\n", number, factorial);

    return 0;
}
```

### 开方



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <math.h>
#include <stdbool.h>

int main(void)
{
    double number = 4.00;

    printf("%lf\n", sqrt(121));

    return 0;
}
```

### 素数



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <math.h>
#include <stdbool.h>

int main(void)
{
    uint32_t num;

    bool is_prime = true;

    puts("Please enter a positive integer,except for 1,we will check if it is prime:");

    scanf_s("%" SCNu32, &num);

    if (num <= 1) {
        is_prime = false;
    }
    else {
        //for , 检查除了1和它本身之外的因数
        for (uint32_t i = 2; i * i <= num; i++) {
            if (num % i == 0) {
                is_prime = 0;
                break;
            }
        }
    }
    return 0;
}
```



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <math.h>
#include <stdbool.h>

int main(void)
{
    uint32_t size;
    puts("请输入图案的大小: ");
    scanf_s("%" SCNu32, &size);

    //特点：长和宽一样
    puts("打印正方形图案");

    for (uint32_t i = 0; i < size; i++) {
        for (uint32_t j = 0; j < size; j++)
        {
            printf("* ");
        }
        printf("\n");
    }
    return 0;
}
```

### p118数组定义时下标需要常量



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>

#define SIZE 5

int main(void)
{
    const int index = 10；//这里const在定义/编译时也是变量。
    
    uint32_t numbers[SIZE] = { 1,2,3,4,5 };//这里SIZE必须是常量{右值}，而非变量{左值}，即数组长度固定。
   uint32_t numbers[SIZE] = { 0 };//这里把第一元素初始化为0
    printf("%" PRIu32 "\n", numbers[0]);
    
    return 0;
}
```

### p119数组的注意事项

**下面介绍数组在使用的时候容易出现的错误：**

**p119数组的注意事项**

**下面介绍数组在使用的时候容易出现的错误：**

**错误一：越界访问**



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>

#define SIZE 5

int main(void)
{
   uint32_t numbers[SIZE] = { 0 };
    printf("%" PRIu32 "\n", numbers[5]);//这里只能访问0-4的元素！
//index "5" is out of valid index range "0" to "4" for ...
    return 0;
}
```

**错误二：**


```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>

#define SIZE 5

int main(void)
{
   uint32_t numbers[SIZE] = { 0 };
    printf("%" PRIu32 "\n", numbers[5]);//这里只能访问0-4的元素！
//index "5" is out of valid index range "0" to "4" for ...
    return 0;
}
```

### 数组的案例：分数



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>

#define STUDENT_COUNT 5
int main(void)
{
    
    
    uint32_t grades[STUDENT_COUNT] = { 85, 92, 88, 96, 66 };

    uint32_t sum = 0;

    uint32_t max_grade = grades[0];

    uint32_t min_grade = grades[0];

    puts("所有人的成绩单如下：");

    for (uint32_t index = 0; index < STUDENT_COUNT; index++) {
    
        printf("学生%" PRIu32 "的成绩：%" PRIu32 "\n", index +1, grades[index]);
        
        sum += grades[index];

        if (grades[index] > max_grade) {
            max_grade = grades[index];

        }

        if (grades[index] < min_grade) {
            min_grade = grades[index];

        }
        

    }

    double average = (double)sum / STUDENT_COUNT;

    printf("\n平均成绩：%.2f\n", average);
    printf("最高成绩：%d\n", max_grade);
    printf("最低成绩：%d\n", min_grade);
    return 0;
}
```

### unicode字符编码与wchar宽字符

> wchar.h  
> unicode字符编码与wchar宽字符  
> 中文字符集为**GBK**  
> 具体存储原理不在讨论范围  
> \n ASCII码  
> 相应的，**各种语言等等使用不同编码**

### locale.h header file

> 货币的格式、时间格式等等，比如MM/DD/YYYY DD/MM/YYYY
> 
> 某些语言的字符串排序比较是不同的。

### p126 二维数组及隐式确定数组大小的初始化



```c
#include <stdio.h>
#include <inttypes.h>
int main(void) {
    //隐式确定数组大小的初始化

    int arr[] = { 0 };

    printf("%d\n", arr[1]);// 试图访问超出范围的元素
    //在vs中自动编辑占内存，会自动输出为0，编译器可能会将未使用的内存初始化为零
    //索引“1”超出了“0”至“0”的有效范围(对于可能在堆栈中分配的缓冲区“arr”)


    char greeting[] = "Hello";

    //二维数组隐式确定数组中行数可以省略的，但是前提是数组的每一个元素必须都初始化！

    int matrix[][3] = {
        {1,2,3},
        {4,5,6},
        {7,8,9},
        {0,0,0}
    };
    
    //选修：函数中确定初始化

    void processMatrix(int matrix[][3], int rows);
    for (uint32_t i = 0; i < 4; i++) {
        for (uint32_t j = 0; j < 3; j++) {
            printf("%" PRIu32 " ", matrix[i][j]);
        }
        printf("\n");
    }
    return 0;
}
```


### p124数组案例：字母次数统计



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <limits.h>
#include <ctype.h>

#define LETTER_COUNT 26

int main(void)
{
    uint32_t frequency[LETTER_COUNT] = { 0 };

    char text[] = "Example text for frequency analysis.\0";

    // 统计每个字母出现的次数
    for (uint32_t i = 0; text[i] != '\0'; i++)
    {
        char ch = tolower(text[i]);

        if (ch >= 'a' && ch <= 'z') {
            frequency[ch - 'a']++;
        }
    }

    puts("Character Frequency:\n");
    for (int i = 0; i < LETTER_COUNT; i++) {
        if (frequency[i] > 0) {
        printf("'%c': %d\n", 'a' + i, frequency[i]); 
        }
    }
    return 0;
}
```

**加强后**：



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <limits.h>
#include <ctype.h>
#include <string.h>  // 包含 strcspn 函数

#define LETTER_COUNT 26
#define MAX_TEXT_LENGTH 256  // 定义最大输入长度

int main(void)
{
    uint32_t frequency[LETTER_COUNT] = { 0 };

    char text[MAX_TEXT_LENGTH];  // 用于存储用户输入的文本

    //char text[] = "Example text for frequency analysis.\0";
    printf("请输入文本: ");
    fgets(text, sizeof(text), stdin);  // 使用 fgets 读取一行文本

    //处理换行符
    text[strcspn(text, "\n")] = '\0';  // 将换行符替换为字符串结束符


    //统计每个字母出现的次数
    for (uint32_t i = 0; text[i] != '\0'; i++)
    {
        char ch = tolower(text[i]);

        if (ch >= 'a' && ch <= 'z') {
            frequency[ch - 'a']++;
        }
    }
    puts("Character Frequency:\n");
    for (int i = 0; i < LETTER_COUNT; i++) {
        if (frequency[i] > 0) {
            printf("'%c': %d\n", 'a' + i, frequency[i]);
        }
    }
    return 0;
}
```


### 数组的知识汇总



```c
#include <stdio.h>
#include <stdint.h>
#include <inttypes.h>
#include <limits.h>
#include <ctype.h>

#define MAX_SIZE 10

int main(void)
{
    //数组 Array 连续不断
    uint32_t number_arr[MAX_SIZE] = { 0 };//初始化

    //元素 Element 每一个格子里面的数据

    //下标 Index 比如number_arr[index]

    //长度 Length/Size 指可以容纳多少的元素

    //数组的内存分布 Memory Layout uint32_t占4个字节

    //遍历 数组的值查找元素、统计信息、修改元素、排序等操作对数组中的每个元素进行访问
    return 0;
}
```

### p128五子棋棋盘的绘制



```c
#include<inttypes.h>
#include<Windows.h>
#include<stdio.h>
#include<stdint.h>
#include<locale.h>
#include<wchar.h>

#define board_size 15//定义五子棋的大小

int main(void)
{
    // 设置控制台代码页为 UTF-8
    SetConsoleOutputCP(65001);

    //设置区域为当前系统默认
    setlocale(LC_ALL, "");

    wchar_t board[board_size][board_size];
    uint8_t x, y;

    //状态
    wchar_t current_player = 0x25CF;

    //初始化棋盘
    for (uint32_t i = 0; i < board_size; i++) {
        for (uint32_t j = 0; j < board_size; j++) {
            board[i][j] = 0x00B7;
        }
    }

    
        wchar_t control = L'y';
        while (control == L'y' || control == L'Y') {
            system("cls");//清屏
            for (uint32_t i = 0; i < board_size; i++) {
                for (uint32_t j = 0; j < board_size; j++) {
                    //board[i][j] = 0x00B7;
                    wprintf(L"%lc ", board[i][j]);
                }
                printf("\n");
            }
            while (1) {
                printf("玩家 %lc 请输入坐标 (0 - %d) , 中间用空格隔开：\n", current_player, board_size - 1);
                if (scanf_s("%" SCNu8 " %" SCNu8, &x, &y) != 2) {
                    while (getchar() != '\n');
                    wprintf(L"输入无效，请重新输入。\n");
                    system("pause");
                    continue;
                }

                if (x < board_size && y < board_size && board[x][y] == 0x00B7) {
                    board[x][y] = current_player;
                    current_player = (current_player == 0x25CF) ? 0x25CB : 0x25CF;
                    break;
                }
                else {
                    wprintf(L"无效的移动，请重试！");
                    while (getchar() != '\n');//清空输入缓冲
                }
            }
            system("cls");
            for (uint32_t i = 0; i < board_size; i++) {
                for (uint32_t j = 0; j < board_size; j++) {
                    wprintf(L"%lc ", board[i][j]);
                }
                wprintf(L"\n");
            }
            wprintf(L"是否继续？ (y / n)：");
            while (getchar() != '\n');//清空输入缓冲
            wscanf_s(L" %lc", &control, 1);
        }
        printf("游戏结束了！");

    return 0;
}
```

## 第六章


## 第七章

###  131 function函数的介绍与作用

```c
#include <stdio.h>  
​  
void print_line() {  
    printf("-------");  
}  
​  
int main() {  
    // function 功能块  
​  
    // 模块化  
    // 功能抽离，达到复用性！  
    for (int i = 0; i < 5; i++)  
    {  
        printf("---\n");  
    }  
​  
    //printf("-------");  
    print_line();  
​  
    for (int i = 0; i < 5; i++)  
    {  
        printf("---\n");  
    }  
​  
    //printf("-------");  
    print_line();  
​  
    for (int i = 0; i < 5; i++)  
    {  
        printf("---\n");  
    }  
​  
    //printf("-------");  
    print_line();  
​  
    for (int i = 0; i < 5; i++)  
    {  
        printf("---\n");  
    }  
​  
    printf("-------");  
​  
    for (int i = 0; i < 5; i++)  
    {  
        printf("---\n");  
    }  
​  
    printf("-------");  
    //我们发现有一些重复的东西，相当于功能块，打包一起后可以直接调用  
    return 0;  
}

```


### 132 函数声明与定义的规则
```c
#include <stdio.h>  
​  
/*  
void print_line() {  
    printf("-------");  
}  
*/  
    //定义在前面？x  
​  
    //我们也可以先声明，后编写，再定义！  
​  
​  
​  
//先声明  
void print_line();  
​  
​  
int main() {  
    // function 功能块  
​  
    // 模块化  
    // 功能抽离，达到复用性！  
    for (int i = 0; i < 5; i++)  
    {  
        print_line();   //调用  
    }  
​  
    //printf("-------");  
    print_line();  
​  
​  
​  
      
    //我们发现有一些重复的东西，相当于功能块，打包一起后可以直接调用  
    return 0;  
}  
// 函数定义  
void print_line() {  
    printf("-------");  
}
```
### 133 函数的参数
```c
#include <stdio.h>  
#include <stdbool.h>  
​  
//但是我们想输出不同的内容怎么办？  
​  
//声明  
void greet(int age, bool gender);  
​  
void add(int num_1, int num_2);  
​  
int add_2(int num_1, int num_2);  
​  
int main() {  
    //调用  
    greet(18, 1);  
    //实际参数 18 在调用中使用  
​  
    add(7, 8);  
​  
    int number = add_2(7, 8);  
    printf("%d\n", number);  
}  
//函数定义  
void greet(int age, bool gender) {  
    //形式参数 int age  
    printf("age=%d, 性别：%s\n", age, gender ? "男" : "女");  
}  
​  
​  
void add(int num_1, int num_2) {  
​  
    printf("sum=%d\n", num_1 + num_2);  
​  
}  
​  
int add_2(int num_1, int num_2) {  
​  
    return num_1 + num_2;  
    //函数返回值与前面的int一致！  
}
```
### 134 案例：求面积函数
```c
//## 134案例：求面积函数

#include <stdio.h>
#include <stdbool.h>
#define PI 3.14

double square_area(double side);
double rectangle_area(double length, double width);
double circle_area(double redius);

int main() {

	printf("%lf\n", square_area(6.0));
	
	printf("%lf\n", rectangle_area(6.0, 7.0));

	printf("%lf\n", circle_area(6.0));

	return 0;
}

double square_area(double side) {
	return side * side;
}
double rectangle_area(double length, double width) {
	return length * width;
}
double circle_area(double redius) {
	return redius * redius * PI;
}
```
​
### 134案例：求面积函数
```c
//### 134案例：求面积函数

#include <stdio.h>
#include <stdbool.h>
#define PI 3.14

double square_area(double side);
double rectangle_area(double length, double width);
double circle_area(double redius);

int main() {

	printf("%lf\n", square_area(6.0));
	
	printf("%lf\n", rectangle_area(6.0, 7.0));

	printf("%lf\n", circle_area(6.0));

	return 0;
}

double square_area(double side) {
	return side * side;
}
double rectangle_area(double length, double width) {
	return length * width;
}
double circle_area(double redius) {
	return redius * redius * PI;
}

```


### 135 函数编写要领
```c
//## 135 函数编写要领

#include <stdio.h>
#include <stdbool.h>

bool is_leap_year(int year);

int main() {

	/*
		1.明确函数目的，明确功能将帮助你决定哪些输入是必须的（形式参数/模板），以及如何报告工作结果。

		2.识别需要的输入

		3.确定函数的输出

		4.使用适当的数据类型

		5.函数命名清晰，并且先声明

	*/
	int year = 2025;
	
	printf("%d年%s\n", year, is_leap_year(year) ? "是闰年" : "不是闰年");

	return 0;
}

bool is_leap_year(int year) {
	return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
}

```