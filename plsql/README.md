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

## Window functions

Compute derived quantities (such as time differences) of timestamp values (*COL1*) among rows by condition based on other column (*COL2*) value with window functions: (OPERATOR(COL1) over (PARTITION BY COL2))

```sql
select
        au.D_RECEIVED as TSreceived,
        max(au.D_RECEIVED) over (PARTITION BY au.N_GRP_MSG_ID) as TSfinished,
        (max(au.D_RECEIVED) over (PARTITION BY au.N_GRP_MSG_ID)) - rd.D_RECEIVED as difTS
from 
  SOME_TAB au;
```

***

Return to **[main page](../README.md)** 
