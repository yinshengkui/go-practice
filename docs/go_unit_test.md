# Go单元测试

## 1. Go语言单元测试框架

首先我们来了解一下go语言单元测试的基础知识。

go语言的单元测试采用内置的测试框架,通过引入testing包以及go test来提供测试功能。

在源代码包目录内，所有以_test.go为后缀名的源文件被go test认定为测试文件，这些文件不包含在go build的代码构建中,而是单独通过 go test来编译，执行。

通常对于测试用例，go test有着以下规约:

- 每个测试函数必须导入testing包。测试函数有如下的命名：

  func TestName(t *testing.T) {

  // ...

  }

- 测试函数的名字必须以Test开头，可选的后缀名必须以大写字母开头：

  func TestSin(t *testing.T) { /* ... */ }

  func TestCos(t *testing.T) { /* ... */ }

  func TestLog(t *testing.T) { /* ... */ }

- 将测试文件和源码放在相同目录下,并将名字命名为{source_filename}_test.go

假设被测试文件example.go,那么在example.go相同目录下建立一个example_test.go的文件去测试example.go文件里的方法。

- 通常情况下，将测试文件和源码放在同一个包内。

当运行go test命令时，go test会遍历所有的*_test.go中符合上述命名规则的函数，然后生成一个临时的main包用于调用相应的测试函数，然后构建并运行、报告测试结果，最后清理测试中生成的临时文件。

了解了基础知识，我们举个例子来直观了解在go项目中编写单元测试的全过程，下图是一个项目src/utils包下源码的结构:

