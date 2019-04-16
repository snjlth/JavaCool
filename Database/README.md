# Database

1. Oracle - TRANSLATE function

      TRANSLATE('ABSDFSD', 'AB', '00'): <br/>
      Rules: 0-> original words, 1-> replace words, 2-> new words
    
        E.g. Replace "AB" to "OO" below:
        SELECT TRANSLATE('ABSDFSD', 'AB', '00') FROM DUAL;
        OUTPUT: 00SDFSD

2. clear Oracle cache

    `alter system flush buffer_cache;`<br/>
    `alter system flush shared_pool;`

3. Innder join example<br/> 

       drop table T_1; drop table T_2;
       create table T_1 (KEY_ATTR1 varchar2(20), KEY_ATTR2 varchar2(20),
       year number(4), month number(2), week number(3), unit number(5));
       
       create table T_2 (KEY_ATTR1 varchar2(20),
       year number(4), month number(2), door_count number(5));
       
       insert into T_1 values ('K1', 'K2-1', 2014, 1,1,20);
       insert into T_1 values ('K1', 'K2-2', 2014, 1,1,110);
       insert into T_1 values ('K1', 'K2-3', 2014, 1,1,40);
       insert into T_1 values ('K1', 'K2-4', 2014, 1,1,50);
       insert into T_1 values ('K2', 'K2-1', 2014, 1,1,20);
       insert into T_1 values ('K2', 'K2-2', 2014, 1,1,110);
       insert into T_1 values ('K2', 'K2-3', 2014, 1,1,40);
       insert into T_1 values ('K2', 'K2-4', 2014, 1,1,50);
       
       insert into T_2 values ('K1',2014, 1, 20);
       insert into T_2 values ('K2',2014, 1, 30);
       
       commit;
       
       select * from T_1;
       select * from T_2;
       select * from T_1 A, T_2 B WHERE A.KEY_ATTR1 = B.KEY_ATTR1 AND A.YEAR = B.YEAR AND A.MONTH = B.MONTH;

4. Oracle - Grouping function
        
        drop table g_1;
        create table G_1 (
        g1 varchar2(20),
        g2 varchar2(20),
        g3 varchar2(20),
        g4 number(5)
        );
        
        insert into g_1 values ('A', 'B', 'C', 111);
        insert into g_1 values ('A1', 'B1', 'C1', 222);
        insert into g_1 values ('A2', 'B2', 'C2', 333);
        insert into g_1 values ('A3', 'B3', 'C3', 444);
        insert into g_1 values ('', 'B4', 'C4', 5);
        insert into g_1 values ('A5', '', 'C5', 66);
        insert into g_1 values ('A6', 'B6', '', 7);
        insert into g_1 values ('A6', 'B7', '', 8);
        insert into g_1 values ('', '', '', 8);
        commit;
        
        select g1, grouping(g1) from g_1 group by g1;
        select g1, g2, grouping_id(g2, g1), sum(g4) from g_1 group by grouping sets(g2, g1);
        select g1, sum(g4) from g_1 group by g1;
        select g2, sum(g4) from g_1 group by g2;
