---
layout:  post
title:   "PHP时间格式转换"
date:    2015-11-21 15:03:00
categories: PHP
---
* content
{:toc}

###基本转换
    
    //默认时区
    date_default_timezone_set('PRC'); 
    //some time
    echo "今天:".date("Y-m-d",time());
    echo "今天:".date("Y-m-d",strtotime("18 june 2008"));
    echo "昨天:".date("Y-m-d",strtotime("-1 day"));
    echo "明天:".date("Y-m-d",strtotime("+1 day"));
    echo "一周后:".date("Y-m-d",strtotime("+1 week"));
    echo "一周零两天四小时两秒后:".date("Y-m-d G:H:s",strtotime("+1 week 2 days 4 hours 2 seconds"));
    echo "下个星期四:",date("Y-m-d",strtotime("next Thursday"));
    echo "上个周一:".date("Y-m-d",strtotime("last Monday"));
    echo "一个月前:".date("Y-m-d",strtotime("last month"));
    echo "一个月后:".date("Y-m-d",strtotime("+1 month"));
    echo "十年后:".date("Y-m-d",strtotime("+10 year"));


###某天 + n天
strtotime可以接受第二个参数，类型timestamp,为指定日期

    //+1
    echo date('Y-m-d', strtotime ("+1 day", strtotime('2011-11-01')));

    echo "今天:".date('Y-m-d H:i:s');

    echo "明天:".date('Y-m-d H:i:s',strtotime('+1 day'));


上一行输出当前时间，下一行输出明天时间

    /**
    *这里+1 day
    *可以修改参数1为任何想需要的数
    *day也可以改成year（年），month（月），hour（小时），minute（分），second（秒）
    */
    echo date('Y-m-d H:i:s',strtotime("+1 day +1 hour +1 minute");

###常用日期函数
下面这些代码是一些常用的日期处理函数了，可以两个时间的日期加减，两日期之差,日期转换时间截等。

    //日期天数相加函数
    echo date('Y-m-d',strtotime('+1 d',strtotime('2009-07-08'))); 
     
    echo date("Y-m-d",'1246982400');

    die();
     
    $d = "2009-07-08 10:19:00";
    //日期天数相加函数
    echo date("Y-m-d",strtotime("$d   +1   day"));   

    //把日期转换成时间堆截
    function dateToTime($d){
        $year=((int)substr("$d",0,4));//取得年份
         
        $month=((int)substr("$d",5,2));//取得月份
         
        $day=((int)substr("$d",8,2));//取得几号
         
        return mktime(0,0,0,$month,$day,$year);
    }


###计算两日期之差

    /*
     *下面函数计算两日期之差
    */
    $Date_1="2009-07-08";
     
    echo $Date_1+1;
     
    $Date_2="2009-06-08";

    $Date_List_a1=explode("-",$Date_1);
     
    $Date_List_a2=explode("-",$Date_2);
     
    $d1=mktime(0,0,0,$Date_List_a1[1],$Date_List_a1[2],$Date_List_a1[0]);
     
    $d2=mktime(0,0,0,$Date_List_a2[1],$Date_List_a2[2],$Date_List_a2[0]);
     
    $Days=round(($d1-$d2)/3600/24);
     
    echo "两日期之前相差有".$Days."天";



