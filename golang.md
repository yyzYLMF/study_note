#go语言总结
##Author：Ryan yang
***
####1.Go语言自带编译和测试工具
	a. Go语言对GOPATH要求很严，一定要注意自己的路径是否配置正确；自带的go build 和go test命令虽然很好用但是要将代码按照格式放到$GOPATH/src下面。否则可以使用指明所有文件的方式编译：go main.go lib.go
	b. 正在开发什么项目就把该项目的路径放到$GOPATH里面的第一个，否则很有可能导入别的库，因为go语言import都是相对路径，而根目录都是$GOPATH配的，而且导入的库是从$GOPATH中从前往后找的，如果路径上有相同库便不会看后面的，从而可能导致错误
	
####2.Go语言import规则
	a. go语言中声明的变量就必须使用；没有用到的lib库不要import
	b. 若只想使用摸个package中的init()等，需要在前面加下划线(eg: import _"abc")，但是一旦使用了里面的变量或者函数就不能使用下划线
	
####3.slice分片与map使用make初始化是一样的
	a. 数组 slice:=make([]int, 5)
	b. Map mm:=make(map[int]string)
	
####4.Go语言中调用成员函数方式
go语言没有*'->'*操作符，普通数据结构或引用型或指针型都可以使用点（*'.'*）来使用成员变量或者成员函数

	type Rect struct {
		x,y				float64
		width,height	float64
	}
	func(r *Rect) Area() float64 {
		return r.width*r.height
	}
	......
	rect := Rect{1.0, 1.0, 5.0, 6.0}
	rect2 := &rect
	var prt *Rect = &rect
	***一下调用方式都正确***
	a. rect.Area()
	b. rect2.Area()
	c. ptr.Area()	(*ptr).Area()
	
####5.指针型函数调用方式注意事项
若存在func (log `Type`) Log (str string) {}

	a. 若Type是*Logger，可以使用对象、引用、指针直接调用函数
	b. 若Type是Logger，也可以使用对象、引用、指针直接调用函数
唯一区别：若想在函数中改变对象的值，Type需要使用`指针`，变量类型可以是对象、引用、指针

####6.Go语言中的匿名组合和非匿名组合
非匿名

	type Job struct {
		name string
		log *Logger
	}
	调用时使用 job.log.Logger()
	初始化时还要对log赋值
匿名

	type Job struct {
		name string
		*Logger
	}
	调用时使用job.Logger()
	初始化时不用对*Logger赋值
	
####7.Go语言字符串处理常用函数
	a. string.Join(str []string, sep string) string //将str中的每个string用sep拼接在一起
	b. Contains(str,substr string) bool //判断str是否包含substr，我们自己好像实现了一个StrContains可以忽略大小写
	
####8.Go fmt 打印
	a. %v the value in a default format (它会自己识别类型)
	b. %+v when printing structs
	
####9.Go异常处理
	a. defer用来添加函数结束时执行的语句
	b. panic表示不可恢复的错误，panic后程序会挂掉然后去执行defer
	c. recover捕获panic中的异常，一般在defer中。然而逻辑不会恢复到panic那个点去，函数还是会在defer之后返回
	defer func() {
		if err := recover(); err!=nil {
			......
		}
	}()
	
####10.Go易错点（一）
	a. 每一行代码可以没有分号，但是for还是要用分号（';'）间隔
	b. go语言没有++i,只能用i++
	c. 切片初始化：a := []int{} 后面的大括号必须有,或者 var a []int
	d. interface后面自带有一个{}，所以ret := []int{}只有一个大括号，而ret:= map[string]interface{}{}后面有两个大括号 
	e. interface{}即使存的是数组类型也不能直接遍历，要进行强制类型转换，所以如果公共接口返回值类型要明确，否则别人不知道你返回的是什么，无从强制类型转换，ret := info["list"].([]map[string]string)
	
####11.使用go启动协程没反应
有一种可能是主程序在一个时间片就可以执行完（执行很快），协程来不及执行就结束了。可以使用`sync.WaitGroup`来使主程序等所有协程都执行完再退出。

####12.协程理解
协程可以认为是线程里的一个函数块，开销小，切换快，最大的亮点是协程的调度机制
	
####13.IDEA让go语言具有跳转功能：
	a. file -> setting -> languages & frameworks -> go libraries
	b. 在project libraries中加入项目src上一层目录
	
####14.go语言中defer、return、返回值之间的执行顺序
多个defer执行顺序为`后进先出`，即最后定义的defer最先执行

	a. return最先执行，return负责将结果写入到返回值中
	b. defer执行收尾工作
	c. 最后函数将返回值返回
	
注意：C语言在函数返回时时通过复制值，go语言在未声明返回值时与C语言相同，如果此时defer对返回值操作很可能无效（因为defer之前return已经将值写入到返回值中，而defer操作的并不是返回值而是一个局部变量，`指针`例外）。但是在有名返回值时，相当于返回值在最开始已经存在了，最后也就不用复制了，defer对其操作也会反应到函数外部（推荐使用`有名返回值`）

####15.go中的sort.Sort()函数
要使用Sort函数必须使用定义了Len(),Less(i,j int),Swap(i,j int)三个接口的类型，然后便可以使用sort.Sort(DataSorter(input))函数进行排序

####16.go中的时间格式
go里对time展现格式进行定义时，必须使用2006-01-02 15:04:05时间点，组织形式可以不同，但是相对时间点必须是这个。 eg: fmt.Println(time.Now().Format("2006-01-02 15:04:05"))