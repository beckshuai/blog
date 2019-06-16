---
title: 【C#】小心使用Try,Catch,finally
tags: .NET
---
## 1.使用try{}finally{}

在有返回值的函数中使用try{}finall{}，未使用catch捕获异常，程序将崩溃:
<!--more-->
{% codeblock lang:c# %}
public static int GetNumber()
{
    int i=0;
    try
    {
        int j =0;
        i =5/j;//引发异常
    }
    finally
    {
        Console.WriteLine("输出来自方法GetNumber的finally块");
        //return -2;//finally中不能含有return语句
    }
    return i;
}

public static void Main()
{
    int i =GetNumber();//发生异常并且未捕获,未能获得返回值
    Console.WriteLine("number:{0}",i);
    Console.ReadKey();
}
{% endcodeblock %}
### Output:

![output](http://pic002.cnblogs.com/images/2011/40936/2011090210304789.jpg)


在Main方法中添加异常处理，使得程序发生异常时不崩溃：
{% codeblock lang:c# %}}
public static void Main()
{
    try
    {
        int i =GetNumber();//发生异常并且未捕获,未能获得返回值
        Console.WriteLine("number:{0}",i);
    }
    catch
    {
        Console.WriteLine("输出来自main的catch块");
    }
    finally
    {
        Console.ReadKey();
    }
}
{% endcodeblock %}
### Output：
![output](http://pic002.cnblogs.com/images/2011/40936/2011090210315039.jpg)

## 2.使用try{}catch{}finally{}

在方法GetNumber中添加catch块捕获异常：
{% codeblock lang:c# %}}
public static int GetNumber()
{
    int i=0;
    try
    {
        int j =0;
        i =5/j;//引发异常
    }
    catch 
    {
        i=-1;
    }
    finally
    {
	    Console.WriteLine("输出来自方法GetNumber的finally块");
        //return -2;//finally中不能含有return语句
    }
    return i;
}	
{% endcodeblock %}}
### Output:
![output](http://pic002.cnblogs.com/images/2011/40936/2011090210331187.jpg)

## 总结

  在含有调用含有返回值的方法时，如果必须保证得到返回值且在被调用函数中使用了try块，则必须同时在被调用函数中使用catch块。