NEW STAFF
=============
### In-Memory OLTP
In-Memory OLTP (In-Memory Optimization) can improve the performance of transaction processing, data load.

It use No locks, no internal latches.

Engine makes new version of rows and timestamp of rows in shared buffer, analyzes and validate before committing. 

### Temporal Tables 
Temporal table is type table that provides correct information about stored facts at any point in time.

Two table inside - current data and historial data. current data change then store in the historial table

BASIC 
=============
### SQL SERVER Service? 
-  It reveive the queries and **execute** them, send **response** back to calling applications.
-  Manage the **data files**

### SQL SERVER Agent?
- Automation service that manages the scheduled execution **task**
- **Notify** the administrators of problem occur

### SQL SERVER Profiler?
Interface to create and manage **traces** and analyze and replay trace result

### SQL SERVER Analyzer, Tuning Advisor?
Automatic database tuning **insight into query performance** problem, recommend solution and automatically fix problem 

### SQL SERVER Execution Plans
- Shows how SQL SERVER compile/executes the querys
- Read the executuation plan

### Instance
complete DB server, one computer can have mant instance, but only one default

### Result set
set of record not only data also metadata like col name, datatype and sizes, **saved in ram**

### Object
Table, schema, db use to store or reference data

### Schema
collection of objects have db schema and table schema

### SQL SERVER SYSTEM DB
    - master (installation, disk space, file location, configuration)
    - model (use as model to create new database)
    - tmpdb (workspace, tmp table, recreate everytime server restarted) **size is important for performance**
    - mssql (resource hidden, store procedure, functions)
    - msdb (agent use, maintance, backup, message)

### Statements
    DDL -> 增删改查数据object table/db
    DCL -> who can see and modify data 权限
    DML -> 增删改查表数据

### Batch
    where we process statement
    directive instructs SERVER to execute and parse sql
    
    GO -> not statement, send current batch
    EXEC -> execute function procedure

### Identifiers, variables
    标识符 128 character 
    start with character
    [ 有括号，关键字]

    @ local variable and parameter
    @@ global variable (system use, system defined function)
    @@error / @@rowcount
    # temporary table / procedure
    ## global temporary table or other object
### data type
    * int tiny 1  small 2 big 8 
    * char varchar nvarchar
    * bit 
    * text (no union)
    * float real
    * decimal numberic (5,2) 123.45
    * date small date(min) date time(3.33ms) date time 2
    * money
    * time stamp 
    * SQL_VarIant 不定类型
### flow-control, statement block
    ```sql
    if ()begin end 
    else begin end

    while () begin end

    goto name1
    name1:
        select ....
    
    select a, b = 
        case c
            when '' then ''
            when '' then '' 
        end
    from ...
    ```
Queries
=============
### JOIN
    querying from more than one table and views
    inner join 0 - smaller(a,b)
    outter join - left right full - bigger(a,b) - a+b
    cross join Cartesian 卡提振
### from + where

### group by + having

### order by + top

### select [col...] into [new_table] from [table]
    only copy data, no constraint, index depend on table
### insert [into] tablename (col...) values (...)

### set identity_insert tablename [on | off]

### insert tablename (select ...)

### update <tablename> set <colname = value, ....> where < >
    ```sql
    UPDATE dbo.Table2   
    SET dbo.Table2.ColB = dbo.Table2.ColB + dbo.Table1.ColB  
    FROM dbo.Table2   
        INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA);  
    ```
### delete [from] tablename where condition
    ```sql
    DELETE Production.ProductProductPhoto 
    FROM Production.Example_ProductProductPhoto ppp 
    LEFT OUTER JOIN Production.Product p 
    ON ppp.ProductID = p.ProductID 
    WHERE p.ProductID IS NULL 
    ```
### difference truncate , drop and delete
    truncate table tablename

| tuncate    | delete   |
| -------- | -----  |
| DDL      | DML   |
| lock table remove all records |   row lock for remove   |
| NO where | use where |  234  |
| minimal log so quick| maintain log so slower |
| reset identity col| retain the identity |
| no use in index view| can use in index view|

drop
    
    remove table from db
    all rows/ index/ privilege removed
    no DML trigger
    no roll back ???
    DDL

### subquery
    select in select connect with condition <>= exists all any in
### corrlated subq
    inner subquery use values from outer query
### union and union all / except / intercept
    combine data vertically

### JOIN VS SUBQ
    no certain answer
    join for return rows
    sub for return few rows

### Rank, row number, dense rank,
    ```sql
    rank() over (partition by order by) as rank 1134 排名
    row_number() over (...) as rownumber 1234 行数
    dense_number() over (...) as densenumber 11234 排名 紧凑

    ```

### identity col
    a column that db made up by the value generate by db
    create..
    (
        id type identity(start, increment),
    )
### Constraints
    
    not null

    primary key have values that uniquely identify a row in a table (only one, no null, unique) 

    composite primary key is table constraint (two or more)

    unique (one null, can be fk) 

    candidate key not pk but can be (unique and no null)

    foreign key is col or combination of col used to establish link between two table
        delete key which fk point to - on delete
            * no action - error
            * cascade - delete reference fk row
            * set null - set to null fk row
            * set default - set to default fk row

    check limit value by accept by col logic expression
    constraint ck check (col < ....)
