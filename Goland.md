
# 配置go环境
1. 解压go包
2. 配置GOPATH
3. 如果需要下载包 `golang.org/x/net/context --> github.com/golang/net/context`

# goland(IDE)快捷操作
| 功能 | 操作 |
|:------------- |:---------------:|
| 删除当前行 | ⌘⌫ |
| 行注释 | ⌘/ |
| 块注释 | ⌥⌘/ |
| 函数使用提示 | ⌘P |
| 代码检查并提供快速修复 | ⌥⏎ |
| 切换window | ⌥⌘` |
| 切换指定位置 | ⌘⌥ 方向键 |

# 常用操作

## swag

```
swag init --parseDependency
swag init
```

## mod

```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct

go mod download
go mod vendor
```

# 导入包

## 集合导入包数据

```
import(
    "fmt"
    "math/rand"
    …… ……
)
```

| 包名 | 介绍 | 作用 |
|:------------- |:---------------:|:-------------:|
| fmt | 基本包 | 用于输入输出 |
| math | 数学基本包 | |
| 表内容 | 表内容 | ```代码内容```|

# 函数

| 函数名 | 作用 |
|:------------- |:---------------:|
| fmt.Println(String) | 输出指定字符串 |

## 函数指定返回值

```
func 函数名(参数1，参数2 数据类型) (返回数据1,返回数据2 数据类型){
    …… ……
    return
}

# example
func add(x , y int) (y , x) {
	return 
}

```

## 函数值指定返回数据类型

```
func 函数名(参数1，参数2 数据类型) (返回数据1的类型,返回数据2的类型){
    …… ……
    return 返回数据1 , 返回数据2
}

# example
func add(x , y int) (int , int) {
	return x + y , x
}
```

 函数返回值类型可以是func函数。
在函数中修改变量一般不影响全局的值，所以如果要改，必须传递一个指针进来，这个和C++相同
如果只返回一个返回数据，可以不写括号`func 函数名(参数1，参数2 数据类型) 返回数据类型{……}`

## 延迟函数

最后执行下面这个函数，多个defer的时候，是采用堆栈逻辑，原理是先进后出执行

```
defer 函数()
```

## 闭包

一个函数的返回值是一个函数。

原理：函数中回调一个子函数，可以理解为匿名函数，因为父函数被子函数依赖，所以其函数中定义的变量会一直存在内存中，每次调用函数后产生变化都是存在的。

可以把闭包理解为一个封装的环境中执行某个函数，每次定义一个变量存储这个环境，每次使用这个变量就是执行这个闭包中的子函数。

所以一般父函数中做的是环境定义，子函数做的是操作执行，如果定义两个变量，这两个变量中的值是相互不影响的。

对于执行的理解，可以是 `f := function()` 理解为执行父函数， `f()` 理解为执行子函数

```
func 函数名() func(参数类型) 返回值类型{
    相关操作1
    return func(参数 参数类型) 返回值类型{
        相关操作2
        return 返回值
    }
}

# example
func function() func() {
    a := 1
    fmt.Println("只有第一次执行函数的时候才会触发")
    return func(){
        a ++
        fmt.Println("每次执行这个函数都会执行，并且a变量值会一直存在")
    }
}

# 定义一个变量存储这个函数
f := function()
f()     // 这就是执行一次闭包
```
   闭包可以有值输入或输出
```
func function() func(int) int{
    a := 1
    fmt.Println("只有第一次执行函数的时候才会触发")
    return func(x int) int{
        a += x
        fmt.Println("每次执行这个函数吧输入的数加入")
        return a
    }
}

# 定义一个变量存储这个函数
f := function()
f(12)     // 这就是执行一次闭包
```


# 变量

  定义的所有变量都要被使用 ，否则会报错 
## 变量类型设定

```
变量名1 变量类型1 , 变量名2 变量类型2

# 当变量类型相同的时候，等同如下
变量名1 , 变量名2 变量类型

# example
x int , y int    <====>    x , y int
```

## 定义变量
```
var 变量名1 , 变量名2 变量类型

# example
var x , y bool
```


## 变量初始化赋值
```
var 变量名1 , 变量名2 变量类型 = 值1 , 值2

# 系统自动获取变量类型
var 变量名1 , 变量名2 = 值1 , 值2

# example
var x , y int = 1 , 2
var x , y = 1 , 2
var (
    x = 1
    y = 2
)
```
下面是简洁写法，但是不能在函数外使用
```
变量1 , 变量2 := 值1 , 值2

# example
x , y := 1 , 2
```

## 变量类型

| 变量类型 | 含义 | 默认值 | 代表符号|
|:------------- |:---------------:|:---------------:|:---------------:|
| bool | 代表ture 或者 false| false | %t |
| string | 代表字符串 | (空) | %s |
| int  int8  int16  int32  int64 uint uint8 uint16 uint32 uint64 uintptr | 整数类型 | 0 | | %d |
| byte  |  uint8 的别名 | 0 | %d |
| rune  |  int32  | 0 | %d |
| float32 float64 | 浮点数 |  0 | %f |
| complex64 complex128 | 虚数 | (0+0i) |

## 特殊运算
```
<< 所有二进制数向左移动n位
>> 所有二进制数向右移动n位
```
### 类型装换

表达式 `T(v)` 将值 `v` 转换为类型 `T`

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)


#string到int
int,err:=strconv.Atoi(string)

#string到int64
int64, err := strconv.ParseInt(string, 10, 64)

#int到string
string:=strconv.Itoa(int)

#int64到string
string:=strconv.FormatInt(int64,10)

# byte到string
str:=string(data)

#string到float
v2, err := strconv.ParseFloat(v, 64)
```

