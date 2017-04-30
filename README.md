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


### 4 Setting up Events
Instruments (collects raw data based on event)
- idle
- memory
- stage
- statement
- transaction
- wait

Consumers(destination of the data collected)
- Current
- History
- History Long
- Summary
- Summary Digests

### 5 Demo: Setting up Events
```
select * from performance_schema.setup_instruments;
```

```
select * from performance_schema.setup_consumers;
```

#### 01:02
```
select * from performance_schema.setup_instruments where name like 'statement/%';
```
```
update performance_schema.setup_instruments set enabled='yes', timed='yes' where name like 'statement/%';
```

### 7 Demo: Steps to Diagnose Problems
```
select current_user(), connection_id();
show full_processlist;
```

```
select thread_id,process_id from performance_schema.threads where process_id = connection_id();
```

#### 03:16
```
select * from performance_schema.events_waits_current;
```

#### 06:54
```
select event_name，（timer_end-timer_start)/1000000000 as 'duration(ms)', object_name, object_type,index_namemoperation from performane_schema.events_waits_current e inner join performance_schema.threads t on e.thread_id=t.thread_id where processlist_id=connection_id();
```
