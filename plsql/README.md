# Basic introduction

## Regular expressions

Parse a column of string format to find out a substring

```sql
select 
  REGEXP_SUBSTR(frd.C_EXT_DATA,'[^"]+', REGEXP_INSTR(frd.C_EXT_DATA,'"requestId":"')+12, 1) as "REQUESTID" 
from
  EX_TABLE frd;
```

Above we are starting to find at position 'i=REGEXP_INSTR(frd.C_EXT_DATA,'"requestId":"')+12'. The latter statemet searches in the string frd.C_EXT_DATA the substring *resquestId":"*, and provides the position where the substring is found (+12). Then, using this position 'i' as a start, 'REGEXP_SUBSTR(frd.C_EXT_DATA,'[^"]+', i, 1)' finds out the chain of characters including all (+ sign) and stopping whenever it finds the first occurrence of character *"*

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

## Control statements in query

With the DECODE function we print out a row value for a new column based on conditions:

```sql
select
        unique au.N_GRP_MSG_ID,
        rd.D_RECEIVED as TSreceived,
        DECODE(
              rd.V_STATUS_CD, NULL, NULL,              
              (max(au.D_RECEIVED) over (PARTITION BY au.N_GRP_MSG_ID)
              )
        ) as TSfinished,
        DECODE(
              rd.V_STATUS_CD, NULL, NULL,        
              (max(au.D_RECEIVED) over (PARTITION BY au.N_GRP_MSG_ID)) - rd.D_RECEIVED 
        ) as difTS
from 
    OFSAAFCCM.FSI_RT_AUDIT au, OFSAAFCCM.FSI_RT_RAW_DATA rd
where 
    rd.N_GRP_MSG_ID = au.N_GRP_MSG_ID;
```

In the where part we can use the following logic operators and much more
```sql
...
where
  au.COL1 != 'SUCCESS' and
  frd.COL2 not like '%TXT%' and
  frd.COL3 like '%dc12%' and
  au.COL3 not in ('AA', 'BB', 'CD') and
  au.COL3 in ('A1', 'B2') and
  frd.COL4 is not null and
  frd.COL5 is null and
  
```

## Selects as tables

We can group a select query into another select and treat the former as a normal table as follows:

```sql
select SEL_TAB.*, TAB2.*, TAB3.* from 
  (
    select a1.COL_1 C1, ...  from TAB1 a1, TAB2 a2 where ...
  ) SEL_TAB
left join
  TABLE_2 TAB2 on SEL_TAB.C1 = TAB2.INDEX1
...
```

***

Return to **[main page](../README.md)** 
