## protobuf安装及golang使用protobuf

### 1. Proto Buffer 介绍

**Google Protocol Buffer**(简称 Protobuf)是google旗下的一款轻便高效的结构化数据存储格式，平台无关、语言无关、可扩展，可用于通讯协议和数据存储等领域。所以很适合用做数据存储和作为不同应用，不同语言之间相互通信的数据交换格式，只要实现相同的协议格式即同一 proto文件被编译成不同的语言版本，加入到各自的工程中去。这样不同语言就可以解析其他语言通过 protobuf序列化的数据。目前官网提供了 C++,Python,JAVA,GO等语言的支持。google在2008年7月7号将其作为开源项目对外公布。

**tips：**

1.  啥叫平台无关？Linux、mac和Windows都可以用，32位系统，64位系统通吃 
2.  啥叫语言无关？C++、Java、Python、Golang语言编写的程序都可以用，而且可以相互通信 
3.  那啥叫可扩展呢？就是这个数据格式可以方便的增删一部分字段啦~ 
4.  最后，啥叫序列化啊？解释得通俗点儿就是把复杂的结构体数据按照一定的规则编码成一个字节切片

### 2. 数据交换格式

#### A)常用的数据交换格式有三种：

1.  `json`: 一般的web项目中，最流行的主要还是 json。因为浏览器对于json 数据支持非常好，有很多内建的函数支持。 
2.  `xml`: 在 webservice 中应用最为广泛，但是相比于 json，它的数据更加冗余，因为需要成对的闭合标签。json 使用了键值对的方式，不仅压缩了一定的数据空间，同时也具有可读性。 
3.  `protobuf`: 是后起之秀，是谷歌开源的一种数据格式，适合高性能，对响应速度有要求的数据传输场景。因为 profobuf 是二进制数据格式，需要编码和解码。数据本身不具有可读性。因此只能反序列化之后得到真正可读的数据。 

#### B)protobuf的优势与劣势

**优势：**

1. 序列化后体积相比Json和XML很小，适合网络传输

2. 支持跨平台多语言

3. 消息格式升级和兼容性还不错

4. 序列化反序列化速度很快，快于Json的处理速速

**劣势：**

1. 应用不够广(相比xml和json)

2. 二进制格式导致可读性差

3. 缺乏自描述 

### 3. 安装 Go 语言环境 和 安装 protoc 编译器

