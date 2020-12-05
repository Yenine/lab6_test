# lab6测试库
目录下的`test.out`文件为测试过程调用程序 **请不要删除**

同学们需要将自己完成的编译器程序拷贝到根目录下 默认命名为`main.out` 后面会介绍如何更改使用不同名字
## 文件结构
`test`文件夹下是不同级别的测试文件以及类型检查的测试文件
### 类型检查
对于类型检查的文件 需要对每份代码输出类型检查的结果 包含必要的报错信息以及可能的修正信息等等 使用方式控制命令部分有写到 默认的检测方式为`./main.out <file.c >file.res`
### 测试文件
通常一个完整的测试样例包含
- `file.c` 测试代码
- `file.in` 测试输入
- `file.output` 测试输出
- `file.s` 同学们的程序输出的汇编代码
- `file.out` 根据同学们翻译出的汇编代码经过编译器翻译得到的程序
- `file_std.out` 通过官方编译器编译得到的标准程序
- `file_std.output` 标准输出 作为对照使用

仓库只包含了必要的代码文件和输入文件 其他文件会在程序运行过程中产生
## 控制命令
- `make` 命令会使用默认设置进行整个测试流程(不包括类型检查) 
    - 默认设置为`x86`平台 
    - 默认测试编译器路径为`./main.out` 
    - 默认测试命令为`./main.out <file.c >file.s` 
    - 默认测试所有等级下所有样例 
    - 默认翻译汇编代码命令为`gcc file.s -m32 -o file.out`
    - 默认测试程序的命令为`qemu-i386 file.out <file.in >file.output`
    - 默认将过程中错误提示写到`err.log`中
- `make arm` 命令会将平台变更为arm平台
    - 翻译汇编代码命令变更为`arm-linux-gnueabi-gcc -static file.s -o file.out`
    - 测试程序命 令变更为`qemu-arm file.out <file.in >file.output`
- `make l1/2/3`命令单独测试不同等级样例
- `make arm-l1/2/3` 命令在arm平台下单独测试不同等级不同样例
- `make noerrlog` 命令将所有报错直接在终端输出
- `make type` 命令进行**类型检查**工作 对所有类型检查代码分别运行`./main.out <file.c >file.res`
- `make std` 命令检查所有样例(不包括类型检查) 对无输入文件的样例创建对应输入文件
- `make different` 命令对所有样例(不包括类型检查) 对未拷贝的代码拷贝出一份对应`.sy`文件 具体作用我们后文会提到
- `make clean` 命令删除编译产生的程序
- `make clean-complete` 命令删除绝大部分自己产生的文件 **慎用**
## 测试过程
- 打印当前设置
- 生成标准输出
- 测试编译器产生汇编代码
- 翻译汇编代码产生输出
- 对比标准输出与测试输出
## 一些额外的说明
查看Makefile可以知道 实际上上文提到的不同设置只是在调用`test.out`时增加了不同的参数 因此在这里说明一些额外的设置
### 我的程序不想命名为main.out/我的类型检查和编译器作业是两个程序
通过`compiler`参数可以更改测试编译器的路径 例如测试编译器命名为`haveatry.tet` 在Makefile中或直接调用时添加`--compiler ./haveatry.tet`即可更改测试编译器调用方式
### 我的IO没有支持标准格式欸/有些样例我规定的写法不允许可以更改吗
首先通过`make different`命令将所有样例拷贝为`.sy`文件 在`.sy`文件中进行修改 而后通过添加`different`参数可以令测试程序将测试命令变为`./main.out <file.sy >file.s` 在Makefile或直接调用时添加`--different`参数即可
### 我想自己添加一些样例
当然可以 直接在任何一个级别下添加你自己的c代码以及标准输入即可
### 一些自己想实现的测试方法或有其他问题
欢迎直接向助教提问或者提issue~