## 获取变量

```
test string
flag.StringVar(&test, "外部调用的名字", "默认值", "help提示文字")
flag.Parse()
// 然后test变量就会通过外部获取值

```

解决没有输入参数的异常
```
	if mip == "Error" || cip == "Error" || vip == "Error" {
		flag.Usage()
		return
	}
```

### 常量定义

```
const 常量1 , 常量2 = 值1 , 值2

# 可以大批量复制
```

## 指针

其零值是 `nil`

```
*类型 指代某类型的指针

```

```
# example
var i *int
```

那么 i 的值就是 nil ;
在不赋值去情况下使用 *i 会报告异常



# 循环

## 基本for循环

同C++的for

```
for i := 初始值 ; i < 最大值 ; i ++ {
    …………
}

```
## 初始值和数据变化模块不写

语法规则，同C++的while

```
for i < 值 {
}
```

## 死循环

```
for {
}
```

## 用range和数组相结合

会遍历整个数组中所有元素，然后每次都会把一个数据给变量2，而变量1会每次+1

```
for 变脸1,变量2 := range 数组名{
    …… ……
}
```
变量1用来记录需要，可以不写


# 判断
```
if 判断语句 {
} else {
}
```
 判断语句中可以有变量定义，例如：
 ```
if err:=test();err != nil {
} else {
}
 ```

## switch - case

```
switch i {
case "值1":
    ……
case 函数名():
    ……    
defalut:
    ……
}
```


# 结构体

可以理解为一个自定义的数据类型

```
type 结构体名 struct{
    变量1 类型
    变量2 类型
}

# 使用方法
结构体.变量

```
## 结构体指针

```
结构体变量 := 结构体{值1,值2……}
指针名 := &结构体变量

# example
a := add{1,2}
b := &a
b.x 的值就是1

```

**go没有类的说法,但是可以就结构体进行函数方法的定义**

语法类似于闭包，函数的返回值是一个函数。

```go
func (结构体变量 结构体类型) 方法名()  返回值类型{
    return 指定类型的值
}

结构体变量 := 结构体…………
结构体变量.方法名()

# example
type Vertex struct {
	X int
	Y int
}

func (v *Vertex) Abs() float64 {
	sum := v.X * v.X + v.Y * v.Y
	return math.Sqrt(float64(sum))
}

func main()  {
	v := Vertex{4,3}
	fmt.Println(v.Abs())
}
```

不光可以对自定义的结构体进行定义，也可以对数据类型，但是需要修改一个名字

```
type Vertex float

func (v Vertex) Abs() float64 {
	return math.Sqrt(float64(v))
}

```

## 反射对象

```go
	var conf configJson
  // 反射对象configJson，遍历获取所有值
	for i := 0; i < reflect.ValueOf(conf).NumField(); i++ {
		name := reflect.TypeOf(conf).Field(i).Tag.Get("json")
		flag.StringVar(&p[i], name, "error", "输入"+name+"的值")
	}
	flag.IntVar(&routing, "route", 3, "输入上传并发数")
	flag.StringVar(&tmpFileDir, "tmpDir", "/work/tmp/uploadToOss", "输入临时文件存放位置")
	flag.StringVar(&module, "module", "check", "输入使用的功能模块(upload、check)")

	// 检验是否所有值都存在
	flag.Parse()
	for i := 0; i < reflect.ValueOf(conf).NumField(); i++ {
		reflect.ValueOf(&conf).Elem().Field(i).SetString(p[i])
		if reflect.ValueOf(conf).Field(i).String() == "error" {
			flag.Usage()
			ErrorLog.Println("please input: ", reflect.TypeOf(conf).Field(i).Tag.Get("json"))
			os.Exit(-1)
		}
	}
```

用于遍历一个对象内所有元素信息，进行相关操作

## 对象操作

```go
// 为NatHostsMapType这个类定义Exist方法
func (m *NatHostsMapType) Exist(ht HostsTable) bool {
	m.Lock.Lock()
	defer m.Lock.Unlock()

	if _, isError := m.Data[ht.IP]; isError {
		return true
	} else {
		return false
	}
}
```



# 数组 slice

其空值为 nil

```
# 基本格式
数组名 []数据类型

# make创建一个数组格式
数组名 := make([]数据类型 , len初始值 ， cap初始值)

# 新添加元素
数组名 = append(数组名,值1,值2)
```

len默认值是0  ,  cap默认值是0

## 切片相关概念

len代表slice实际元素的个数；
cap代表当前slice的容量

当len的长度不足够的时候，可以直接把数据存到容量中，如果达到初始设定容量，就会添加新加一个不比初始容量小的新容量，添加的容量无上限。

1. 原则：cap>=len
2. cap一定是偶数
3. cap要么和初始cap相同，要么是>=(cap初始值*2)


## 键名映射

一般和make连用，这个不要求有数据

