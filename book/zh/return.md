返回值相关
========

1、`RETURN_NULL()`

描述：返回一个null值

参数：无

返回值：无

宏展开：
```c
/*rokety 2014-02-24*/
{
    {
        Z_TYPE_P(return_value) = IS_NULL;
    };
    return;
}
```

2、`RETURN_BOOL(b)`

描述：返回一个布尔值

参数：整型

返回值：无

宏展开：
```c
/*rokety 2014-02-24*/
{
    do {
        zval *__z = (return_value);
        Z_LVAL_P(__z) = ((b) != 0);
        Z_TYPE_P(__z) = IS_BOOL;
    } while (0);
    return;
}
```

3、`RETURN_TRUE`

描述：返回一个布尔值真

参数：无

返回值：无

宏展开：
```c
/*rokety 2014-02-24*/
{
    do {
        zval *__z = (return_value);
        Z_LVAL_P(__z) = ((1) != 0);
        Z_TYPE_P(__z) = IS_BOOL;
    } while (0);
    return;
}
```

4、`RETURN_FALSE`

描述：返回一个布尔值假

参数：无

返回值：无

宏展开：
```c
/*rokety 2014-02-24*/
{
    do {
        zval *__z = (return_value);
        Z_LVAL_P(__z) = ((0) != 0);
        Z_TYPE_P(__z) = IS_BOOL;
    } while (0);
    return;
}
```

5、`RETURN_LONG(l)`

描述：返回一个长整型

参数：长整型

返回值：无

宏展开：
```c
/*rokety 2014-02-24*/
{
    {
        zval *__z = (return_value);
        Z_LVAL_P(__z) = l;
        Z_TYPE_P(__z) = IS_LONG;
    };
    return; 
}
```

6、`RETURN_DOUBLE(d)`

描述：返回一个双精度浮点数

参数：双精度浮点数

返回值：无

宏展开：
```c
/*rokety 2014-02-24*/
{
    {
        zval *__z = (return_value);
        Z_DVAL_P(__z) = d;
        Z_TYPE_P(__z) = IS_DOUBLE;
    };
    return;
}
```

7、`RETURN_STRING(str, duplicate)`

描述：返回一个字符串，duplicate 表示这个字符是否使用 estrdup() 进行复制

参数：字符串，是否需要复制0或1

返回值：无

宏展开：
```c
/*rokety 2014-02-24*/
{
    do {
        const char *__s=(str);
        zval *__z = (return_value);
        Z_STRLEN_P(__z) = strlen(__s);
        Z_STRVAL_P(__z) = (duplicate?estrndup(__s, Z_STRLEN_P(__z)):(char*)__s);
        Z_TYPE_P(__z) = IS_STRING;
    } while (0);
    return;
}
```

8、`RETURN_STRINGL(str, len, duplicate)`

描述：返回一个定长的字符串，其余跟 RETURN_STRING 相同

参数：字符串，长度，是否需要复制0或1

返回值：无

宏展开：
```c
/*rokety 2014-02-24*/
{
    do {
        const char *__s=(str); int __l=len;
        zval *__z = (return_value);
        Z_STRLEN_P(__z) = __l;
        Z_STRVAL_P(__z) = (duplicate?estrndup(__s, __l):(char*)__s);
        Z_TYPE_P(__z) = IS_STRING;
    } while (0);
    return;
}
```

9、`RETURN_EMPTY_STRING()`

描述：返回一个空字符串

参数：无

返回值：无

宏展开：
```c
/*rokety 2014-02-24*/
{
    do {
        zval *__z = (return_value);
        Z_STRLEN_P(__z) = 0;
        Z_STRVAL_P(__z) = STR_EMPTY_ALLOC();
        Z_TYPE_P(__z) = IS_STRING;
    } while (0);
    return;
}
```

10、`RETURN_RESOURCE(l)`

描述：返回一个资源

参数：长整型（资源标识）

返回值：无

宏展开：
```c
/*rokety 2014-02-24*/
{
    do {
        zval *__z = (return_value);
        Z_LVAL_P(__z) = l;
        Z_TYPE_P(__z) = IS_RESOURCE;
    } while (0);
    return;
}
```

11、`RETURN_ZVAL(zv, copy, dtor)`

描述：返回一个zval

参数：zval，是否复制，是否释放zv所占内存

返回值：

宏展开：
```c
/*rokety 2014-02-24*/
{
    {
        zend_uchar is_ref = Z_ISREF_P(return_value);
        zend_uint refcount = Z_REFCOUNT_P(return_value);
        ZVAL_COPY_VALUE(return_value, zv);
        if (copy) {
            zval_copy_ctor(return_value);
        }
        if (dtor) {
            if (!copy) {
                ZVAL_NULL(zv);
            }
            zval_ptr_dtor(&zv);
        }
        Z_SET_ISREF_TO_P(return_value, is_ref);
        Z_SET_REFCOUNT_P(return_value, refcount);
    };
    return;
}
```