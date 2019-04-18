# ROWID vs ROWNUM

## ROWID
 
 **Definition**: In Oracle PL/SQL, the ROWID can be used as a
pseudocolumn and also as a Data Type.

1. As a pseudocolumn

    An Oracle server assigns each row in each table with a unique ROWID to identify the row in the table. The ROWID is the address of the row which contains the data object number, the data block of the row, the row position and data file. Using the ROWID method is generally considered to be the fastest way to search for a given row in the database. 
    
    It's important to note that ROWID values are not necessarily unique within a database. It is entirely possible for two rows of two different tables stored in the same cluster to have the same ROWID.
    
    The ROWID is intended to be immutable (that is, unchangeable) and the user should not try to change it once it is assigned. 
    
    Note that the  ROWID may change if the row is physically moved on disk, such as:
    
   -  Doing an export or import of the table
   -  Doing ALTER TABLE XXXX MOVE
   -  Doing ALTER TABLE XXXX SHRINK SPACE
   -  Doing FLASHBACK TABLE XXXX
   -  When splitting a partition
   -  When updating a value so that it moves to a new partition
   -  When combining two partitions
    
    **Example Usage:**
    
    SQL> SELECT ROWID,  ENAME, DEPTNO, SAL FROM EMPLOYEE;


2. As a Data Type

    As a Data Type, the ROWID is a valid data type of a column and is available in both SQL and Pl/SQL. It stores the ROWID pseudocolumn value of a row in the database.
    
    Example Syntax:
    [COLUMN] ROWID
    
    **Example Usage:**
    The below SQL statements create a table T_ROW with RID as ROWID column. Note that it is updated with ROWID of its own row.
    
    CREATE TABLE T_ROW (ID NUMBER, RID ROWID); <BR/> Table created.<BR/>
    INSERT INTO T_ROW(ID) VALUES(1); <BR/> 1 row created. <BR/>UPDATE
    T_ROW T SET RID = T.ROWID;<BR/> 1 row updated. <BR/>SQL> SELECT * FROM T_ROW;
    
    |ID |RID | 
    |---| ---|
    |AAAIYxAABAAAMiSAAA| 1`|

## ROWID vs ROWNUM

- ROWID
  -  ROWID is nothing but the physical memory location on which that data/row is stored.
  -  ROWID basically returns address of row.
  -  ROWID uniquely identifies row in database.
  -  ROWID is combination of data object number, data block in data file, position of row and data file in which row resides.
  -  ROWID is 16 digit hexadecimal number whose data type is also ROWID Or UROWID
    The fastest way to access a single row is ROWID
  -  ROWID is unique identifier of the ROW.
    
  -  ROWID is nothing but Physical memory allocation
  -  ROWID is permanant to that row which identifies the address of that
     row.
  -  ROWID is 16 digit Hexadecimal number which is uniquely identifies
     the rows.
  -  ROWID returns PHYSICAL ADDRESS of that row.
  -  ROWID is automatically generated unique id of a row and it is
     generated at the time of insertion of row.
  -  ROWID is the fastest means of accessing data.

- ROWNUM

  -  ROWNUM is magical column in Oracle which assigns the sequence number to the rows retreives in the table.
    To limit the values in the table you can use rownum pseudocolumn
  - ROWNUM is nothing but logical sequence number given to the rows
    fetched from the table.
  - ROWNUM is logical number assigned temporarily to the physical
    location of the row.
  -  You can limit the values in the table using rownum ROWNUM is also
    unique temparary sequence number assigned to that row.
    
  -  ROWNUM is nothing but the sequence which is allocated to that data
     retrieval bunch.
  -  ROWNUM is tempararily allocated sequence to the rows.
  -  ROWNUM is numeric sequence number allocated to that row temporarily.
  -  ROWNUM returns the sequence number to that row.
  -  ROWNUM is an dynamic value automatically retrieved along with select statement output.
  -  ROWNUM is not related to access of data.
