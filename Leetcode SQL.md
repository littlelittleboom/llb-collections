[1225. 报告系统状态的连续日期](https://leetcode.cn/problems/report-contiguous-dates/description/?envType=study-plan-v2&envId=sql-premium-50)


``` Mysql
with tmp as (
  select *, 
	row_number() over(partition by status order by d) as row_num
 from (
  select fail_date as d, "failed" as status from Failed
  where (fail_date between date_format("2019-01-01", "%Y-%m-%d") and date_format("2019-12-31", "%Y-%m-%d"))
  union all
  select success_date as d, "succeeded" as status from Succeeded
  where (success_date between date_format("2019-01-01", "%Y-%m-%d") and date_format("2019-12-31", "%Y-%m-%d")
)) b
)

select status as period_state, d as start_date,  max(d) as end_date
from tmp
group by status, date_sub(d, interval row_num day) # 有可能只有一个日期，会被忽略掉，需要加上状态分组
order by d
```