```
数组名 = make(map[键名类型]数组类型)

# 赋值的时候
数组名[任意键名] = 满足数组类型值
```

主要作用就是让数组的键名(下标)，可以被自定义

如果有数据

```
 数组名 = map[键名类型]数组类型{
     键名1 : {值1 , 值2}
     键名2 : {值1 , 值2}
}
```

### 删除某键名的元素

```
delete(数组，键名)
```

### 查看某键名的元素是否存在

```
变量名 , 存储状态变量 := 数组名[键名]
变量名 , 存储状态变量 = 数组名[键名]
```

# 接口

接口类型是由一组方法定义的集合。 接口类型的值可以存放实现这些方法的任何值。
但是不需要完成接口内方法的具体内容。

```
type 接口名 interface{
    方法名() 方法返回值
}

```

# 日志处理

```go
  var InfoLog   *log.Logger
	file, err := os.OpenFile(UpdateLog(LogFileName), os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0666)
	if err != nil {
		log.Fatalln(UpdateLog(LogFileName) + " 文件无法打开")
	}
	InfoLog = log.New(file,
		"",
		log.Ldate|log.Ltime|log.Lshortfile)
```

# Json处理

json文件结构体

```
type json结构体名 struct {
	代表名1(开头大写)      string `json:"json名"`
	代表名2(开头大写)      int    `json:"json名"`
}
```

将string解读为json数据

```
// 解析json格式的string信息
func ReadStringToJson(body string) json格式 {
	var jsonfile json格式
	if err := json.Unmarshal([]byte(body), &jsonfile); err != nil {
		log.Fatal(err.Error())
	}
	return jsonfile
}
```

创建一个json文件

```
	jsonfile := json格式 {
		代表名1: "值",
		代表名2: 值,
		...
	}
```

将json格式转化为string

```
func (mj json格式) ReadJson() (string) {
	js, _ := json.Marshal(&mj)
	return string(js)
}
```

断言处理，获取需要的Json数据

```go
func ReadProjectJsonString(jsonData string) {
	var v interface{}
	if err := json.Unmarshal([]byte(jsonData), &v); err != nil {
		log.Fatalln(err)
	}
	data := v.(map[string]interface{})
	data_2 := data["data"].(map[string]interface{})
	data_3 := data_2["data"].([]interface{})
	for k, v := range data_3 {
		fmt.Println(k, fmt.Sprintf("%v",k))
	}
}
```

# 异常处理

```
if err := buildMha() ; err != nil {
    // 程序继续，中断函数
	log.Panic(err)

	// 直接切断程序
	log.Fatal(err)
}
```


# 写文件

## 覆盖写

```go
    err := ioutil.WriteFile("test.txt", []byte("Hi\n"), 0666)
    if err != nil {
        log.Fatal(err)
    }
```

## 追加写

```go
func myWrite(config string, word ...string) error {
	for _, i := range word {
		f, err := os.OpenFile(config, os.O_WRONLY, 0644)
		if err != nil {
			fmt.Println("cacheFileList.yml file create failed. err: " + err.Error())
			return errors.New(i)
		} else {
			// 查找文件末尾的偏移量
			n, _ := f.Seek(0, os.SEEK_END)
			// 从末尾的偏移量开始写入内容
			_, err = f.WriteAt([]byte(i+"\n"), n)
		}
		_ = f.Close()

		if err != nil {
			fmt.Println("cacheFileList.yml file writed failed. err: " + err.Error())
			return errors.New(i)
		}
	}
	return nil
}
```

# Linux

## ssh连接服务器
```go
func connect(user, password, host, key string, port int, cipherList []string) (*ssh.Session,*ssh.Client, error) {
	var (
		auth         []ssh.AuthMethod
		addr         string
		clientConfig *ssh.ClientConfig
		client       *ssh.Client
		config       ssh.Config
		session      *ssh.Session
		err          error
	)
	// get auth method , 用秘钥或者密码连接
	auth = make([]ssh.AuthMethod, 0)
	if key == "" {
		auth = append(auth, ssh.Password(password))
	} else {
		pemBytes, err := ioutil.ReadFile(key)
		if err != nil {
			return nil, nil, err
		}

		var signer ssh.Signer
		if password == "" {
			signer, err = ssh.ParsePrivateKey(pemBytes)
		} else {
			signer, err = ssh.ParsePrivateKeyWithPassphrase(pemBytes, []byte(password))
		}
		if err != nil {
			return nil,nil, err
		}
		auth = append(auth, ssh.PublicKeys(signer))
	}

	if len(cipherList) == 0 {
		config = ssh.Config{
			Ciphers: []string{"aes128-ctr", "aes192-ctr",
				"aes256-ctr", "aes128-gcm@openssh.com", "arcfour256",
				"arcfour128", "aes128-cbc", "3des-cbc", "aes192-cbc", "aes256-cbc"},
		}
	} else {
		config = ssh.Config{
			Ciphers: cipherList,
		}
	}

	clientConfig = &ssh.ClientConfig{
		User:    user,
		Auth:    auth,
		Timeout: 30 * time.Second,
		Config:  config,
		HostKeyCallback: func(hostname string, remote net.Addr, key ssh.PublicKey) error {
			return nil
		},
	}

	// ssh 连接
	addr = fmt.Sprintf("%s:%d", host, port)

	if client, err = ssh.Dial("tcp", addr, clientConfig); err != nil {
		return nil,nil, err
	}

	// create session
	if session, err = client.NewSession(); err != nil {

		return nil, client , err
	}

	modes := ssh.TerminalModes{
		ssh.ECHO:          0,     // disable echoing
		ssh.TTY_OP_ISPEED: 14400, // input speed = 14.4kbaud
		ssh.TTY_OP_OSPEED: 14400, // output speed = 14.4kbaud
	}

	if err := session.RequestPty("xterm", 80, 40, modes); err != nil {
		return nil,nil, err
	}

	return session, client ,nil
}
const (
	username = "root"
	password = ""
	key      = "/root/.ssh/id_rsa"
	port     = 22
)

func sshDoShell(ip string, cmd string) error{
	ciphers := []string{}
	session, client, err := connect(username, password, ip, key, port, ciphers)

	if err != nil {
		fmt.Println("连接 ", ip, " 异常")
		log.Fatal(err)
	}
	
	defer func() {
		if err := session.Close(); err != nil {
			// log etc
		}
	}()
	
	defer func() {
		if err := client.Close(); err != nil {
			// log etc
		}
	}()

	session.Stdout = os.Stdout
	session.Stderr = os.Stderr

	err = session.Run(cmd)
	if err != nil{
		return errors.New(err.Error())
	}

	return nil
}
```

