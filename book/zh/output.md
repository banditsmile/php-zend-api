输出相关
=======

1. `php_printf(format, var...)`

描述：标准的 C 语言的 printf 针对 PHP 的版本。与 printf 不同，它不是打印到进程的 STDOUT 通道，而是到当前的[输出流](http://www.php.net/manual/zh/wrappers.php.php)，可被用户进行缓冲。要注意的是，php_printf 不象大部分 PHP 的 API，它不是二进制安全的。想要二进制安全输出应使用 PHPWRITE。

> 二进制安全：[In PHP what does it mean by a function being binary-safe?](http://stackoverflow.com/questions/3264514/in-php-what-does-it-mean-by-a-function-being-binary-safe) 和 [Binary-safe](http://en.wikipedia.org/wiki/Binary-safe)

参数：格式化字符串，变量列表
返回值：输出的字符数

**Note: **通常来说，要直接向输出流打印数据，建议改为返回字符串数据给用户，由用户决定做什么。 此规则的例外情况可能是处理二进制数据（如图像）的函数，API 要求就在函数里提供方法来存取数据。

示例代码：
```c
/*rokety 2014-02-23*/
PHP_FUNCTION(hello_world)
{
    char *name;
    int name_len;

    if (zend_parse_parameters(ZEND_NUM_ARGS() TSRMLS_CC, "s", &name, &name_len) == FAILURE) {
        RETURN_FALSE;
    }

    php_printf("Hello %s!", name);

    RETURN_TRUE;
}
```

2. `PHPWRITE(string, strlen(string))`

描述：PHP二进制安全的输出宏。
参数：字符串变量，字符串长度
返回值：输出的字符数

示例代码：
```c
/*rokety 2014-02-23*/
PHP_FUNCTION(print_str)
{
    char *name;
    int name_len;

    if (zend_parse_parameters(ZEND_NUM_ARGS() TSRMLS_CC, "s", &name, &name_len) == FAILURE) {
        return;
    }

    PHPWRITE(name, name_len);

    RETURN_TRUE;
}
```

3. `snprintf(char * str, length, format, var...)`

描述：snprintf根据参数format来转换并格式化数据，然后将结果复制到参数str所指的字符串数组，必须先为str分配足够的内存。
参数：字符串变量，字符串长度，格式化字符串，变量列表
返回值：格式化后得到的字符串的长度

示例代码：
```c
/*rokety 2014-02-23*/
PHP_FUNCTION(hello_world)
{
    char str[20];
    char *hello = "hello";
    char *world = "world";

    snprintf(str, sizeof(str), "%s %s!", hello, world);

    PHPWRITE(str, strlen(str));

}
```

4. `spprintf(char ** str, max, format, var...)`

描述：spprintf是snprintf的动态版本，它会自动根据需要为str分配内存空间，可以通过设置max参数来限制所分配内存空间的最大值，如果此值为0，那将没有限制。
参数：字符串变量地址，内存分配限制，格式化字符串，变量列表
返回值：格式化后得到的字符串的长度

**Note: **该函数比snprintf稍微慢点，并且如果你忘记释放该函数分配的内存，将可能导致内存泄漏。

示例代码：
```c
/*rokety 2014-02-23*/
PHP_FUNCTION(hello_world)
{
    char *str;
    char *hello = "hello";
    char *world = "world";

    spprintf(&str, 0, "%s %s!", hello, world);

    PHPWRITE(str, strlen(str));
    efree(str);
}
```