[![img](https://testerhome.com/photo/2016/8f8a51d472e0726e05761854f157415a.png)](https://testerhome.com/photo/2016/8f8a51d472e0726e05761854f157415a.png)

对于每个包下的每一个文件都有相应的test文件。对于slice.go这个文件，源码是这样的:

[![img](https://testerhome.com/photo/2016/1feccec0b3bc3a2af9a74de21fd4214b.png)](https://testerhome.com/photo/2016/1feccec0b3bc3a2af9a74de21fd4214b.png)

StringInSlice这个函数去检查一个字符串是否在一个字符串列表中，输入一个string,返回一个布尔值。

slice_test.go中对StringInSlice的测试用例是这样的:

[![img](https://testerhome.com/photo/2016/d677840b22dd7a88d85f0083fb50b0c0.png)](https://testerhome.com/photo/2016/d677840b22dd7a88d85f0083fb50b0c0.png)

然后运行测试，运行方式有多种：

- 当只想测试slice_test.go文件时, 使用命令:

```
# -v是显示出详细的测试结果, -cover 显示出执行的测试用例的测试覆盖率。

go test -v -cover=true ./src/utils/slice_test.go ./src/utils/slice.go


```

执行结果:

[![img](https://testerhome.com/photo/2016/cab937e5658439d7d82adaa056de57e0.png)](https://testerhome.com/photo/2016/cab937e5658439d7d82adaa056de57e0.png)

- 当测试整个utils包时，使用命令:

  ```
  go test -v -cover=true ./src/utils/...

  ```

- 当测试单个测试用例时，使用命令:

```
#./src/utils为包utils的路径
go test -v -cover=true ./src/utils -run TestSuccessStringInSlice

```

## 2. 自动生成表格驱动的测试用例

在go语言中表格驱动测试非常常见。表格驱动的测试用例是在表格中预先定义好输入，期望的输出，和测试失败的描述信息，

然后循环表格调用被测试的方法，根据输入判断输出是否与期望输出一致，不一致时则测试失败, 返回错误的描述信息。

这种方法易于覆盖各种测试分支 ，测试逻辑代码没有冗余，开发人员只需要向表格添加新的测试数据即可。

对于适用于表格驱动测试的源码，我们采用开源工具[gotests](https://github.com/cweill/gotests)来自动生成测试用例。拿src/utils/slice.go为例，开发环境安装gotests，

然后运行gotests -all -w slice.go, slice_test.go会自动创建在当前目录下，并自动生成测试代码:

[![img](https://testerhome.com/photo/2016/b41c0cfa3753a4d29e1923f2093e19d9.png)](https://testerhome.com/photo/2016/b41c0cfa3753a4d29e1923f2093e19d9.png)

开发人员只需要将不同的测试数据按照tests定义的结构写在//TODO:Add test cases下面，测试用例就完成了。

## 3. mock的使用实践

mock是单元测试中常用的一种测试手法，mock对象被定义，并能够替换掉真实的对象被测试的函数所调用。

而mock对象可以被开发人员很灵活的指定传入参数，调用次数，返回值和执行动作，来满足测试的各种情景假设。

那什么情况下需要使用mock呢?一般来说分这几种情况:

1. 依赖的服务返回不确定的结果，如获取当前时间。
2. 依赖的服务返回状态中有的难以重建或复现，比如模拟网络错误。
3. 依赖的服务搭建环境代价高，速度慢，需要一定的成本，比如数据库，web服务
4. 依赖的服务行为多变。

为了保证测试的轻量以及开发人员对测试数据的掌控，采用mock来斩断被测试代码中的依赖不失为一种好方法。

每种编程语言根据语言特点其所采用的mock实现有所不同。

在go语言中，mock一般通过两种方法来实现，一种是依赖注入，一种是通过interface,下面我们分别通过例子来说明这两种技术实践。

### 3.1 依赖注入

依赖注入为一个类或者函数A，用到内部对象B，B在A的外部创建，当运行A调用B时，通过某种方式将外部创建的B的实例赋给A内的B。

这样当A调用B时，B就会按照外部定义的方式去运行。下面是一个例子:

[![img](https://testerhome.com/photo/2016/1a7e5a09a22729a1d54c43ef3e55704b.png)](https://testerhome.com/photo/2016/1a7e5a09a22729a1d54c43ef3e55704b.png)

在测试CheckQuota时，我们看到其函数体内有一个依赖notifyUser, notifyUser是用来向用户发送email信息，

在测试时，我们当然不希望发送真实的邮件, 因此，需要创建一个伪邮件发送函数替代真实的邮件发送函数。

[![img](https://testerhome.com/photo/2016/4fb42a853cc2cccb5810156738cd9673.png)](https://testerhome.com/photo/2016/4fb42a853cc2cccb5810156738cd9673.png)

上图中TestCheckQuotaNotifiesUser中定义的匿名函数就是一个伪邮件发送函数，用户自定义伪邮件函数的行为,然后替换真实的邮件发送函数。

在这里需要特别注意的是，在notifyUser被伪邮件函数赋值前，需要将原来的值存下来,测试用例执行完之后再赋回去, 否则notifyUser的行为将会全局改变。

关于示例代码的详情请参考go语言圣经[白盒测试](http://gopl-zh.b0.upaiyun.com/ch11/ch11-02.html)部分。

### 3.2 gomock的使用

除了依赖注入，另一种mock实现是通过go语言的interface，被mock的对象需要继承interface,并在interface中定义好被mock对象的方法。

mock对象通过实现interface的所有方法来表明自己实现了这个interface，这样mock对象的值就可以替换被mock对象的值。

对于mock对象我们可以自己定义实现，也可以通过工具实现。开源软件gomock[3](https://testerhome.com/topics/images/slice_test.png)可以根据指定的interface自动生成mock对象，
并对mock对象自定义行为和返回结果，检查被调用次数，是一款非常好用的工具。

下面通过一个简单的示例来描述如何使用gomock工具。

1. 首先从github上获取gomock的相关源码包，并将其放在项目的vendor目录中。

   ```
   go get github.com/golang/mock/gomock
   go get github.com/golang/mock/mockgen

   ```

2. 将需要mock的方法放在interface中,使用mockgen命令指定接口实现mock接口,命令为:

   ```
   mockgen -source {source_file}.go -destination {dest_file}.go

   ```

3. 之后就可以初始化并使用dest_file.go里生成的mock接口来自定义被mock示例调用的方法的行为。

[![img](https://testerhome.com/photo/2016/399a4b4b1fbf8aa19ecb2772b2c52e54.png)](https://testerhome.com/photo/2016/399a4b4b1fbf8aa19ecb2772b2c52e54.png)

看图中的代码，appControllerInterface被gomock实现,生成mock对象mockControllerImpl来替代mockAppImpl.

mockControllerImpl对Applications方法的定义是不管参数是何值，当Applications被调用时会返回nil, nil，并且此方法只能被调用1次。

当mockAppImpl的Applications方法被调用时,gomock会根据预先定义的行为给出返回值，做出判断。

对于gomock的详细使用情况，可参考源码设计[gomock](https://github.com/rafrombrc/gomock)

## 4. 云平台产品测试实践

这里有个关键词:云平台, 大家会疑惑: 云平台的测试和其他产品的测试有什么不同吗?

云平台的产品有这样的特点，其底层依赖的基础服务会比较多，且难以mock。

比如本公司的开源产品crane和swan, [crane](https://github.com/Dataman-Cloud/crane)是基于docker swarm实现的容器管理工具，其底层紧密的依赖docker容器。

而[swan](https://github.com/Dataman-Cloud/swan)是基于mesos集群的调度器，其底层会紧密依赖mesos，同样由于基于docker 容器技术，也会紧密依赖docker容器来实现调度。

对于调用底层docker接口的代码，在源码结构设计上，可以将这一部分代码封装成自定义的dockerclient, dockerclient继承interface,
这样dockerclient可以通过mock被自定义的dockerclient替代，从而斩断依赖, 测试上层逻辑代码。

那么如何测试dockerclient呢, dockerclient的代码直接调用了docker endpoint的api，而docker对象并没有办法mock,我们采用的方法是创建mock server。

通过定义request url, request method 来自定义返回的response status code和body。

为了提高编写测试用例的效率，团队写了一个通用的[mock-server](https://github.com/Dataman-Cloud/mock-server)工具来供大家使用。

mock-server的优点是支持多种形式的request body和response body的数据定义, 支持的形式有string, interface, json，file, io.Reader，

这样开发人员在定义body的数据时，可以选择自己熟悉便利的方式来定义。

例如crane中定义docker endpoint服务来测试ListContainersID方法的代码如下:

[![img](https://testerhome.com/photo/2016/e575dc649a165b8f70774cd6902a8555.png)](https://testerhome.com/photo/2016/e575dc649a165b8f70774cd6902a8555.png)

由此可看到，mock-server在解决reset api的依赖中是非常便利的。

### 5. CI搭建和代码覆盖率

在团队认识到单元测试是控制产品质量和促进良好代码结构的重要手段后，我们开始把单元测试代码覆盖率作为代码提交的第一道审查防线。

开发提交的pr通过github webhook触发jenkins CI job，运行go test，查看新的更改是否能通过单元测试，并且能获得当前每个文件的单元测试覆盖率。

当然在github中也可以通过travis CI来实现这样的控制。我们可以把运行测试和获取单元测试覆盖率的命令在Makefile中实现, 然后根据需要搭建CI环境。

[![img](https://testerhome.com/photo/2016/5bd56bd1403af873449ae0c3f6cfa244.png)](https://testerhome.com/photo/2016/5bd56bd1403af873449ae0c3f6cfa244.png)

在makefile中，需要注意的是go test是按package计算测试覆盖率的，如果想获得整个项目的覆盖率，首先需要列出项目内的packages，然后遍历packages，通过聚合统计文件coverage.out，最后算出整个项目的覆盖率。

***来自数人云2016年11月的分享，原文地址https://testerhome.com/topics/6374***