## 执行shell命令

```go
func myCmd(bash string, shell ...string) error {
	contentArray := make([]string, 0, 5)
	cmd := exec.Command(bash, shell...)

	stdout, err := cmd.StdoutPipe()
	if err != nil {
		fmt.Println(cmd.Stderr, "error=>", err.Error())
	}

	_ = cmd.Start()

	reader := bufio.NewReader(stdout)

	contentArray = contentArray[0:0]
	var index int
	//实时循环读取输出流中的一行内容
	for {
		line, err2 := reader.ReadString('\n')
		if err2 != nil || io.EOF == err2 {
			break
		}
		fmt.Print(line)
		index++
		contentArray = append(contentArray, line)
	}
	err = cmd.Wait()

	if err != nil {
		fmt.Printf("Execute Shell %s: ", shell)
		return errors.New("failed with error:"+err.Error())
	}

	return nil
}
```

## 基于http协议的API接口实现
```go
var ListenSig  = make(chan int) 

type Route struct {
	Name        string
	Method      string
	Pattern     string
	HandlerFunc http.HandlerFunc
}

type Routes []Route

// 记录访问记录
func Logger(inner http.Handler, name string) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		start := time.Now()
		inner.ServeHTTP(w, r)

		File.WriteAccessLog(r.Method+"\t"+r.RequestURI+"\t"+name+"\t"+time.Since(start).String())
	})
}

// 书写路由信息
func NewRouter() *mux.Router {
	router := mux.NewRouter().StrictSlash(true)
	for _, route := range routes {
		var handler http.Handler
		handler = route.HandlerFunc
		handler = Logger(handler, route.Name)
		router.
			Methods(route.Method).
			Path(route.Pattern).
			Name(route.Name).
			Handler(handler)
	}

	return router
}

var routes = Routes{
	// 获取指定日期的监控信息
	Route{
		"GetInfo",
		"GET",
		"/monitor/info",
		ReturnMonitorInfo,
	},

	// 收集监控数据并保存处理
	Route{
		"MonitorCollect",
		"POST",
		"/monitor/collect",
		PostMonitorInfo,
	},
}

// 具体接口触发函数
func PostMonitorInfo(w http.ResponseWriter, r *http.Request) {

}

func ReturnMonitorInfo(w http.ResponseWriter, r *http.Request) {

}

// 启动api接口，port参数为暴露端口
func StartApi(port string) {
	router := NewRouter()
	File.WriteErrorLog(http.ListenAndServe(":"+port, router).Error())
	ListenSig <- 0
}
```



# go test

## 单元测试

`go test -v ./...` 读取目录下所有`*_test.go`文件，运行里面的test任务

每个包，都是从`*Test.M`出发

```go
package api

func TestMain(m *testing.M) {
	fmt.Println("Start test Api")

	// 开始测试
	m.Run()

	// 测试结束
	fmt.Println("Finish test Api ")
}

func TestPostProjects(t *testing.T) {
	convey.Convey("测试", t, func() {
		// start api and test
		convey.So(get_body, convey.ShouldEqual, right_body)
	})
}
```

## Mock方法

* `sqlMock`
* `goMock`
* `gomonkey`
  - `gomonkey.ApplyFunc`： mock某个方法
  - `gomonkey.ApplyMethod`： mock某个类的方法
  - `xxx.Reset()`： 重置各个方法

