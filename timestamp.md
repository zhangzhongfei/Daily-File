# php实现今天、昨天、上周、本周、本月数据统计

> 关键字 mktime

## mktime()

* 语法 mktime(hour,minute,second,mnth,day,year)

## 代码实现

```
//php获取今日开始时间戳和结束时间戳
$today_start=mktime(0,0,0,date('m'),date('d'),date('Y'));
$today_end=mktime(0,0,0,date('m'),date('d')+1,date('Y'))-1;

//php获取昨日起始时间戳和结束时间戳
$yesterday_start=mktime(0,0,0,date('m'),date('d')-1,date('Y'));
$yesterday_end=mktime(0,0,0,date('m'),date('d'),date('Y'))-1;

//php获取上周起始时间戳和结束时间戳
$lastweek_start=mktime(0,0,0,date('m'),date('d')-date('w')+1-7,date('Y'));
$lastweek_end=mktime(23,59,59,date('m'),date('d')-date('w')+7-7,date('Y'));

//php获取本周周起始时间戳和结束时间戳
$thisweek_start=mktime(0,0,0,date('m'),date('d')-date('w'+1),date('Y'));
$thisweek_end=mktime(23,59,59,date('m'),date('d')-date('w')+7,date('Y'));

//php获取本月起始时间戳和结束时间戳
$thismonth_start=mktime(0,0,0,date('m'),1,date('Y'));
$thismonth_end=mktime(23,59,59,date('m'),date('t'),date('Y'));
```

