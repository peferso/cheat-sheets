# Basic introduction

## Regular expressions

Parse a column of string format to find out a substring

```sql
select 
  REGEXP_SUBSTR(frd.C_EXT_DATA,'[^"]+', REGEXP_INSTR(frd.C_EXT_DATA,'"requestId":"')+12, 1) as "REQUESTID" 
from
  EX_TABLE frd;
```

## Working with timestamps

Filter by timestamp:

```sql
select 
  SOME_COL
from 
  SOME_TAB
where SOME_COL >= TO_TIMESTAMP('14-09-2021 10:00:00','DD-MM-YY HH24:MI:SS');
```

## Joins

Here follows the notation to join table A and B based on a column with common values. Inner join (shows only rows that match the condition)
```sql
select 
  A.*, B.*
from  
  TAB_A A, TAB_B B
where
  A.COLA = B.COLB
```
Left join of table A and table B shows all rows of A completing with NULLS corresponding B rows with missing matches of condition 
```sql
select 
  A.*, B.*
from  
  TAB_A A
left join
  TAB_B B on A.COLA = B.COLB
```

## Window functions

Compute derived quantities (such as time differences) of timestamp values (*COL1*) among rows by condition based on other column (*COL2*) value with window functions: (OPERATOR(COL1) over (PARTITION BY COL2))

```sql
select
        au.D_RECEIVED as TSreceived,
        max(au.D_RECEIVED) over (PARTITION BY au.N_GRP_MSG_ID) as TSfinished,
        (max(au.D_RECEIVED) over (PARTITION BY au.N_GRP_MSG_ID)) - au.D_RECEIVED as difTS
from 
  SOME_TAB au;
```

***

Return to **[main page](../README.md)** 