```go
package api

// 定义各个测试数据类型
var (
	// 作为测试中传入的config值，这些信息会被mock掉
	Api_test_cfg config.OctopusServerConfig

	// 作为测试中使用的project表信息
	Api_test_projects []*mysql.Project
)

func TestMain(m *testing.M) {
	// 启动api准备测试
	patches := mockMysql()
	fmt.Println("Start test Api")

	// 开始测试
	m.Run()

	// 测试结束
	for _, p := range patches {
		p.Reset()
	}
	fmt.Println("Finish test Api ")
}

// mock mysqlservice的相关操作
func mockMysql() map[string]*gomonkey.Patches {
	var (
		// 作为数据库mock使用的对象
		test_sql *mysql.Mysql
		patches  = make(map[string]*gomonkey.Patches)
	)

	// mock mysql func
	patches["GetMysqlInstance"] = gomonkey.ApplyFunc(mysql.GetMysqlInstance,
		func(_ *config.DataSourceDetail) (*mysql.Mysql, error) {
			var dbConn *gorm.DB
			mysqlInstance := &mysql.Mysql{DbConn: dbConn}
			return mysqlInstance, nil
		})

  // mock mysql method
	patches["GetProjects"] = gomonkey.ApplyMethod(reflect.TypeOf(test_sql), "GetProjects",
		func(_ *mysql.Mysql) (interface{}, error) {
			return Api_test_projects, nil
		})
	return patches
}

```

## httptest

测试api接口的单元测试方法

```go
package api


// 定义各个测试数据类型
var (
	// 作为测试中传入的config值，这些信息会被mock掉
	Api_test_cfg config.OctopusServerConfig

	// api接口的使用对象
	Apptest *iris.Application
)

func TestMain(m *testing.M) {
	// 启动api准备测试
	Apptest = CreateApi(&Api_test_cfg)
	fmt.Println("Start test Api")

	// 开始测试
	m.Run()

	// 测试结束
	fmt.Println("Finish test Api ")
}

func TestPostProjects(t *testing.T) {
	convey.Convey("测试 post /projects", t, func() {
		// start api and test
		e := httptest.New(t, Apptest)
		getAccess := e.POST("/projects").WithHeader("traceid", "xxxx").WithJSON("body").
			WithBasicAuth(Auth.Username, Auth.Password).
			Expect().Status(httptest.StatusOK)

		getAccess.Body().Equal("")
	})
}
```

## Faker

随机生成相关参数，可以通过`print dir(fake)`，下面是相关列表

