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
```