首先要保证 Go 语言开发环境已安装妥当。你可以从[Go 官网](https://go.dev/dl/)下载适合你操作系统的安装包，也可以借助包管理器来安装。

1、下载二进制包：go1.24.linux-amd64.tar.gz。

2、将下载的二进制包解压至 /usr/local目录

3、将 /usr/local/go/bin 目录添加至 PATH 环境变量：

```bash
编辑 ~/.bashrc文件，添加以下命令到该文件末尾
export PATH=$PATH:/usr/local/go/bin

添加后执行：
source ~/.bashrc
```

安装完成之后，使用下面的命令检查 Go 的版本：

```bash
go version
```

同时，要把`$GOPATH/bin`添加到系统的环境变量`PATH`中，这样就能正常使用 Go 安装的工具了。

----

`protoc`是 Protocol Buffers 的编译器，其作用是生成 gRPC 客户端和服务器的代码。你可以从[Protocol Buffers releases](https://github.com/protocolbuffers/protobuf/releases)下载与你系统相对应的安装包，

安装包里有可执行文件将其加入环境变量，可以编辑`vim ~/.bashrc`

```bash
export PROTOC_DIR=/home/guodun/download/protoc/bin

export PATH="$PATH:$PROTOC_DIR"
```

安装完成后，通过以下命令验证安装是否成功：

```bash
protoc --version
```

### 4、安装Go语言的protoc插件和依赖

接下来，安装 Go 语言的 Protocol Buffers 和 gRPC 插件，命令如下：

```bash
go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.31.0
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.3.0
```

这两个命令会将插件安装到 `$GOPATH/bin` 目录下，确保 `protoc` 编译器能够找到它们。

---

**获取 Protobuf 和 gRPC 库**

虽然在执行 `go install` 时会自动下载依赖的库，但为了确保项目正确引用这些库，还需要在项目中添加对它们的依赖声明。具体步骤如下：

首先，在项目根目录下初始化 Go 模块（如果尚未初始化）：

```bash
go mod init your_project_path
```

然后，使用 `go get` 命令显式获取 Protobuf 和 gRPC 库：

```bash
go get google.golang.org/protobuf
go get google.golang.org/grpc
```

最后，运行 `go mod tidy` 命令整理依赖，确保 `go.mod` 和 `go.sum` 文件正确记录所有依赖：

```bash
go mod tidy
```

**为什么需要这些库**

- `google.golang.org/protobuf`：提供 Protocol Buffers 的核心功能，包括消息序列化和反序列化。
- `google.golang.org/grpc`：提供 gRPC 框架的实现，包括客户端和服务器的通信机制。

在生成的代码（如 `hello.pb.go` 和 `hello_grpc.pb.go`）中，会引用这些库的包。如果项目中没有正确声明这些依赖，编译时会报错

###  5. 编写示例

整体demo目录结构展示：

```bash
(base) guodun@hfut-101:~/test/go$ tree lib_protobuf/
lib_protobuf/
├── go.mod
├── go.sum
├── lib_protobuf.go
├── pb
│   └── examplemsg.pb.go
└── srcproto
    └── examplemsg.proto
```

简单解释下，`examplemsg.proto` proto源文件，`examplemsg.pb.go` 是proto导出文件，`lib_protobuf.go` 是proto协议使用和测试文件

#### A)示例初始化

执行下面的命令，创建示例基础目录文件结构

```bash
$ mkdir lib_protobuf
$ cd lib_protobuf
$ mkdir srcproto
$ touch srcproto/examplremsg.proto
$ touch lib_protobuf.go
$ go mod init lib_protobuf
go: creating new go.mod: module lib_protobuf
go: to add module requirements and sums:
        go mod tidy

$ go mod tidy
$ ls
go.mod  lib_protobuf.go  srcproto
```

#### B)编写协议文件内容

先编写proto文件，`vim srcproto/examplremsg.proto`，内容如下：

```protobuf
syntax = "proto3"; 						//指定版本信息，不指定会报错
option go_package = "./pb";	   			//后期生成go文件的包名

//message为关键字，作用为定义一种消息类型
message Person {
	string	name = 1;					//姓名
        int32	age = 2;					//年龄
	repeated string emails = 3; 		//电子邮件（repeated表示字段允许重复）
	repeated PhoneNumber phones = 4;	//手机号
}

//enum为关键字，作用为定义一种枚举类型
enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
}

//message为关键字，作用为定义一种消息类型可以被另外的消息类型嵌套使用
message PhoneNumber {
    string number = 1;
    PhoneType type = 2;
}
```

#### C)执行导出命令

```bash
protoc --proto_path=srcproto --go_out=. examplemsg.proto
```

使用`protoc`将proto文件导出golang可用的文件最重要的两个参数如下：

`--proto_path`：proto源文件所在文件夹
`--go_out`：proto文件导出的目标文件夹



执行完之后生成`pb/examplemsg.pb.go`

> 注意：
>
> - 官方推荐是用直接在.proto文件中添加 `go_package` 选项（或者在生成的命令上添加）
> - `go_package` 要求必须至少包含一个 `.` 或者 `/`，那么把选项改成`"./pb"`；有时为了在当前目录下生成，又要指定包名，可以写成这样`option go_package = "./;msgproto";`

#### D)利用protobuf生成的类来编码

在文件 lib_protobuf.go 中编写如下内容，执行 `go mod tidy` 和 `go run .` 查看输出结果

```go
package main

import (
	"fmt"
	"google.golang.org/protobuf/proto"
	"lib_protobuf/pb"
)

func main() {
	person := &pb.Person{
		Name:   "Aceld",
		Age:    16,
		Emails: []string{"https://legacy.gitbook.com/@aceld", "https://github.com/aceld"},
		Phones: []*pb.PhoneNumber{
			&pb.PhoneNumber{
				Number: "13113111311",
				Type:   pb.PhoneType_MOBILE,
			},
			&pb.PhoneNumber{
				Number: "14141444144",
				Type:   pb.PhoneType_HOME,
			},
			&pb.PhoneNumber{
				Number: "19191919191",
				Type:   pb.PhoneType_WORK,
			},
		},
	}
    
	// 序列化
	data, err := proto.Marshal(person)
	if err != nil {
		fmt.Println("marshal err:", err)
	}

    // 反序列化
	newdata := &pb.Person{}
	err = proto.Unmarshal(data, newdata)
	if err != nil {
		fmt.Println("unmarshal err:", err)
	}

	fmt.Println(newdata)

}
```

运行结果：

![](https://pic1.imgdb.cn/item/68839c2f58cb8da5c8df1ed0.png)





> 参考链接：
>
> 1. https://www.yuque.com/aceld/npyr8s/kiydp7
> 2. http://www.008ct.top/blog/2024/02/18/%E5%9C%A8golang%E8%AF%AD%E8%A8%80%E4%B8%AD%E7%AE%80%E5%8D%95%E4%BD%BF%E7%94%A8protobuf%E6%97%B6%E9%81%AD%E9%81%87go-package%E5%9B%B0%E9%9A%BE%E9%87%8D%E9%87%8D/