```
address : 962 Brown Mountain
East Mary, CT 04738
am_pm : PM
ascii_company_email : dawnhines@cantrell.com
ascii_email : richardsanna@yahoo.com
ascii_free_email : ejohnson@yahoo.com
ascii_safe_email : steven92@example.org
bank_country : GB
bban : XRKR3466353844591
boolean : False
bothify : 16 zD
bs : harness cross-media web-readiness
building_number : 2202
catch_phrase : Quality-focused solution-oriented hierarchy
century : XII
chrome : Mozilla/5.0 (Macintosh; Intel Mac OS X 10_5_0) AppleWebKit/5352 (KHTML, like Gecko) Chrome/40.0.802.0 Safari/5352
city : New Lisa
city_prefix : Lake
city_suffix : town
color_name : Purple
company : Gray and Sons
company_email : martinallen@johnson.net
company_suffix : and Sons
coordinate : -87.141470
country : Northern Mariana Islands
country_code : SO
credit_card_expire : 12/27
credit_card_full : VISA 19 digit
Adam Murphy
4436102807805944284 02/22
CVC: 607

credit_card_number : 3508735520995723
credit_card_provider : American Express
credit_card_security_code : 129
cryptocurrency : ('BC', 'BlackCoin')
cryptocurrency_code : DRC
cryptocurrency_name : EOS.IO
currency : ('RWF', 'Rwandan franc')
currency_code : CHF
currency_name : São Tomé and Príncipe dobra
date : 2017-03-08
date_between : 2004-09-09
date_between_dates : 2019-01-02
date_object : 2016-06-06
date_of_birth : 1989-05-07
date_this_century : 2016-07-27
date_this_decade : 2015-06-18
date_this_month : 2019-01-01
date_this_year : 2019-01-01
date_time : 1998-05-01 17:23:46
date_time_ad : 0381-09-12 09:49:54
date_time_between : 2004-01-05 11:45:01
date_time_between_dates : 2019-01-02 20:42:43
date_time_this_century : 2008-12-05 18:31:25
date_time_this_decade : 2012-08-18 21:45:49
date_time_this_month : 2019-01-01 22:56:29
date_time_this_year : 2019-01-02 02:34:05
day_of_month : 09
day_of_week : Tuesday
domain_name : mendoza.biz
domain_word : zavala
ean : 4633017886662
ean13 : 1212638242694
ean8 : 00511063
ein : 27-4912026
email : ruizpeter@hotmail.com
file_extension : wav
file_name : close.jpg
file_path : /authority/section.mp4
firefox : Mozilla/5.0 (Windows NT 5.2; mn-MN; rv:1.9.1.20) Gecko/2016-07-12 09:28:12 Firefox/3.6.4
first_name : Brian
first_name_female : Emily
first_name_male : Robert
free_email : toddedward@gmail.com
free_email_domain : yahoo.com
future_date : 2019-01-12
future_datetime : 2019-01-27 11:39:59
get_providers : [<faker.providers.user_agent.Provider object at 0x0000000012F20EF0>, <faker.providers.ssn.en_US.Provider object at 0x0000000012F20E80>, <faker.providers.python.Provider object at 0x0000000012F20E48>, <faker.providers.profile.Provider object at 0x0000000012F20E10>, <faker.providers.phone_number.en_US.Provider object at 0x0000000012F20DD8>, <faker.providers.person.en_US.Provider object at 0x0000000012F20D30>, <faker.providers.misc.Provider object at 0x0000000012F20B00>, <faker.providers.lorem.en_US.Provider object at 0x0000000012F20AC8>, <faker.providers.job.en_US.Provider object at 0x0000000012F20A90>, <faker.providers.isbn.Provider object at 0x0000000012F20A58>, <faker.providers.internet.en_US.Provider object at 0x0000000012F20A20>, <faker.providers.geo.en_US.Provider object at 0x0000000012F209B0>, <faker.providers.file.Provider object at 0x0000000012F20978>, <faker.providers.date_time.en_US.Provider object at 0x0000000012F20940>, <faker.providers.currency.Provider object at 0x0000000012F208D0>, <faker.providers.credit_card.Provider object at 0x0000000012F20898>, <faker.providers.company.en_US.Provider object at 0x0000000012F20860>, <faker.providers.color.en_US.Provider object at 0x0000000012F20828>, <faker.providers.barcode.Provider object at 0x0000000012F207B8>, <faker.providers.bank.en_GB.Provider object at 0x0000000012F20780>, <faker.providers.automotive.en_US.Provider object at 0x0000000012F20518>, <faker.providers.address.en_US.Provider object at 0x0000000012F204A8>]
hex_color : #63efde
hexify : 333f
hostname : srv-88.campbell.com
iban : GB33UOJP7722297851032
image_url : https://placeimg.com/47/982/any
internet_explorer : Mozilla/5.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.1)
ipv4 : 192.53.46.149
ipv4_network_class : a
ipv4_private : 10.11.77.107
ipv4_public : 98.36.178.232
ipv6 : 9171:502a:4187:a38b:cab:2ae5:a008:3537
isbn10 : 1-84636-208-3
isbn13 : 978-1-80668-720-6
iso8601 : 1981-11-27T00:10:05
itin : 980-73-6362
job : Environmental health practitioner
language_code : ha
last_name : Harrison
last_name_female : Dean
last_name_male : Washington
latitude : -71.7117395
latlng : (Decimal('-87.243354'), Decimal('68.383566'))
lexify : ynqd
license_plate : 04XI233
linux_platform_token : X11; Linux x86_64
linux_processor : i686
local_latlng : ('30.5427', '-97.54667', 'Hutto', 'US', 'America/Chicago')
locale : tk_TM
location_on_land : ('18.51957', '73.85535', 'Pune', 'IN', 'Asia/Kolkata')
longitude : -149.969067
mac_address : f7:42:08:e8:c9:a5
mac_platform_token : Macintosh; PPC Mac OS X 10_6_3
mac_processor : U; Intel
md5 : 7e7cb2e1d96aa3ea8af015bd7d21ae3c
military_apo : PSC 3000, Box 1879
military_dpo : Unit 8330 Box 5496
military_ship : USNV
military_state : AA
mime_type : multipart/signed
month : 07
month_name : August
msisdn : 3162686718629
name : Melissa Gonzalez
name_female : Annette Norton
name_male : Justin Martinez
null_boolean : True
numerify : 110
opera : Opera/8.67.(X11; Linux i686; mai-IN) Presto/2.9.166 Version/10.00
paragraph : Less huge partner herself. Major career personal sort western impact.
paragraphs : [u'Tonight else fear doctor. Radio past event garden.', u'Series technology section capital hundred go choice.', u'Large month resource concern Mr form machine. Simple site however watch.']
password : _NJnqyB+o8
past_date : 2018-12-03
past_datetime : 2018-12-29 23:03:20
phone_number : 001-810-068-3558x30683
postalcode : 63294
postalcode_plus4 : 24817-9240
postcode : 36965
prefix : Miss
prefix_female : Miss
prefix_male : Mr.
profile : {'website': [u'http://knox.com/', u'https://hall-james.com/', u'https://gibbs.com/', u'http://gonzalez-pacheco.biz/'], 'username': u'hensonana', 'name': u'Timothy Strickland', 'blood_group': 'A+', 'residence': u'50459 April Dale\nNorth Jasmine, IL 92057', 'company': u'Hays, Mcdonald and Costa', 'address': u'9407 Donna Summit\nAnnetteville, WY 18513', 'birthdate': datetime.date(1980, 12, 19), 'sex': 'M', 'job': 'Engineer, site', 'ssn': u'170-61-7134', 'current_location': (Decimal('-42.5555445'), Decimal('42.848347')), 'mail': u'janetwalsh@gmail.com'}
pybool : False
pydecimal : -88679.375
pydict : {u'try': u'arthurday@hotmail.com', u'attorney': u'https://davis-shepherd.org/', u'beyond': u'AHMeTAiybSObDygFLCdP', u'make': Decimal('8.9'), u'sign': u'ugHKkPrnstWEoDyEWvmx', u'should': 9959, u'project': Decimal('-924424356.274'), u'born': u'kenHkltRzSJhjBZmvRaT', u'oil': u'xCFyZuRlAKZrvYkpSqxg', u'each': Decimal('61.99'), u'arrive': u'dHyljteKscrRHmswyaZU', u'minute': u'uZRBiOfKafHJcaaKXZXM'}
pyfloat : -7651023230.8
pyint : 3534
pyiterable : [u'VHLKqNIghXURLCTtpzRn', Decimal('6012.9444191'), -35834818801.4104, 6171, Decimal('-12265352.2901'), 3595, u'https://www.salinas.biz/category.php', u'tmullen@harris.biz', datetime.datetime(2014, 6, 24, 0, 55, 42), -93.696436254134, u'NHntgKLcrlNkmEzlgNAR']
pylist : [u'jose07@yahoo.com', u'BgAbucmgaDtzDNkWHZxA', u'BGHXEbOTzWesohqDvSvd', datetime.datetime(1996, 7, 15, 11, 51, 9), u'rWVgMmfqZPaHMvIKXqbR', u'zachary98@yahoo.com', u'aPYhQZYdfMWLbegYpTrX', u'LBlIvbwnXKmfLscVFmyu', u'http://www.brown.com/homepage/', 2009, u'terri30@stafford-barnett.biz']
pyset : set([u'vtURKBQQRApIoUZROrll', 1105, u'jalvarado@gmail.com', u'QmqPChxAQibTqClxJhuD', datetime.datetime(2002, 8, 15, 23, 1, 56), Decimal('6.968001051'), 3837655878192.6, u'benjaminholloway@clark.com', -6306.918624, 3190, Decimal('-25374593628.5'), u'lgnXmYBtXNzCKpVvCYrb', Decimal('-33897542.763')])
pystr : LqxqCNVABxSlGFNkvrlx
pystruct : ([u'jjkLtZqEljABLAkzTfUt', datetime.datetime(2008, 10, 8, 1, 50, 36), u'ZjEpHkuuMBAZkaBhQyrU', u'OilUAUmcThgkcxMbqSAh', datetime.datetime(1975, 7, 5, 0, 36, 55), u'fOJrQchzyPhFxqMdGMnE', 9250, 8949, u'sleWkpsNBMihVkGpRoot', Decimal('807164941.401')], {u'begin': u'BjbPTjfUvVfKcGiJMNQD', u'necessary': u'FzOHQMomtEMTgadZmixE', u'show': Decimal('53203338.7976'), u'country': u'swfMuAlLcqRLksRZwlyU', u'understand': u'lisalewis@mckay.org', u'rock': 9670.5514, u'close': 5565, u'east': 102813994221205.0, u'my': u'yPyrXsPeXixbldFbWhFU', u'those': u'efry@hotmail.com'}, {u'product': {4: u'patrickparker@hotmail.com', 5: [u'qCHqzUfasGcUMKjUhkLc', u'FxwucZAfcnYxrhqOOtXR', -395.803], 6: {4: u'aLqAwymVaNpmhyHhQOLH', 5: u'ErfioxhgoWNVCdnaVanq', 6: [u'RlnaSIsbWnKfaPViBLNj', u'jbTTgCCjCEtuzGodHAdV']}}, u'could': {0: u'RgwNMAMzXKlHhazdfhCX', 1: [u'EAUsPCgHgoQDJcblUCja', datetime.datetime(2012, 1, 19, 13, 35, 8), u'http://walters.org/explore/wp-content/app/author.htm'], 2: {0: u'lwPTuQsvNnCUPBBTffno', 1: u'eXQMtXfRtlBJfpVFAUjg', 2: [u'ECOKoifYhSzLoifZdWnl', u'https://johnson.net/category.html']}}, u'term': {8: datetime.datetime(2010, 1, 9, 12, 24, 49), 9: [datetime.datetime(2005, 11, 13, 14, 10, 18), 6119, u'rzsUSoLRotLlQSjzaskb'], 10: {8: u'belinda84@hotmail.com', 9: datetime.datetime(1997, 3, 10, 4, 46, 15), 10: [Decimal('663.0'), Decimal('6.83132854674E+13')]}}, u'environment': {1: u'qbWPXDBMWzHZELWxQBuz', 2: [6074, 3400, u'eHYTsFimBxNJryhThDNc'], 3: {1: datetime.datetime(1984, 4, 17, 4, 38, 31), 2: u'VhigpvUUAfCHutXcwfbX', 3: [u'UAjJhtniSwdhayTOLWEq', u'NrYZWiQYFJZMYmxiRLww']}}, u'rich': {9: u'UxqyzUuWWYhlsbfWtBPa', 10: [u'VvsHEFsWBmxlVzeCKMIX', 4015, datetime.datetime(1987, 5, 8, 6, 28, 31)], 11: {9: datetime.datetime(1994, 12, 29, 22, 3, 29), 10: u'IeVrPuRMbVQPQLpnXodo', 11: [u'YKUAepdkQNykJLtiEmeH', Decimal('86832605439.3')]}}, u'into': {8: [u'WFkKXgCihEjoMxPMpKiB', -371.213906693373, u'dbrowning@yahoo.com'], 9: {8: u'melissa96@hotmail.com', 9: [u'adamstammy@hotmail.com', 4613], 7: Decimal('-870.723488439')}, 7: u'https://anderson.com/'}, u'skill': {5: u'ZcQKPbNrKTKerNTBcOUu', 6: [1294, Decimal('532156.674'), 247], 7: {5: u'ZDVnfVsiIkWhZRhwhElc', 6: 3777, 7: [u'basssusan@barnett.com', u'gTRgZifCkCkKVgAoSuqm']}}, u'democratic': {2: u'https://gonzalez-dean.com/', 3: [u'millerrachel@schneider.com', u'FkpbQxHdbGinsxifzjLo', u'IPhyirwsceVwTKNpduNW'], 4: {2: u'AOOVvMWWxDyWBFYqZPZq', 3: u'PHdenfFfHQzsHrqmFFNJ', 4: [datetime.datetime(2006, 12, 10, 10, 21, 14), 7330]}}, u'themselves': {8: {8: [u'william87@yahoo.com', u'YRoBtIsJwgGXcuvpIJPj'], 6: 1245, 7: u'ygriffin@smith-rowe.com'}, 6: u'jefferyhall@yahoo.com', 7: [u'qherrera@hunt.info', u'andrewharvey@gmail.com', u'http://brown.info/list/wp-content/author/']}, u'today': {3: u'btSTHtwKXrLHvLpFKOVZ', 4: [u'https://www.ramirez.com/faq.php', u'dpsAElcgRCbovSDyVvdd', u'https://mcintosh-swanson.com/main/search/tag/terms/'], 5: {3: 9055, 4: datetime.datetime(1971, 7, 28, 12, 33, 9), 5: [u'danielhodges@fisher.com', u'EfuYhbcQKCQcptDTlmhM']}}})
random_choices : ['c']
random_digit : 6
random_digit_not_null : 8
random_digit_not_null_or_empty : 6
random_digit_or_empty : 
random_element : b
random_elements : ['a']
random_int : 7153
random_letter : v
random_letters : ['u', 'e', 'a', 'y', 'E', 'A', 'm', 'R', 'k', 'm', 'G', 'o', 'q', 's', 'l', 'O']
random_lowercase_letter : c
random_number : 60
random_sample : ['b', 'a', 'c']
random_uppercase_letter : E
randomize_nb_elements : 13
rgb_color : 207,33,21
rgb_css_color : rgb(141,14,63)
safari : Mozilla/5.0 (Windows; U; Windows NT 6.0) AppleWebKit/535.12.3 (KHTML, like Gecko) Version/5.0 Safari/535.12.3
safe_color_name : olive
safe_email : martinkayla@example.org
safe_hex_color : #bb2200
secondary_address : Suite 437
seed : None
seed_instance : <faker.generator.Generator object at 0x000000001280D7F0>
sentence : Couple race sit sing TV.
sentences : [u'Surface seem serve five each magazine.', u'Statement current line share force prevent company.', u'Daughter listen fear miss.']
```



