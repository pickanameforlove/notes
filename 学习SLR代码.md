**词法分析器**

这个部分实现很有意思，实现了一个词元类，这个类里面有三个map，分别是存一个字符的标点符号或者运算符，存占两个字符的运算符，存关键字。还实现了一个next方法，此方法一次返回一个token。

next方法大致的实现思路就是一个个排除，先看看读取到的一个字符c能不能在第一个map里面搜到，如果搜到则返回，如果搜不到看看它是不是两个字符的运算符。然后判断是否为数字和标识符（这里可能有关键字）

```c++
bool Lexana::next()
{
    while(c == ' ' || c == '\n')
    {
        lines += (c == '\n');
        c = stream.get();
        if(stream.eof())
        {
            iseof = true;
            return false;
        }
    }

    if(m1.find(string(1, c)) != m1.end())
    {
        value = string(1, c);
        kind = m1[value];
        c = stream.get();

        return true;
    }

    if(m2.find(string(1, c)) != m2.end())
    {
        char c1 = stream.get();
        string s;
        s.push_back(c);
        s.push_back(c1);
        if(m2.find(s) != m2.end())
        {
            value = s;
            kind = m2[s];
            c = stream.get();
        }
        else
        {
            value = string(1, c);
            kind = m2[value];
            c = c1;
        }

        return true;
    }

    if(c == ':')
    {
        char c1 = stream.get();
        if(c1 != '=')
        {
            errmsg = "lines " + to_string(lines) + ", lex error: := is a whole";
            c = c1;
            return false;
        }

        value = ":=";
        kind = PL_ASSIGN;
        c = stream.get();
        return true;
    }

    string s;
    bool flag = true;
    while(true)
    {
		//此处是设置一个完成的标志。
        if(c == ' ' ||
           c == '\n' ||
           m1.find(string(1, c)) != m1.end() ||
           m2.find(string(1, c)) != m2.end())
        {
            //lines += (c == '\n');
            value = s;
            if(kw.find(s) != kw.end())
            {
                kind = kw[value];
            }
            else
            {
                if(flag)
                    kind = PL_INT;
                else{
                    if(s.length()>10)
                    {
                        errmsg = "lines " + to_string(lines) + ", lex error: " + s + " length limit exceeed";
                        return false;
                    }else{
                        kind = PL_IDENTIFIER;
                    }
                }
            }

            break;
        }

        if(!is_dig(c) && !is_cha(c))
        {
            errmsg = "lines " + to_string(lines) + ", lex error: " + string(1, c) + " error";
            c = stream.get();
            return false;
        }
        if(is_cha(c)){
            flag = false;
        }
        s.push_back(c);    
        c = stream.get();
    } 

    return true;
}
```

**语法分析器**

