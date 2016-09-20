#PHP总结
##Author: Ryan yang
***
####1.数组赋值技巧
	foreach ($input as $key => $value) {
		$output[] = $key.$value
	}
	$output[]这样写会每次自动加1
	
####2.PHP有一种错误抑制运算符@
当@放置在一个PHP表达式之前，该表达式可能产生的任何错误都会被忽略，利于可以用于XML解析上。$xml = @simplexml_load_string($input)

####3.PHP一边的调试命令
	a. die();  注意php后面必须要有分号
	b. var_dump("asdfasd") 或者 var_dump(debug_backtrace());
	c. file_put_contents("test.txt","this is something or $val"), 如果想把array存入到文件中需要使用print_r($result, true)格式

####4.执行php程序
不通过浏览器访问的方式，可以使用`php -f`命令来执行php文件

####5.类初始化问题
	a. 如果类实现时有function __construct()函数则在new操作时会被调用。
	b. 如果没有__construct()函数而有同名函数，则会调用同名函数。
	c. 若之前的两个都没有，则进行默认的初始化。
	
####6.file_put_contents在文件后面追加
file_put_contents(filename, value, `FILE_APPEND`);

####7.php打印调试
die(`print_r(array,true)`)

####8.php对多维数组按照某一维度排序
	1. 首先取出多维数组中的某一列，存到sort_column[]中
	2. 使用array_multisort(sort_column, SORT_ASC, array)进行排序
