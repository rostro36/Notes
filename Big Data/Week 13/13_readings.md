# Big Data Week 13 readings
## [Database Systems Chapter 10.6](https://people.inf.elte.hu/miiqaai/elektroModulatorDva.pdf)
OLAP:
- **O**n
- **L**ine
- **A**nalytic
- **P**rocessing
Highly complex queries with one or more aggregations over large amounts of data in a *data warehouse*, a copy of the master database(s), that gets updated only daily or even weekly and do not disturb the master databases that touches a lot of rows

OLTP:
- **O**n
- **L**ine
- **T**ransaction
- **P**rocessing

The *fact table* is the central collection of data inside a warehouse.

*Raw-data cube*: only the raw data, no aggregation therefore may have duplicates
*Formal data cube*: includes aggregations of the data in all subsets and the data itself
					maybe aggregation is already done for the first time to remove duplicates
*ROLAP*: Relational OLAP, data saved in a star schema, where the central fact table is composed of raw data cubes
*MOLAP*: Multidimensional OLAP, uses formal data cubes and complex operators

*Star Schema*: consists of a schema for the fact table in the centre, which links to other relations on the rays, stored in *dimension tables*
*Dimensions* open the space to describe the item, where *dependent attributes* are interesting for the end user, e.g. price. 
Each dimension is a foreign key in a (different) dimension table, whose columns can be used to aggregate in other tables, mainly the fact table.
*Dicing*: a choice of partition (group by) and also aggregation of the raw data
*Slicing*: where clause, filtering values
*Drill-down*: partitioning more finely
*Roll-up*: partition more coarsely
## [Database Systems Chapter 10.7](https://people.inf.elte.hu/miiqaai/elektroModulatorDva.pdf)
focus on *formal* data cube, precomputes all possible aggregates in a systematic way, not much more space needed, pre aggregation done.

Add/Precompute sides to the cube, which represent *any* and is an aggregate over those rows it is the side of.
```
CREATE  MATERIALIZED  VIEW  SalesCube  AS
SELECT  model,  color,  date,  dealer,  SUM(val),  SUM(cnt) 
FROM  Sales
GROUP  BY  model,  color,  date,  dealer  WITH  CUBE;
```
Gives the powerset of all possible aggregates e.g.
( ’Gobi’,  NULL,  ’2001-05-21’,  NULL,  2348000,  100)
( ’Gobi’,  NULL,  NULL,  NULL,  1339800000,  58000)
(NULL,  NULL,  NULL,  NULL,  3521727000,  198000)

```
CREATE  MATERIALIZED  VIEW  SalesRollup  AS
SELECT  model,  color,  date,  dealer,  SUM(val),  SUM(cnt)
FROM  Sales
GROUP  BY  model,  color,  date,  dealer  WITH  ROLLUP;
```
Rolls up from behind, starting with dealer, then dealer and date and so on until it has all:
( ’Gobi’,  ’red’,  ’2001-05-21’ ,  ’Friendly  Fred’,  45000,  2)
( ’Gobi’,  ’red’,  ’2001-05-21’,  NULL,  3678000,  135)
( ’Gobi’,  ’red’,  NULL,  NULL,  657100000,  34566)
(’Gobi’,  NULL,  NULL,  NULL,  1339800000,  58000)
(NULL,  NULL,  NULL,  NULL,  3521727000,  198000)