### Integrities 
    * Domain
        - all col in db must declare upon a defined domain
    * Entity
        - pk constraint unique and not null
    * Referential
        - FK constraint change can not make where fk point to

### normalization 
    reduce redundacy ensure consistency

    save place/ avoid restructing/ increase flexibility 
    - Fist normal Form
        no same col , no repeating value, atomic value 
        无重复
    - Second 
        non-prime attribute is dependent on any proper subset of any candidate key
        非码列 完全依赖 键列
    - third 
        all the attributes in a table are determined only by the candidate keys of that relation and not by any non-prime attributes
        非码列 之间不能有完全依赖

### Exception handling

    ```sql
        begin try
        end try
        begin catch
        end catch
    ```


VIEW and TABLE
=============
### SQL SERVER CTE 
    common table expression (常用表表示) 
    Temporary named result set
    Defined within the execution scope of single DML statement
    Create View as part of SELECT, Functions, Store Procedure, Triggers

- Syntax
    ```sql
    With <name> (colname.....)
    AS
    (
        Select from group by...
        No order by unless with top 
        NO INTO
    ), <name>
    AS
    (
        anchor memeber
        union all / union / Intersect / except
        anchor member
        union all
        recursive member (no distinct, group by, having, join, top, subquery)
    )
    Select from <name> ....
    ```

- Example
    
    选出一个人以及他的所有下级
    ```sql
    WITH tmp 
    AS (
        select empid, mgid
        from employee where empid = 3
        Union all
        select e.empid, e.mgid
        from employee e 
        join tmp t on e.mgid = t.empid
    )
    select * 
    from tmp
    ```
- Usage
    * Used for recursive query 
    * Replace VIEW when use of view is not required, don't need to store metadata
    * Readability, easy to read and maintance

- Why use
    * Improve readability and manageability of complex querys
- Why not use

### View 

    Virtual table and content is defined by queries
    view of data of one or more tables in the database
        * regular view
        * index view (with clustered index)
        * distributed partitioned view (accross sql server instance)

- Syntax
    ```sql
    Create View <name> (cols...)
    AS (
        select distinct 
        from 
        Join
        NO order by 
        NO into
        NO temporary table or table variable
    )
    ```
- Example
    
- Usage

- Why use
    * Simplify data manipulation, frequently used, no need everytime
    * Hide structure of table, backward compatible interface
    * Customize data for user, no need permission
    * distribute querues.....combine data from different database

- Why not use

### Table/ Temporary Table
    
    Table is collection of data organized in certain way
    Temporary table defined like regular table, just store in tempdb
        **local temporary table #table**
            for current user use, 
            multiple connection can have same name,
            destory after sp finished(in store procedure the sp finished then drop),
            destory at end of session,(当前的不用了)

        **global temporary table ##table**
            anyuser with permission can use
            multiple connections share same temp table, no same name,
            dropped when session that create the table end and all other task have stop referencing (都不用了) 
    
- Syntax
    ```sql
    Create Table <name> / #<temp table>
    (
        id type constraints,
        id ....
        constraint check
    )
    ```

- Example
    
    ```sql

    ```
- Usage

- Why use

- Why not use

### Temporary Variable
    
    data type that can be used within batch, store procedure, function
    NO indexs
    NO FK
    YES pk/unique/check/null/not null 
    
- Syntax
    ```sql
    declare @name table
    (
        id type constraint
    )
    ```
- Example
    ```sql

    ```
- Usage
    * replace temp table, tmp table have performance problem
    * scope last duration of the batch, function, store procedure
- Why use
    * tmp table may cause overhead of tmpdb or store procedure recomplication
    * short lock time
    * less recomplication 
- Why not use
    * performance suffer when result set is too large


### temporary table VS temporary variables


Transaction, SP, function
=============
### Transaction 
    recoverable logic unit of work
        commit - saved
        rollback - undone

    Transaction mode 
        - implicit transactions
            start new transaction after current transaction commit or rollback
        - explicit transactions
            explicitly define both start and end of transaction

    @@trancount - depth of the executed begin trans block

    set savepoint - 
    ```sql
        begin transaction
        save {transaction | tran} savepoint_name
        ....
        if @@error <> 0 
            rollback transaction savepoint_name
        
        commit transaction
    ```
- Syntax
    ```sql
    Begin transation
    commit
    rollback
    ```
- Example
    
    ```sql
    DECLARE @TranName VARCHAR(20);  
    SELECT @TranName = 'MyTransaction';  

    BEGIN TRANSACTION @TranName;  
    USE AdventureWorks2012;  
    DELETE FROM AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  

    COMMIT TRANSACTION @TranName;  
    GO  
    ```
- Usage
    * least one DML 
    * change db state
    * use within one db
        
    * atomicity 原子 - management
        - work is atomic (all done or fail)
    * consistency 一致 - 
        - data is integrity and consist 
        - structure is correct
    * isolation 隔离 - isolation
        - isolated
    * durability 持续 -  loging 
        - change is permanently 
