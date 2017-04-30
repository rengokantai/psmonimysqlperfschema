# psmonimysqlperfschema
## 3. Getting Started with Performance Schema
### 6 Impact on Server Performance
Impact of performance on server performance
- minimal impact on server performance
- no excessive memory allocation
- continuous and unobstrusive monitoring
- no impact on parser
- priority to server code execution
- backward compatibility with versioning

### 8 Demo:Performance Schema
```
select version();
show variables like 'performance_schema';
select * from information_schema.engines where engine='performance_schema';
show tables from performance_schema;
```
change context
```
use performance_schema;
select * from user_variable_by_thread;
select * from setup_timers;
```
find query with high execution time
```
select digest_text as query, count_star as exec_count, sec_to_time(sum_timer_wait/1000000000000) as exec_time_total,
sec_to_time(max_timer_wait/1000000000000) as exec_time_max, (avg_timer_wait/1000000000) as exec_time_avg_ms,
sum_rows_sent as rows_sent,
sum_rows_examined as rows_scanned
from performance_schema.events_statements_summary_by_digest order by sum_timer_wait desc;
```

## 4. Identifying Resource Bottlenecks by Wait Events
### 2 What is Wait Event?
When MySQL Engine has to wait for any resource or operation to execute the next task, it is called as a Wait Event.
