> java太麻烦了，java内存不初始化就不能用。

C++可以在一块儿内存里面进行操作。比如大小为4个字节的空间，可以解释为有一个int属性的对象A，可以解释为有两个short属性的对象B。也就是说可以进行下面的操作：

```c++
A table[2];
B b = &table[0];
b.first = 1;
b.second = 2;
```

但是JAVA是不能这样操作的。。。

JAVA的枚举类型，是一个类，不再是 C++里面的数字了。

在block函数中，有下面的语句：

```
  mk = (mask *)&table[tx];
  mk->address = cx;
```

这是利用了第一次调用时，table[0]的address字段是无用的，可以用于暂存code数组的下标。而每次遇到procedure 调用block时，table[0]的address字段，就是process声明的address字段，所以也是没用的，也可以用于存放此处code数组的下标。

下面写一下在实现代码过程中具体的不同。

```c++
void enter(int kind)
{
    mask *mk;
    tx++;
    table[tx].name = id;
    table[tx].kind = kind;
    switch (kind)
    {
    case ID_CONSTANT:
        table[tx].value = num;
        break;
    case ID_VARIABLE:
        mk = (mask *)&table[tx];
        mk->level = level;
        mk->address = dx++;
        break;
    case ID_PROCEDURE:
        mk = (mask *)&table[tx];
        mk->level = level;
        break;
    default:
        break;
    }
}
```



```java
private void enter(idtype kind)
    {

        tx++;
        switch (kind)
        {
            case ID_CONSTANT:
                table[tx] = new mask(lexer.id, kind, lexer.num);
                break;
            case ID_VARIABLE:
                table[tx] = new mask(lexer.id, kind, level, dx++);
                break;
            case ID_PROCEDURE:
                table[tx] = new mask(lexer.id, kind, level, 0);
                break;
            default:
                break;
        }

    }
```

还有一个麻烦的事情就是java读取文件的时候，BufferedReader的readLine方法会自动把行后面的换行符给处理掉，这是需要注意的地方。