- Why use

- Why not use


### Concurrency
    pessimistic control when lock cost less

    optimistic control when roll back cost less

### isolation level
    * read uncommited - can read while modify (dirty read)
    * read commited - can't read while modify but can change by others (nonrepeatable read)
    * repeatable read - can't read update, but can insert (phantom data)
    * serializable 
    * snapshot
        - maintained in tempdb
        - optimistic

### deadlock
    multiple process simultaneously require locks held by other

    fix : examine deak lock trans and kill the simple one with an error to break dead lock


### reduce deadlock
    * trans access table in same order (守秩序)
    * hold lock when repeatable read are necessary (别人要重复读就等)
    * avoid long run transactions (不要花太长时间)
    * avoid user input while hold lock (占位置之后不要干别的)
    * avoid numerous simultaneous execution of DML (不要同时大量做增删改)

### Store Procedure 
    groups one or more sql statement into logical unit, store as an object
    
    when executed for first time, sql will store most optimal query plan in memory cache for reuse

- Syntax

    ```sql
    create procedure [name] 
    (
        @var1 type,
        @var2 int = value,
        @var3 int OUTPUT
    )
    as 
    if
    begin

    select....
    return(1)
    

    end
    else
    begin
    @var3 = ..
    return(@var2)
    end
    go
    ```

- Example
    
    ```sql

    ```
- Usage
    * call procedure - exec name @var = , @var2 = @read
    * set automatically exec

- Why use
    * faster and reliable performance
    * security for injection 
    * centralize sql code easy debug
    * reduce network traffic
    * code reuseability

- Why not use

### Function
    built-in function
    user-defined functions
        a common language runtime(CLR) routine that accepts parameters perform action
     

- Syntax
    ```sql
    create function name 
    (
        id type
    )
    returns int
    AS
    begin

    end
    GO

    select name(id) 
    ```

- Example
    
    ```sql

    ```
- Usage
    * only used in sql statement, most in select or present data 

- Why use

- Why not use

### Trigger
    specical type store procedure that executed when user issue DML (not select)
    * DML trigger
    * DDL trigger
    * Logon trigger
    * server level
- Syntax
    ```sql
    create trigger tri_name
    on (table|view)
    (for = after | instead of)
    (insert|update|delete)
    as
    {
        sql....
    }
    ```

- Example
    
    DML
    ```sql
    create trigger tri
    on department
    for insert as
    begin
        ...
    end
    ```
    DDL
    ```sql
    CREATE TRIGGER Drop Trigger 
    ON DATABASE
    FOR DROP_TABLE, ALTER_TABLE
    AS
    PRINT ‘Trigger is fired to rollback!’
    ROLLBACK
    ```
- Usage
    * automatically fired on a event
    * cannot explicitly executed 

    insert trigger

    delete trigger
    * effects
        * delete - parent delete all children also delete
        * restrict - child exist can not delete
        * net null - set child to null
    update trigger
    * effects
        * update - change parent change child
        * restrict - child exists can not change parents

- Why use
    * enforce integrity 加强完整性
    * maintain audit record 审计记录
    * implement business rule 实施特殊规则
    * cascading update 级联更新
- Why not use


### OUTPUT
    - for DML 
    - RETURN rows as part of DML
    - access to INSERTED and DELETED system table
    输出来看

```sql
UPDATE Employee 
SET Salary = Salary * 1.10
OUTPUT 
DELETED.* AS [Old Values], 
INSERTED.* AS New Values 

DECLARE @TempTable TABLE (EMPID INT)

INSERT Employee 

OUTPUT INSERTED.EMPID 
INTO @TempTable 

VALUES ('David')

INSERT Employee 

OUTPUT INSERTED.EMPID 
INTO @TempTable 

VALUES ('James')
SELECT * FROM @TempTable
```
### cursor
    object applied on result set to read and load the result row by row
    
- Syntax
    ```sql
    
    ```

- Example
    
    ```sql
    declare <name> cursor for
    select eid from emp where ...

    open <name>
    fetch next from <name> into @var

    while @@fetch_status = 0
    Begin
        insert into table(col) values (@var)

        fetch next from <name> into @var
    End
    ```
- Usage

- Why use
    * used for data migration
- Why not use
    * performance killer

### Index 
    objects allows sql server to find data in a table without scanning entire table
    

- Syntax
    ```sql
    create index ind_name on table (col)
    ```

- Example
    
    ```sql
    CREATE CLUSTERED INDEX i1 
    ON d1.s1.t1 (col1)

    CREATE UNIQUE INDEX i1 
    ON t1 (col1 DESC, col2 ASC, col3 DESC); 

    create nonclustered index
    ```
- Usage
    - clustered index (only one, sorted, point to physical, default primary)
    - non clustered index (249, not sorted, point to clustered, no cluster to physical)

- Why use
    * quick find data in where 
    * matching in join
    * maintain uniqueness in insert update
    * sort group 
    * separate structure to the table


- Why not use




### difference CTE AND VIEW


### difference store procedure and function