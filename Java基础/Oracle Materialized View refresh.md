# Oracle Materialized View refresh

## 简述
Materialized views - 物化视图 - 简称 MV，是已经被存储的或者说被物化-'materialized' 成 schema对象的查询结果。其中查询的 'From' 子句可以给 table, view 和 materialized view命名。这些用来建立 MV的对象都可以被称为 主表-master tables (a replication term) 或者 具体表-detail tables (a data warehousing term)。

## Refresh Process
MV中的数据不必和主表-master table 当前的数据总是一致。为了保持 MV中的数据和主表数据一致，必须来刷新 MV。

对于刷新 MV， Oracle 支持几种刷新类型。

## Refresh Types
Oracle 支持快速刷新 - fast， 完全刷新 - complete 和 强制刷新 - force。

## 完全刷新 - Complete Refresh
DBMS_MVIEW.REFRESH(MV_NAME, method =>'C');

对 MV执行 complete refresh， server 将会执行 MV的定义语句，本质上就是重新创建一遍这个MV。用新的结果集替换已经存在的旧的结果集。Oracle 可以对任意一个 MV 执行 complete refresh。Complete refresh 执行的时间依赖于其定义语句的查询效率，基本上来说，complete refresh 相比于快速刷新-fast refresh 时间长。

如果对于主 MV执行了 complete refresh，那么依赖于这个 MV创建的其他 MV也必须使用 complete refresh。如果对这些 MV使用 fast refresh，Oracle 会返回 error：

　　ORA-12034 mview log is younger than last refresh

## 快速刷新 - Fast Refresh
DBMS_MVIEW.REFRESH(MV_NAME, method =>'F');

执行 fast refresh， Oracle会首先识别当前主表和最后一次刷新 MV时候数据的变化，根据变化的数据更新 MV。当变化比较小的时候，Fast Refresh 比 Complete Refresh更加高效，因为数据量比较少。

因为 fast refresh 时需要和最近一次更新后的数据进行比较，所以需要 MV所依赖的主表必须有日志log 文件。并且，为了 fast refresh 比 complete refresh 更快速，在创建 MV时每个join 的 column 必须有索引。

另外，如果 MV是基于 partitioned master tables，则可能需要使用 PCT Refresh - Partition Change Tracking。

DBMS_MVIEW.REFRESH(MV_NAME, 'P');

## 强制刷新 - Force Refresh
DBMS_MVIEW.REFRESH(MV_NAME, method =>'?');

执行 force refresh 时， Oracle会先试图执行 fast refresh。如果发现 fast refresh 不可行，则执行 complete refresh。

## 刷新方法

<table style="height: 248px; width: 905px;" border="0">
<tbody>
<tr>
<td>Refresh Option</td>
<td>Parameter</td>
<td>Description</td>
</tr>
<tr>
<td>COMPLETE</td>
<td>C</td>
<td>Refreshes by recalculating the defining query of the materialized view.</td>
</tr>
<tr>
<td>FAST</td>
<td>F</td>
<td>
<p>Refreshes by incrementally applying changes to the materialized view.</p>
<p>For local materialized views, it chooses the refresh method which is estimated by optimizer to be most efficient. The refresh methods considered are log-based&nbsp;<code>FAST</code>&nbsp;and&nbsp;<code>FAST_PCT</code>.</p>
</td>
</tr>
<tr>
<td>FAST_PCT</td>
<td>P</td>
<td>Refreshes by recomputing the rows in the materialized view affected by changed partitions in the detail tables.</td>
</tr>
<tr>
<td>FORCE</td>
<td>?</td>
<td>
<p>Attempts a fast refresh. If that is not possible, it does a complete refresh.</p>
<p>For local materialized views, it chooses the refresh method which is estimated by optimizer to be most efficient. The refresh methods considered are log based&nbsp;<code>FAST</code>,&nbsp;<code>FAST_PCT</code>, and&nbsp;<code>COMPLETE</code>.</p>
</td>
</tr>
</tbody>
</table>


Refresh Option	Parameter	Description
COMPLETE	C	Refreshes by recalculating the defining query of the materialized view.
FAST	F	
Refreshes by incrementally applying changes to the materialized view.

For local materialized views, it chooses the refresh method which is estimated by optimizer to be most efficient. The refresh methods considered are log-based FAST and FAST_PCT.

FAST_PCT	P	Refreshes by recomputing the rows in the materialized view affected by changed partitions in the detail tables.
FORCE	?	
Attempts a fast refresh. If that is not possible, it does a complete refresh.

For local materialized views, it chooses the refresh method which is estimated by optimizer to be most efficient. The refresh methods considered are log based FAST, FAST_PCT, and COMPLETE.

## 小结 对于 FAST_PCT 刷新方式没有使用过，不了解，只是在
Oracle文档中看到的。MV还可以选择是 ON COMMIT刷新和ON DEMAND，即主表
commit之后 MV开始刷新和在需要刷新的时候进行刷新的两种方式。

## 参考资料
Overview of Materialized Views
Refresh Types
Table 16-1 ON DEMAND Refresh Methods