# 数据存储

## Redis 操作

```go
type RedisStore struct {
	sync.RWMutex
	expiration    time.Duration
	redisInstance *redis.Client
}

func NewRedisStore(expiration time.Duration, redisInstance *redis.Client) (r *RedisStore) {
	r = &RedisStore{
		expiration:    expiration,
		redisInstance: redisInstance,
	}
	return
}

func (this *RedisStore) Set(id string, value string) {
	this.Lock()
	defer this.Unlock()
	this.redisInstance.Set(context.Background(), id, value, this.expiration)
}

func (this *RedisStore) Verify(id, answer string, clear bool) bool {
	return this.Get(id, clear) == answer
}

func (this *RedisStore) Get(id string, clear bool) (value string) {
	this.RLock()
	defer this.RUnlock()
	var err error
	value, err = this.redisInstance.Get(context.Background(), id).Result()
	if err != nil {
		logger.Log.Errorf("从redis中获取 key `%s` 失败, %s", id, err)
		return
	}
	if clear {
		this.redisInstance.Del(context.Background(), id)
	}
	return
}
```

# Captcha 验证码

```go
package api

import (
	"github.com/kataras/iris/v12"
	"github.com/mojocn/base64Captcha"
	"image/color"
	"math/rand"
	"time"
)

var (
	fonts        = []string{"DENNEthree-dee.ttf", "RitaSmith.ttf", "actionj.ttf", "chromohv.ttf", "wqy-microhei.ttc"}
	defaultDigit = [...]interface{}{
		base64Captcha.NewDriverDigit(80, 200, 4, 0.7, 80),
		base64Captcha.NewDriverString(80, 200, 0, base64Captcha.OptionShowHollowLine,
			4, base64Captcha.TxtAlphabet, &color.RGBA{0, 0, 0, 0}, fonts),
		base64Captcha.NewDriverMath(80, 200, 0, 0,
			&color.RGBA{0, 0, 0, 0}, fonts),
	}
)

func GetCaptcha() (string, string){
  // 创建redis存储
	store := NewRedisStore(5*time.Minute, redis.GetRedisInstance())
	driver := defaultDigit[rand.Intn(len(defaultDigit))].(base64Captcha.Driver)
	captcha := base64Captcha.NewCaptcha(driver, store)
	uuid, b64s, err := captcha.Generate()
	if err != nil {
		CaptchaErr(ctx, err, response)
		return "", ""
	}
	return uuid, b64s
}

func validateCaptcha(key, value string) (validated bool) {
	if key == "" || value == "" {
		return
	}
  // 读取redis存储
	store := NewRedisStore(5*time.Minute, redis.GetRedisInstance())
	validated = store.Verify(key, value, true)
	return
}

```

