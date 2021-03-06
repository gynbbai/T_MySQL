PK
    ่~Jsธ[_  _    ) MySQL ์ฟผ๋ฆฌ ํ๋ ์ต์ ํ.mdup% d`ZMySQL ์ฟผ๋ฆฌ ํ๋ ์ต์ ํ.md# MySQL ์ฟผ๋ฆฌ ํ๋ ์ต์ ํ

**Day 1**

## MySQL ๊ฐ์

### MySQL ๊ฐ์
- ์คํ์์ค DBMS, 1995.03 ์ฒซ ๋ฒ์  ๋ฐํ
- [LAMP](https://ko.wikipedia.org/wiki/LAMP) ์ ์ค์ฌ ์์
<br>

### MySQL ํน์ง
- ๋์ฉ๋ DB์ ์ฌ์ฉ๊ฐ๋ฅ(ํ์ด๋ธ ๊ธฐ๋ณธ ๊ฐ์ `256TB`)
- ํ๋์ ํ์ด๋ธ์ `64`๊ฐ์ index ์ค์  ๊ฐ๋ฅ. 
- ํ๋์ index์ `16`๊ฐ์ ์นผ๋ผ ์ง์ ๊ฐ๋ฅ
<br>

### MySQL Architecture
![@MySQL Architecture | 500x0](http://cfs10.tistory.com/image/20/tistory/2009/02/18/18/21/499bd3254afb6)
<br>

#### ์๋ฒ ์์ง
- SQL interface, Parser, Optimizer, Cache & Buffer ๋ก ๊ตฌ์ฑ
- ์ฟผ๋ฆฌ ์ฌ๊ตฌ์ฑ, ๋ฐ์ดํฐ ์ฒ๋ฆฌ
- ์คํ ๋ฆฌ์ง ์์ง์ ๋ฐ์ดํฐ ์์ฒญ
- Join, Group by, Order by ์ฒ๋ฆฌ
- ํจ์, ํธ๋ฆฌ๊ฑฐ, ํ๋ก์์  ์ฒ๋ฆฌ
<br>

#### ์คํ ๋ฆฌ์ง ์์ง
```sql
show engines;
```

| Engine | Support | Comment | Transactions | XA | Savepoints |
|--------------------|---------|----------------------------------------------------------------|--------------|------|------------|
| InnoDB | DEFAULT | Supports transactions, row-level locking, and foreign keys | YES | YES | YES |
| MRG_MYISAM | YES | Collection of identical MyISAM tables | NO | NO | NO |
| MEMORY | YES | Hash based, stored in memory, useful for temporary tables | NO | NO | NO |
| BLACKHOLE | YES | /dev/null storage engine (anything you write to it disappears) | NO | NO | NO |
| MyISAM | YES | MyISAM storage engine | NO | NO | NO |
| CSV | YES | CSV storage engine | NO | NO | NO |
| ARCHIVE | YES | Archive storage engine | NO | NO | NO |
| PERFORMANCE_SCHEMA | YES | Performance Schema | NO | NO | NO |
| FEDERATED | NO | Federated MySQL storage engine | NULL | NULL | NULL |
<br>

##### ์คํ ๋ฆฌ์ง ์์ง๋ณ ํน์ง
- **MyISAM** 
	- File ๊ธฐ๋ฐ
	- ๋์คํฌ์ ์ง์  ์ ๊ทผ, ๋ฉ๋ชจ๋ฆฌ์ ์ ์ฅํ์ง ์๋๋ค.
	- ํธ๋์ญ์ X, ํ์ด๋ธ ๋จ์ Lock
- **InnoDB**
	- ํธ๋์ญ์ O
	- ํ๋จ์ Lock
	- ๋ฐ์ดํฐ์ ์ธ๋ฑ์ค๋ฅผ ๋ฉ๋ชจ๋ฆฌ์ ์ ์ฅ
		- InnoDB_buffer_pool_size ํฌ๊ธฐ๊ฐ ์ฑ๋ฅ์ ์ํฅ
- **archive**
	- index X
	- ๋ฐ์ดํฐ ์์ถ ์ ์ฅ
<br>

### ๊ฐ์ ๋ด์ฉ 

#### ์คํค๋ง ์์ฑ
`hr-schema-mysql.sql` ์ ๋ด์ฉ์ ์คํํด์ ์๋ก์ด `hr` ์คํค๋ง ์์ฑ (๋ณต์ฌ ํ ์ฟผ๋ฆฌ ์คํ)
<br>

#### ERD ๋ง๋ค๊ธฐ
Menu - Database - Reverse Enginner(Ctrl + R) - `hr` ์ ํ

<br>

## MySQL ์ฑ๋ฅ ํฅ์, ์ฟผ๋ฆฌ ์ต์ ํ, ์คํ ๊ณํ

### ์ฑ๋ฅ ํฅ์

#### DB ์ฑ๋ฅ์ I/O ์์์ ๊ฐ์ฅ ์ํฅ์ ๋ง์ด ๋ฐ๋๋ค.

#### DB ์ฑ๋ฅ
- ์ ํ๋ฆฌ์ผ์ด์ ๋ก์ง
- ํ์ดํฐ ๋ชจ๋ธ๋ง, ํ์ด๋ธ ๊ตฌ์กฐ
- **์ฟผ๋ฆฌ ํ๋**
- ํ๋์จ์ด ์ฑ๋ฅ ํ๋
- ์๋ฒ ํ๊ฒฝ ํ๋
<br>

### SW ๋ ๋ฒจ
- ํ์ด๋ธ์ ๊ตฌ์กฐ
	- ์นผ๋ผ์ ํ์์ด ์ ์ ํ์ง ์ดํด๋ด์ผ ํ๋ค.
- ์ธ๋ฑ์ค
	- ์ ์ ํ ์นผ๋ผ์ ์ธ๋ฑ์ค๊ฐ ์ ํด์ ธ ์๋์ง ํ์ธ
- ์คํ ๋ฆฌ์ง ์์ง ์ ํ
- Lock ์ ๋ต
- ๋ฉ๋ชจ๋ฆฌ ์บ์๋
	- ์บ์ ๋ฉ๋ชจ๋ฆฌ์ ์ฌ์ด์ฆ๊ฐ ์ ์ ํ์ง ํ์ธ 
<br>

### HW ๋ ๋ฒจ
- ๋์คํฌ ๊ฒ์ ์๊ฐ
- ๋์คํฌ I/O ์๋
- CPU ์ฌ์ดํด
<br>

### ์ฟผ๋ฆฌ ์ต์ ํ
- DBMS์์ ์ฟผ๋ฆฌ๊ฐ ์คํ๋๋ ๊ตฌ์กฐ๋ฅผ ์์์ผ ํจ
- ๋ก๊ทธ ๋ถ์์ ํตํด `๋๋ฆฐ ์ฟผ๋ฆฌ๋ฅผ ์ฐพ๋ ๋ฐฉ๋ฒ`์ ์์์ผ ํจ
- ํ๋กํ์ผ๋ง
- **์ฟผ๋ฆฌ๋ฌธ ์ต์ ํ ๋ฐฉ๋ฒ**
	- ์ค์  ์ฟผ๋ฆฌ๋ฌธ ์คํ ํ ๊ฒฐ๊ณผ ํ์ธ
	- ํ๋กํ์ผ๋ง
		- ```select @@profilinig;```
	- ์คํ ๊ณํ ๋ถ์ explain
<br>

### ํ๋กํ์ผ๋ง
- ํ์ฌ  ์ธ์์์ ํด๋ฆฌ๊ฐ ์คํ๋  ๋ ๋ฆฌ์์ค ์ฌ์ฉ๋ ํ์ธ
- ํ๋กํ์ผ ํ๊ฒฝ ๋ณ์ ์ค์ 
	- ```set profiling = 1;```
	- ๋ํดํธ `0`, `1`๋ก ์ค์ ํ๋ฉด ์ฟผ๋ฆฌ ์คํ ๋ก๊ทธ๊ฐ ๊ธฐ๋ก ๋จ
- ํ๋กํ์ผ ๋ชฉ๋ก ๋ณด๊ธฐ
	- ```show profile;```
	- ```show profiles;```
	- ```show profile for query 4;```
<br>

### ์คํ ๊ณํ
- ์ฟผ๋ฆฌ ์คํ ๊ณผ์ ์์ ์ด๋ ํ ์์์ ์กฐํฉ์ผ๋ก ์ฟผ๋ฆฌ๊ฐ ์คํ๋๋ ์ง ์๋ ค์ค
- ๋ฐ๋์ ์คํ ๊ณํ์ ๋ฐ๋ผ ์ํ๋์ง๋ ์๋๋ค.
- ์ฟผ๋ฆฌ๋ฌธ ์์ ```explain```์ ๋ถ์ธ๋ค
- 5.7 ๋ฒ์ ๋ถํฐ `SELECT` ์ด์ธ ์ฟผ๋ฆฌ๋ ๊ฐ๋ฅ

#### ์คํ ๊ณํ ์์ธ
| item | contents |
|---------------|-------------------------------------------|
| id | select ๋ฒํธ |
| select_type | select ์ข๋ฅ |
| table | ์ฐธ์กฐ ํ์ด๋ธ ๋ช |
| type | ์กฐ์ธ์ด๋ ์กฐํ ์ข๋ฅ |
| possible_keys | ์ฌ์ฉ๊ฐ๋ฅํ index ๋ชฉ๋ก |
| key | ์ฌ์ฉ๋ index |
| key_len | index ๊ธธ์ด |
| ref | key ์ ํจ๊ป ์ฌ์ฉ๋ ์นผ๋ผ ํน์ ์์ ๊ฐ |
| rows | ์กฐํ ํ ์ |
| filtered | ์กฐํ ์์ ํ ์ถ์ ๋น(์กฐํํ ์ / ์ ์ฒดํ ์) |
| Extra | ์ถ๊ฐ ์ ๋ณด |

#### ์คํ ๊ณํ select_type
| select_type | description |
|--------------------|--------------------------------------------|
| SIMPLE | ๋จ์ํ select union์ด๋ sub query๊ฐ ์๋ค |
| PRIMARY | ์ ์ผ ์ธ๊ณฝ์ select(sub query ๊ฐ ์๋ ๊ฒฝ์ฐ) |
| DERIVED | from ์  ๋ด๋ถ์ sub query |
| DEPENDENT SUBQUERY | ์ํธ ์ฐ๊ด๋ sub query |
| UNION | union ์์ ๋๋ฒ์งธ ํน์ธ ๋์ค select |
| SUBQUERY | sub query์ ์ฒซ ๋ฒ์งธ select |

#### ์คํ ๊ณํ type
| type | description |
|-----------------|--------------------------------------------------------------------------------------------|
| system | table ์ ๋ฐ์ดํฐ๊ฐ 1๊ฐ์ธ ๊ฒฝ์ฐ |
| const | PK ๋ Unique key ๋ฅผ ์์์ ๋น๊ตํ๋ ๊ฒฝ์ฐ. ๋ฐ์ดํฐ๊ฐ ํ ๊ฐ ์กด์ฌ |
| eq_ref | ์กฐ์ธ์์ PK ํน์ Unique key ๊ฐ ์ฌ์ฉ๋ ๊ฒฝ์ฐ |
| ref | where ์์ ์ฌ์ฉ๋ ์ปฌ๋ผ์ด index๋ก ์ฐธ์กฐ๋๊ฑฐ๋ ์กฐ์ธ์์ PK Unique key ์ด์ธ ์นผ๋ผ์ด ์ฌ์ฉ๋ ๊ฒฝ์ฐ |
| ref_or_null | ref ์ ๋์ผ. Null์ด ์ถ๊ฐ๋์ด ์ฌ์ฉ๋ ๊ฒฝ์ฐ |
| index_merge | ๋๊ฐ์ index ์ฌ์ฉ๋ ๊ฒฝ์ฐ |
| unique_subquery | ์๋ธ์ฟผ๋ฆฌ์์ in ๋ด๋ถ์ PK ๊ฐ ์ฌ์ฉ๋ ๊ฒฝ์ฐ |
| index_subquery | unique_subquery ์ ์ ์ฌ. ์ผ๋ฐ index ์ฌ์ฉ๋ ๊ฒฝ์ฐ |
| range | ๋ฒ์ ์ค์บ |
| index | ์ธ๋ฑ์ค๋ฅผ Full scan. ์ปค๋ฒ๋ง index. ํ์ด๋ธ ์กฐํ ์์ด index๋ก๋ง ๋ฐ์ดํฐ ๊ฐ์ ธ์จ ๊ฒฝ์ฐ |
| all | ํ์ด๋ธ Full scan |

#### ์คํ ๊ณํ Extra
| Extra | description |
|-----------------|---------------------------------------------------------------|
| Using Index | ์ปค๋ฒ๋ง index. Index๋ก๋ง ๊ฒฐ๊ณผ๋ฅผ ์ถ์ถํ ๊ฒฝ์ฐ |
| Using Where | where ์กฐ๊ฑด์ผ๋ก ๋ฐ์ดํฐ ์ถ์ถ |
| Using Filesort | ๋ฐ์ดํฐ ์ ๋ ฌ์ด ํ์ํ ๊ฒฝ์ฐ |
| Using Temporary | ์์ ํ์ด๋ธ ์ฌ์ฉ. group by, order by ๊ฐ ํฌํจ๋ ๊ฒฝ์ฐ ์ฃผ๋ก ๋ฐ์ |


<br>

### ๊ฐ์ ๋ด์ฉ 
./day1.sql

<br>

```powershell
# ํ์ฌ ํด๋์ sql ํ์ผ์ ๋ชจ๋ ํฉ์ณ all.sql๋ก ๋ง๋ฌ
copy *.sql all.sql
```
```powershell
# all.sql ์ myhr ์คํค๋ง์ ๋ฃ์ผ๋ฉด์ DB ์ ์
# ์ฌ์ ์ myhr ์ด๋ผ๋ ์คํค๋ง๊ฐ ์์ด์ผ ํจ
# worckbench Navigator ์์ ์ค๋ฅธ์ชฝ ํด๋ฆญ์ผ๋ก ์์ฑ
mysql -u root -p myhr < all.sql
```
./myhr.sql

### ์ค์ต
#### ๋ฌธ์ 
hr ์คํค๋ง๋ฅผ ์ด์ฉํ์ฌ `Seattle` ์ ๊ทผ๋ฌดํ๋ ์ฌ์๋ค์ ์ฑ, ์ด๋ฆ, ๋ถ์๋ช, ๊ธ์ฌ, ์์ฌ์ผ์ ์ถ๋ ฅํ๊ณ  ์คํ๊ณํ์ ์ค๋ช.
<br>

#### ๊ฒฐ๊ณผ SQL
```sql
select e.last_name, e.first_name, d.department_name, e.salary, e.hire_date
from departments d
join locations l using(location_id)
join employees e using(department_id)
where l.city = 'Seattle';
```
#### ์คํ ๊ณํ
![@๊ธฐ์กด ์คํ ๊ณํ | day1-work1-explain](https://cloud.githubusercontent.com/assets/9030565/24394715/0779e696-13d8-11e7-9857-7354d9099750.PNG)

- table `l`์ type ์ด `ALL` ์ธ ์ด์ ๋ ```where l.city = 'Seattle'``` ์ฟผ๋ฆฌ์ `city` ์นผ๋ผ์ด PK, index ๋ชจ๋ ์๋๊ธฐ ๋๋ฌธ์ Full scan ํ๊ธฐ ๋๋ฌธ์ด๋ค.
- table `d`, `e` ๋ type ์ด `ref` ์ธ ์ด์ ๋ join ์ ์นผ๋ผ์ด PK, index, unique ์ค ํ๋์๊ธฐ ๋๋ฌธ์ด๋ค.
- Full scan์ ๋ฐฉ์งํ๋ ค๋ฉด locations `city` ์นผ๋ผ์ index์ ์ถ๊ฐํ๋ค.

#### locations ์ index ์์ฑ
```sql
create index location_city_idx on locations(city);
```

#### index ์์ฑ ํ ์คํ ๊ณํ
![@์์  ์คํ ๊ณํ | day1-work1-explain index](https://cloud.githubusercontent.com/assets/9030565/24394734/194622ea-13d8-11e7-8250-0bc27e700dda.PNG)

<br>

## Index ์ดํด์ ์ ์ฉ 

### Index ๊ฐ์
#### ๊ธฐ๋ณธ ๊ฐ๋
- SELECT ์ฑ๋ฅ ํฅ์์ ์ํด ์์ฑ๋ ๋ณ๋ ๋ฐ์ดํฐ
- Table ๊ณผ๋ ๋ณ๋๋ก ์กด์ฌ
- CUD ์ index๋ ๋ณ๊ฒฝ
#### ์ฅ์ 
- SELECT ๋น ๋ฆ
- ์ ๋ ฌ ๋น ๋ฆ
#### ๋จ์ 
- Table ์ด์ธ์ ์ ์ฅ๊ณต๊ฐ ํ์
- ๋ฐ์ดํฐ ๋ณ๊ฒฝ์ index ๋ ๋ณ๊ฒฝ๋์ overhead 
<br>

### Index ์ข๋ฅ
#### ์๊ณ ๋ฆฌ์ฆ ๋ถ๋ฅ
- **B-Tree**
- R-Tree
- Hash Index
- Full-Text Index
#### ์นผ๋ผ ๋ถ๋ฅ
- ๋จ์ผ ์ปฌ๋ผ Index
- ๋ณตํฉ ์ปฌ๋ผ Index
- ๋ถ๋ถ Index
- ์ปค๋ฒ๋ง Index
<br>

### B-Tree
- Root, Branch, Leaf ๋ก ๊ตฌ์ฑ
- ์ด๋ ํ ๋ฐ์ดํฐ๋ ์ผ์ ํ ์๊ฐ์ด ์์
- ๋ฐ์ดํฐ ๋ณ๊ฒฝ ์ ์ฌ๊ตฌ์ฑ ํ์
<br>

### MySQL Index
- `PK`, `Unique`, `Key` ๋ก ๊ตฌ๋ถ
- `InnoDB` ์์  PK ์ ๋ฐ๋ผ ํด๋ฌ์คํฐํ ๋จ
- ๋ณด์กฐ Index ๋ ๋ด๋ถ์ ์ผ๋ก PK ๋ฅผ ํฌํจํ์ฌ ์์ฑ
	- index(email) ์ index(email, id) ์ ๊ฐ๋ค.
	- **PK ๋ก ์ง์ ๋๋ ์นผ๋ผ์ ๊ธธ์ด๊ฐ ๊ธธ์ด์ง๋ ๊ฒ์ ์ง์ํ  ๊ฒ**
<br>

**Day 2**

### MySQL Index - InnoDB ์คํ ๋ฆฌ์ง Index
- InnoDB๋ `Clustered Index` ์ `Secondary Index` ๊ฐ ์๋ค.
- **Clustered Index**
	- PK ์ ์๋์ผ๋ก `Clustered` ์์ฑ
	- ๋ฐ์ดํฐ๊ฐ ์์๋๋ก ์ ๋ ฌ
	- ํ์ด๋ธ ๋น **ํ๋**์ Clustered Index
	- Unique, Not null ์กฐ๊ฑด ๋ถ์ฌ์ Clustered Index ์์ฑ, PK๊ฐ ์์ผ๋ฉด Clustered Index๋ ์์ฑ๋์ง ์๋๋ค. (ํ์ด๋ธ ๋ค ํ๋์ด๊ธฐ ๋๋ฌธ)
	- ๊ฒ์ ์๋ ๋น ๋ฆ. ์๋ ฅ, ์์ , ์ญ์  ๋๋ฆผ
- **Secondary Index**
	- ์๋ ฅ, ์์ , ์ญ์  ๋น ๋ฆ 
	- ๊ฒ์์ `Clustered` ์ ๋นํด ๋๋ฆผ
	- ํ์ด๋ธ์ ์ฌ๋ฌ๊ฐ ์์ฑ ๊ฐ๋ฅ
< br>

### Index ๋ช๋ น์ด
#### ์์ฑ
- PK ์ง์ ์ index ์๋ ์์ฑ
- FK ์ง์ ์ index ์๋ ์์ฑ
```sql
CREAE [UNIQUE | FULLTEXT | SPATIAL] INDEX index_name
[index_type]
ON table_name(column_names)
[index_option]
[algorithm_option | lock_option]
```
<br>

#### ์กฐํฌ
```sql
show indexes from table_name;

# ํต๊ณ ์ ๋ณด
select * from information_schema.STATISTICS where table_name = 'employees';
```
<br>

#### ์ญ์ 
- index๋ **์์  ๋ถ๊ฐ**
- ์์ ํ๋ ค๋ฉด ์ญ์  ํ ๋ค์ ์์ฑ
- **Index๋ฅผ ์ญ์ ํด์ผ ํ๋ ๊ฒฝ์ฐ**
	- ๋์ฉ๋ ๋ฐ์ดํฐ๋ฅผ ๋ฃ๋ ๊ฒฝ์ฐ
		- ๋ฐ์ดํฐ insert ๋ง๋ค index๋ฅผ update ํด์ผํ๊ธฐ ๋๋ฌธ์ ์ผ๋จ ์ง์ฐ๊ณ  ๋ฐ์ดํฐ ๋ฃ๊ณ  index๋ฅผ ๋ค์ ์์ฑํ๋ค.
	- ์ฌ์ฉํ์ง ์๋ index ์๋ ๊ฒฝ์ฐ
```sql
drop index index_name on table_name;
# or
alter table table_name drop index index_name;
```
<br>

### Index ์ข๋ฅ
#### ๋จ์ผ ์ปฌ๋ผ Index
- Unique Index, = ๊ฒ์
```sql
explain
select last_name, first_name, salary, hire_date 
from employees where employee_id = 100;
```

- Unique Index, ๋ฒ์
```sql
explain
select last_name, first_name, salary, hire_date 
from employees where employee_id >= 100;
```

- Non-Unique Index, = ๊ฒ์
```sql
explain
select last_name, first_name, salary, hire_date 
from employees where salary = 8000;
```

- Non-Unique Index, ๋ฒ์
```sql
explain
select last_name, first_name, salary, hire_date 
from employees where salary between 8000 and 9000;
```

- OR & IN ์กฐ๊ฑด
```sql
explain
select last_name, first_name, salary, hire_date 
from employees where employee_id in (100, 200);
```
<br>

#### ๋ณตํฉ ์ปฌ๋ผ Index
```sql
alter table employees add index(last_name, first_name);

explain
select last_name, first_name, salary, hire_date 
from employees where last_name like 'K%';
```
<br>

#### ์ปค๋ฒ๋ง Index
- Index ๋ง์ผ๋ก ๋ฐ์ดํฐ๋ฅผ ์กฐํ. ํ์ด๋ธ์ ์ ์ฅ๋ ๋ฐ์ดํฐ๋ฅผ ์กฐํํ์ง ์๋๋ค.
- ์คํ ๊ณํ  type ์ด `index`
<br>

#### Prefix Index
- ์ปฌ๋ผ์ ์ผ๋ถ๋ถ์ผ๋ก index ์์ฑ

##### ํน์ง
- indexํฌ๊ธฐ ์ค์
- ์ปค๋ฒ๋ง index ์ฌ์ฉ ๋ถ๊ฐ
- BLOB / TEXT ํ์ ์ปฌ๋ผ ์ ์ฉ
- **์ ์ ํ ๊ธธ์ด ํ์**
	- `Selectivity`(์ ํ๋) ๋ก ๊ธธ์ด ํ๋จ
```sql
alter table emp add index(last_name(4));
```

##### ์๋ ์์ฑ ์นผ๋ผ (v5.7 ์ด์)
MySQL์๋ Oracle์ Function based index ๋ ์์ผ๋, **์๋ ์์ฑ ์นผ๋ผ**์ ์ด์ฉํด ์ด์ ๋น์ทํ ํจ๊ณผ๋ฅผ ๋ผ ์ ์๋ค.
``` sql
create table some_table(
	id varchar(10),
	sub_id varchar(8) as (substring(id, 1, 8)),
	index(sub_id)
);

insert some_table(id) values('sub_id_001');

select * from some_table;
```
<br>

### Index ํํธ
- **use index**
	- Index ์ฌ์ฉ
	- ```... from employee use index(name_idx) ...```
- **ignore index**
	- Index ์ฌ์ฉ X
	- - ```... from employee ignore index(name_idx) ...```
- **force index**
	- index ๊ฐ์  ์ฌ์ฉ
	- - ```... from employee force index(name_idx) ...```
<br>

### Index ์ฌ์ฉ๋ถ๊ฐ Case
1. `not` ์ฐ์ฐ์
2. `is not null` ์ฐ์ฐ์
3. ์ปฌ๋ผ์ ๋ณํํ์ฌ ๋น๊ต
```sql
# salary ์๋ index๊ฐ ์์ง๋ง, * 12 ๋๋ฌธ์ ์ปฌ๋ผ ๋ณํ๋จ
explain
select * from employees where salary*12 >= 120000;

# ํด๊ฒฐ์ฑ - ์ปฌ๋ผ ๋ณํ X, ์กฐ๊ฑด์์ ๋ณํ!
explain
select * from employees where salary >= (120000/12);
```
4. ์นผ๋ผ ํ์์ด ์๋ ๋ณํ๋๋ ๊ฒฝ์ฐ
```sql
create table my_internal(
	t_no varchar(10) primary key, # PK๋ฅผ varchar๋ก 
    t_name varchar(20)
);

insert into my_internal(t_no, t_name) values(1234, 'hong'); # ์๋ ํ๋ณํ

explain select * from my_internal where t_no = 1234; # type: All, index ์ฌ์ฉ X, ์ฑ๋ฅ ์ด์ ์ผ๊ธฐ

explain select * from my_internal where t_no = '1234'; # type: const
```
5. ๋ณตํฉ ์นผ๋ผ์์ AND, OR ์ ๋ฐ๋ผ
```sql
# index ์ฌ์ฉ O
explain
select * from employees where last_name = 'King' and first_name = 'Steven'; 
explain

# index ์ฌ์ฉ X
select * from employees where last_name = 'King' or first_name = 'Steven'; 
```
<br>

### ์ค์ต
#### ๋ฌธ์ 
`t_emp` ํ์ด๋ธ ์์ฑ **id: int, name: varchar(200), hire_date: varchar(8)**
`employees` ํ์ด๋ธ์ ๋ฐ์ดํฐ๋ฅผ `t_emp` ์ ์ฝ์
`t_emp` hire_date index ์์ฑ
`t_emp` ์์ hire_date ๊ฐ `19930113`, `'1993013'` ์ผ๋ก ๊ฐ๊ฐ ์กฐํํ๊ณ  ์คํ๊ณํ ํ์ธ
#### ๊ฒฐ๊ณผ
- t_emp ํ์ด๋ธ ์์ฑ
```sql
create table t_emp(
	id int,
    name varchar(200),
    hire_date varchar(8)
);
```

- t_emp ์ employees ๋ฐ์ดํฐ ์ฝ์
```sql
insert into t_emp(id, name, hire_date) 
select employee_id as id, concat(first_name, ', ', last_name) as name, replace(hire_date,'-','')  from employees;
```

- hire_date index ์์ฑ
```sql
create index hire_date_idx on t_emp(hire_date);
```

- ์คํ๊ณํ
```sql
explain
select id, name, hire_date from t_emp where hire_date = 19930113;
```
![@19930113 ์ผ๋ก ์กฐํ์ type ALL | day2-work1-explain int](https://cloud.githubusercontent.com/assets/9030565/24438270/253c59bc-1481-11e7-9b03-77ea5f4065c0.PNG)

---

```sql
explain
select id, name, hire_date from t_emp where hire_date = '19930113';
```
![@'19930113' ์ผ๋ก ์กฐํ์ type ref | day2-work1-explain varchar](https://cloud.githubusercontent.com/assets/9030565/24438294/570b3652-1481-11e7-886c-9ad68f53cd03.PNG)

<br>

### Index ์ค๊ณ
#### Index ์์ฑ ์ ์ ํ ๊ฒฝ์ฐ
- ์ ์ฒด ํ์ด๋ธ์ **10 ~15%** ์กฐํ๋๋ ๊ฒฝ์ฐ
- Index๋ก๋ง ์กฐํ์. **์ปค๋ฒ๋ง index**
- Join ์ ์ฐ๊ฒฐ๋๋ ์นผ๋ผ์ธ ๊ฒฝ์ฐ
	- MySQL์ nested join ์ด๊ธฐ ๋๋ฌธ์ **์ฐ๊ฒฐ ์นผ๋ผ์ `index` ๋ ํ์์ **
<br>

#### Index ์์ฑ ์ ์ ํ์ง ์์ ๊ฒฝ์ฐ
- ๋จ์ ์ ์ฅ์ฉ table
- `Full scan` ๊ฒฝ์ฐ
- `CUD` ์ ๋น์จ์ด `R` ๋ณด๋ค ํ์ ํ ๋์ ๊ฒฝ์ฐ
<br>

#### Selectivity(์ ํ๋)
์ปฌ๋ผ ๋ด๋ถ์ ์ ์ฅ๋ ๋ฐ์ดํฐ ๊ฐ๋ค์ ์ข๋ฅ์ ์ ์ฒด ๊ฐ์ ๋น์จ
<br>

#### ๊ณ ๋ ค์ฌํญ
- select ์ **where**, **group by**, **order by** ์ ์ฐ๋ ์ปฌ๋ผ ์์ฃผ๋ก ์ ํ
- ์ ํ๋
- ๊ธธ์ด๊ฐ ์งง์ ์ปฌ๋ผ
<br>

#### ํต๊ณ ์ ๋ณด
- Index์ ํต๊ณ์ ๋ณด๋ฅผ updateํ  ํ์๊ฐ ์๋ค.
- optimize table_name ๋ช๋ น์ด ์ฌ์ฉ
	- ํ์ด๋ธ ๋ฐ์ดํฐ์ index ๋ฅผ ์ฌ๊ตฌ์กฐํ
	- ์ ์ฅ ๊ณต๊ฐ ์ค์ด๊ณ  I / O ํจ์จ ์ฆ๊ฐ
```sql
optimize table employee;
```
- **optimize** ํ๋ ๊ฒฝ์ฐ
	- ๋์ฉ๋ ๋ฐ์ดํฐ๋ฅผ `CUD` ํ ๊ฒฝ์ฐ
	- FULLTEXT index๋ฅผ `CUD` ํ ๊ฒฝ์ฐ

<br>

## Join์ ์ดํด์ ์ต์ ํ
###  Join ๊ฐ์
- ๋ ์ด์์ ํ์ด๋ธ์ ํ์ดํฐ๋ฅผ ์ฐ๊ฒฐํ์ฌ ์กฐํ
- ํ์์ฑ ๋ฐฉ๋ฒ
	- ํ์ด๋ธ์ ์ ๊ทํํ๋ฉด ๋ฐ์ดํฐ ์ค๋ณต์ด ์ต์ํ ๋์ง๋ง ๊ฐ ๋ฐ์ดํฐ๊ฐ ํฉ์ด์ง๊ฒ๋๋ค. ์ด ๋ฐ์ดํฐ๋ฅผ ๋ค์ ํฉ์น๊ธฐ ์ํด join ํ์
	- ์ฃผ๋ก `FK` ์ด์ฉ
<br>

#### Equi-Join
- ์กฐ๊ฑด์ด `=` ์ธ ๊ฒฝ์ฐ
```sql
# ์ฌ์ ์ฑ, ๋ช, ๊ธ์ฌ, ์์ฌ์ผ, ๋ถ์๋ฒํธ, ๋ถ์๋ช ๋ถ์์ฅ๋ช ์ถ๋ ฅ
select e.last_name, e.first_name, e.salary, e.salary, d.department_id, concat(m.last_name, ', ', m.first_name) as manager_name
from employees e, departments d, employees m
where e.department_id = d.department_id
and d.manager_id = m.employee_id;
```

#### Non Equi-Join

#### Outer Join
- **Left outer join**
	- `์ผ์ชฝ` ํ์ด๋ธ์์ join ๋ง์กฑ์ํค์ง ๋ชปํ๋ **null ๊ฒฝ์ฐ๋ ํฌํจ**
- **Right outer join**
	- `์ค๋ฅธ์ชฝ` ํ์ด๋ธ์์ **null ํฌํจ**
- **Full outer join**
	- ์์ชฝ ๋ชจ๋ join ๋ง์กฑ์ํค์ง ๋ชปํ๋ ๊ฒฝ์ฐ๋ ์กฐํ

```sql
# ์ฌ์ ์ฑ,๋ช,๊ธ์ฌ,์์ฌ์ผ, ๊ด์ง์ ์ฌ๋ฒ, ๊ด๋ฆฌ์ ์์ฌ์ผ ์ถ๋ ฅ
# ๊ด๋ฆฌ์ ์๋ ๊ฒฝ์ฐ ๊ด๋ฆฌ์ ์์ ์ถ๋ ฅ
select e.last_name, e.first_name, e.salary, e.hire_date, ifnull(m.employee_id, '๊ด๋ฆฌ์ ์์') as '๊ด๋ฆฌ์ ์ฌ๋ฒ', ifnull(m.hire_date, '๊ด๋ฆฌ์ ์์') as '๊ด๋ฆฌ์ ์์ฌ์ผ'
from employees e
left outer join employees m on e.manager_id = m.employee_id;
```

#### Self Join
```sql
# ์์ ์ ๊ด๋ฆฌ์๋ณด๋ค ๋ง์ ๊ธ์ฌ๋ฅผ ๋ฐ๋ ์ฌ์์ ์กฐํ
select * from employees e join employees m on e.manager_id = m.employee_id
where e.salary > m.salary;

# ์์ ์ ๊ด๋ฆฌ์๋ณด๋ค ์์ฌ์ผ์ด ๋น ๋ฅธ ์ฌ์์ ์กฐํ
select * from employees e join employees m on e.manager_id = m.employee_id
where e.hire_date < m.hire_date;
```

### ์ค์ต
#### ๋ฌธ์ 
hr.employees
๋์๋ช์ ๊ทธ ๋์์ ๋ฐฐ์น๋ ๋ถ์๋ฒํธ์ ๋ถ์๋ช๋ ํจ๊ป ์ถ๋ ฅํ์์ค.
๋จ, ๋ฐฐ์น๋ ๋ถ์๊ฐ ์๋ ๊ฒฝ์ฐ๋ ์ถ๋ ฅ
๋์๊ฐ ์์นํ ๋๋ผ๋ช๋ ํจ๊ป ์ถ๋ ฅ

#### ๊ฒฐ๊ณผ
```sql
select l.city ๋์๋ช, d.department_id ๋ถ์, d.department_name ๋ถ์๋ช, c.country_name ๋๋ผ๋ช 
from locations l
left join departments d on l.location_id = d.location_id
join countries c on l.country_id = c.country_id;
```

<br>

### Join ๋ด๋ถ ์๊ณ ๋ฆฌ์ฆ
- **Nested Loop Join**
	- ํ๋์ ํ์ด๋ธ์ ๊ธฐ์ค์ผ๋ก ์์ฐจ์ ์ผ๋ก join
- **Sort Merge Join**
	- ์ ํ์ด๋ธ์ ์ฒ๋ฆฌ ๋ฒ์๋ฅผ access ํด์ ์ ๋ ฌํ ๊ฒฐ๊ณผ๋ฅผ ํฉ์ณ join
	- ๋ฐฐ์น ์์์ ์ฌ์ฉ
- **Hash Join**
	- hash ํจ์ ์ฌ์ฉ
- **Block Nested Loop**
	- v5.6 ์ด์๋ถํฐ MySQL join ๋ฐฉ๋ฒ

`MySQL(v5.7)`์์  Nested Loop ๋ง ์ฌ์ฉ
`Oracle`์ ๋ค์ํ ์๊ณ ๋ฆฌ์ฆ ์ฌ์ฉ

<br>

### Join ์ต์ ํ ํฌ์ธํธ
- MySQL ์ `Nested Loop` ์ด๊ธฐ ๋๋ฌธ์ ๊ธฐ์ค ํ์ด๋ธ์ด ์ค์ํ๋ค
	- ๊ธฐ์ค ํ์ด๋ธ์์ ์กฐํ๋๋ ๋ฐ์ดํฐ์์ ๋ฐ๋ผ ์ฐ๊ด ํ์ด๋ธ์ ๋ฐ์ดํฐ์์ด ๊ฒฐ์ ๋๊ธฐ ๋๋ฌธ์ **๊ธฐ์ค ํ์ด๋ธ(์ผ์ชฝ)์ ๋ฐ์ดํฐ์์ ์ค์ด๋ ๊ฒ์ด ๊ด๊ฑด**
- Outer join ์ ์ง์ํ๋ค. ๊ผญ ํ์ํ ๊ฒฝ์ฐ๋ง ์ด๋ค.
- join ์ ์กฐํฉ ๊ฒฝ์ฐ์ ์๋ฅผ ์ค์ด๊ธฐ ์ํด **๋ณตํฉ ์นผ๋ผ index**๋ฅผ ์ฌ์ฉํ๋ค

<br>

### ์ค์ต
#### ๋ฌธ์ 
100004 ~ 100014 ์ฌ๋ฒ์ ์ฑ, ์ด๋ฆ, ์์ฌ์ผ, ํ์ฌ๊ธ์ฌ ๋ฐ ํ์ฌ์ง๊ธ์ ์ถ๋ ฅ
employee ํ์ด๋ธ์ ๋จผ์  ์กฐํ
๊ด๋ จ ์ ์ฝ์กฐ๊ฑด ์์ฑ
ํ์์ ๋ฐ๋ผ index ์์ฑ
์คํ๊ณํ ์ ์ถ

#### ๊ฒฐ๊ณผ
```sql
use myhr;
#explain
select straight_join e.emp_no, e.last_name, e.first_name, e.hire_date, es.salary, et.title
from employee e 
left join emp_salary es using(emp_no)
left join emp_title et using(emp_no)
where e.emp_no between 100004 and 100014
and es.to_date = (select max(to_date) from emp_salary where emp_no = e.emp_no)
and et.to_date = (select max(to_date) from emp_salary where emp_no = e.emp_no);
```
- where ์ ์ `'9999-01-01'` ๋ก ๋น๊ตํ๋ฉด ํด์ฌ์๋ ๋์ค์ง ์๋๋ค. ํด์ฌ์๋ to_date ๊ฐ  '9999-01-01' ๊ฐ ์๋๊ธฐ ๋๋ฌธ์
- ๋ฌธ์ ์์ ํด์ฌ์์ ๋ํ ์ค๋ช ์์ด 100004 ~ 100014 ์ ๋ํ ๊ฒฐ๊ณผ๋ฅผ ์๊ตฌํ๊ธฐ ๋๋ฌธ์ ๋ค์๊ณผ ๊ฐ์ด ์์ฑํ๋ค.


![@์คํ ๊ณํ| day2-work2-explain](https://cloud.githubusercontent.com/assets/9030565/24445310/74c31f34-14a4-11e7-9b23-459612ab18e6.PNG)

<br>

## ์๋ธ ์ฟผ๋ฆฌ ํ๋ ๋ฐ ์ต์ ํ
### ์๋ธ ์ฟผ๋ฆฌ - ๊ธฐ๋ณธ ๊ฐ๋
- **Inline View**
	- `FROM` ์ ์ ์ฌ์ฉ๋ ์ฟผ๋ฆฌ
- **Nested** ์๋ธ ์ฟผ๋ฆฌ
	- `WHERE` ์ ์ ์ฌ์ฉ๋ ์ฟผ๋ฆฌ
- **Correlated** ์๋ธ ์ฟผ๋ฆฌ(์ํธ์ฐ๊ด)
	- ์๋ธ ์ฟผ๋ฆฌ์์ ๋ฉ์ธ ์ฟผ๋ฆฌ ์ฐธ์กฐ
- **Scalar** ์๋ธ ์ฟผ๋ฆฌ
	- ํ๋์ ๊ฐ๋ง์ ์ถ๋ ฅํ๋ ์๋ธ ์ฟผ๋ฆฌ

<br>

**day3**

### ์๋ธ ์ฟผ๋ฆฌ ์ต์ ํ
#### ์ธ๋ผ์ธ ๋ทฐ ์๋ธ ์ฟผ๋ฆฌ
- Inline view ๋ก ์ต์ํ ํ join ํ๋ ๊ฒ์ด ์ข๋ค
```sql
# p52 ์ธ๋ผ์ธ ๋ทฐ์ ์ฌ์ฉ์ผ๋ก join ์ ๊ฐ์ 

# ์ผ๋ฐ join -> 1:n
#explain
select d.department_id, d.department_name, avg(e.salary) dept_salary
from employees e join departments d using(department_id)
group by e.department_id;

# ์ธ๋ผ์ธ ๋ทฐ -> 1:1
#explain
select d.department_id, d.department_name, da.dept_salary
from departments d join (
	select department_id, avg(salary) dept_salary from employees group by department_id
) da using(department_id);
```
์ฌ์ ๋ฐ์ดํฐ 8๋ง, ๋ถ์ 100 ์ด๋ผ๊ณ  ๊ฐ์ ํ๋ฉด,  **์ฒซ๋ฒ์งธ ์ฟผ๋ฆฌ**๋ `8๋ง ๋ฒ์ join` ์ด ํ์ํ๊ณ  **๋๋ฒ์งธ ์ฟผ๋ฆฌ**๋ `100 ๋ฒ์ join` ์ด ํ์ํ๋ค. ( ์ฑ ๋ด์ฉ)

**๊ทธ๋ฌ๋,** ์คํ ๊ณํ์ ์๋์ ๊ฐ์ด ๋๋ฒ์งธ ์ฟผ๋ฆฌ๊ฐ ๋ ๋ง์ ์คํ ๊ณํ์ด ํ์ํ๋ค๊ณ  ๋ณด์ฌ์ง๋ค.

![@์ฒซ๋ฒ์งธ ์ฟผ๋ฆฌ | day3-inline-1](https://cloud.githubusercontent.com/assets/9030565/24482974/0b0dd4e4-1530-11e7-95c0-843fd6cb0bf5.PNG)


![@๋๋ฒ์งธ ์ฟผ๋ฆฌ | day3-inline-2](https://cloud.githubusercontent.com/assets/9030565/24482979/15c91484-1530-11e7-81c5-f0174a646d5e.PNG)

<br>

#### ์ค์นผ๋ผ ์๋ธ ์ฟผ๋ฆฌ
- ์๋ธ ์ฟผ๋ฆฌ ๊ฒฐ๊ณผ ์งํฉ์ด **์๋**์ผ ๋ ์ฌ์ฉ
- Join ์ ๋ง์ด ํ  ๊ฒ์ผ๋ก ์์๋๋ ๊ฒฝ์ฐ ์ฌ์ฉ

#### ์ํธ ์ฐ๊ด ์๋ธ ์ฟผ๋ฆฌ
- ์๋ธ ์ฟผ๋ฆฌ ๋ด์ ๋ฉ์ธ ์ฟผ๋ฆฌ์ ์ปฌ๋ผ๋ค์ด ์ฌ์ฉ๋จ
- **๋งค๋ฒ ์๋ธ ์ฟผ๋ฆฌ๊ฐ ์ฌ์ฉ๋จ**

#### Nested ์ ์ํธ ์ฐ๊ด ๋น๊ต
``` sql
# ๋ถ์ ํ๊ท ๋ณด๋ค ๋ง์ ๊ธ์ฌ ๋ฐ๋ ์ฌ์ ์กฐํ (์ ์ฒด ์ฌ์์ 10๋ง)
use hr;
# ์ํธ ์ฐ๊ด ์ฟผ๋ฆฌ
explain
select * from employees e 
where salary > ( 
	# ์ํ 10๋ง๋ฒ. ๋์ผ ๋ฐ์ดํฐ ์ค๋ณต ์ ๊ทผ
	select avg(salary) from employees where department_id = e.department_id
);

# ์ธ๋ผ์ธ ๋ทฐ
explain
select * from employees e
join (
	# 10๋ง๊ฑด์ 1๋ฒ Full Scan
	select department_id, avg(salary) avg_salary from employees group by department_id
) da using(department_id)
where e.salary > da.avg_salary;
```

<br>

### ์๋ธ ์ฟผ๋ฆฌ์ DCL & DML
#### CREATE
- create ์ ์๋ธ ์ฟผ๋ฆฌ ์ฌ์ฉํ๋ฉด ์ด๋ฏธ ์กด์ฌํ๋ ํ์ด๋ธ์ ํ์ํ ๋ฐ์ดํฐ๋ง ๋ณต์ฌํด์ ํ์ด๋ธ ์์ฑ ๊ฐ๋ฅ
- ๋ฌด๊ฒฐ์ฑ ๊ท์น์ ๋ณต์ฌ๋์ง ์๋๋ค. `Not Null`์ ๋ณต์ฌ ๋จ

#### INSERT
- ์ด๋ฏธ ์กด์ฌํ๋ ํ์ด๋ธ์์ ํ์ํ ๋ฐ์ดํฐ๋ง ๋ณต์ฌ

#### UPDATE
- ๋ค๋ฅธ ํ์ด๋ธ์ ๊ฐ์ ๊ธฐ๋ฐ์ผ๋ก ํ์ด๋ธ ํ **๋ณ๊ฒฝ** ๊ฐ๋ฅ

#### DELETE
- ๋ค๋ฅธ ํ์ด๋ธ์ ๊ฐ์ ๊ธฐ๋ฐ์ผ๋ก ํ์ด๋ธ ํ **์ญ์ ** ๊ฐ๋ฅ

<br>

### ์๋ธ ์ฟผ๋ฆฌ ํ์ฉ
#### IN & exists
- MySQL **v5.5 ์ดํ**์์  `IN` ์ ์์ ์๋ธ ์ฟผ๋ฆฌ๋ฅผ ๋นํจ์จ์ ์ผ๋ก ์ฒ๋ฆฌํ๋ค.
	- v5.5 ์ด์ ์์  ๋ฉ์ธ ์ฟผ๋ฆฌ์์ ์กฐํ๋ ๊ฐ row ์ ๋ํด `in` ์กฐ๊ฑด ๋ด๋ถ์ ์๋ธ ์ฟผ๋ฆฌ๋ฅผ **๋งค๋ฒ ์คํ**
	- v5.6 ์ดํ์์  ์๋ธ์ฟผ๋ฆฌ๋ฅผ ๋ฉ์ธ ์ฟผ๋ฆฌ ์ด์ ์ **ํ๋ฒ๋ง ์คํ**ํ์ฌ ๋ฉ์ธ ์ฟผ๋ฆฌ์ ๋น๊ตํ๋ ๋ฐฉ์์ผ๋ก ์คํ

##### IN & exists ๋น๊ต ์ฟผ๋ฆฌ
```sql
# ๋ถ์์ฅ์ ์ ๋ณด๋ฅผ ์ถ๋ ฅ

explain
select * from employees e # exists ์ฌ์ฉ
where exists ( select 'x' from departments where manager_id = e.employee_id);

explain
select * from employees e # in ์ฌ์ฉ
where employee_id in ( select manager_id from departments);
```

###### IN ์คํ ๊ณํ
| id | select_type | table | partitions | type | possible_keys | key | key_len | ref | rows | filtered | Extra |
|----|--------------------|-------------|------------|------|---------------|------------|---------|------------------|------|----------|-------------|
| 1 | PRIMARY | e | NULL | ALL | NULL | NULL | NULL | NULL | 107 | 100.00 | Using where |
| 2 | DEPENDENT SUBQUERY | departments | NULL | ref | manager_id | manager_id | 5 | hr.e.employee_id | 2 | 100.00 | Using index |

###### exists ์คํ ๊ณํ
| id | select_type | table | partitions | type | possible_keys | key | key_len | ref | rows | filtered | Extra |
|----|-------------|-------------|------------|--------|---------------|------------|---------|---------------------------|------|----------|-------------------------------------|
| 1 | SIMPLE | departments | NULL | index | manager_id | manager_id | 5 | NULL | 27 | 44.44 | Using where; Using index; LooseScan |
| 1 | SIMPLE | e | NULL | eq_ref | PRIMARY | PRIMARY | 4 | hr.departments.manager_id | 1 | 100.00 | NULL |

<br>

#### COALESCE() & IF()
- ์๋ธ ์ฟผ๋ฆฌ ์ฌ์ฉํ  ๋, COALESCE() & IF() ํจ์๋ฅผ ์ด์ฉํ๋ฉด ํจ์จ์ ์ธ ์ฟผ๋ฆฌ ์์ฑ์ด ๊ฐ๋ฅํ๋ค.

<br>

### ์ค์ต
#### ๋ฌธ์ 
๋ถ์(department_id), ์ง๋ฌด(job_id) ๋ณ ์ง์์์ ๊ธ์ฌ ์ด์ก ์ถ๋ ฅ
- ๋ถ์๋ณ/์ง๋ฌด๋ณ,  ์ฒ์ฒด ์ด ์ง์ ์์ ๊ธ์ฌ ์ด์ก ์ถ๋ ฅ
- ๋ถ์๋ฒํธ/ ์ง๋ฌด๋ฒํธ ์ ์ ๋ ฌ null ์ ๋์ค์

<br>

## MySQL ๋์์ฑ ์ ์ด, ์๋ฒ ํ๋, ํํฐ์, SQL ํ์ฉ

### ๋์์ฑ ์ ์ด
- MySQL  ์ Lock ๊ณผ ํธ๋์ญ์์ผ๋ก ๋์์ฑ์ ์ ์ด
- **MySQL Lock ๋งค์ปค๋์ฆ**
	- ์ฐ๋ ๋๊ฐ ๋ฐ์ดํฐ ์งํฉ์ ์์ฒญํ  ๋ Lock ์ค์ 
	- ๋ฐ์ดํฐ ์งํฉ์ ํ์ด๋ธ, ํ, ํ์ด์ง, ๋ฉํ๋ฐ์ดํ 
	- ์ฐ๋ ๋๊ฐ ๋ฐ์ดํฐ ์งํฉ ์ฒ๋ฆฌ ์๋ฃํ๋ฉด Lock ํด์ 
- **MySQL ํธ๋์ญ์**
	- ์ ๋ขฐ์ฑ ์๊ฒ ์ฒ๋ฆฌ๋๋ ์ผ์ ๋จ์

### Lock
- **์ฝ๊ธฐ Lock**
	- `R` ๋ง ๊ฐ๋ฅ `CUD` ๋ถ๊ฐ
- **์ฐ๊ธฐ Lock**
	- `CRUD` ๋ถ๊ฐ

#### Lock ์ข๋ฅ
- **Table Lock**
	- **ํ์ด๋ธ ์ ์ฒด**์ Lock์ ๊ฑธ๊ธฐ ๋๋ฌธ์ ๋ค๋ฅธ ํธ๋์ญ์์ด ํด๋น ํ์ด๋ธ์ ์ ๊ทผ ๋ถ๊ฐ
- **Row Lock**
	- ํ๋ ์ด์์ **๊ฐ๋ณ์ ์ธ ํ**์ Lock
	- Lock์ด ๊ฑธ๋ฆฌ์ง ์์ ํ์ ์ ๊ทผ ๊ฐ๋ฅ
- **Page Lock**
	- `BDB` ๋ผ๋ ์คํ ๋ฆฌ์ง ์์ง์ ์ ์ฉ.
	- ๊ฑฐ์ ์ฌ์ฉ๋์ง ์์
- **Metadata Lock**
	- ์ฐ๋ ๋๊ฐ ํ์ด๋ธ์ ์ฌ์ฉํ  ๋ ํ์ด๋ธ์ ๋ชจ๋  ๋ฉํ๋ฐ์ดํฐ์ Lock
	- ๋ฉํํ์ดํฐ๋, `DDL ์ ์ํด ๋ณ๊ฒฝ๋๋ ์ ๋ณด`
	

#### ํ์ด๋ธ Lock
- `MyISAM`  ์คํ ๋ฆฌ์ง ์์ง์์ ์ฌ์ฉ
- ์คํ ๋ฆฌ์ง ์์ง๊ณผ ๋ฌด๊ดํ๊ฒ `LOCK TABLE` ๋ช๋ น์ผ๋ก Lock ๊ฐ๋ฅ
- **v5.5 ์ด์**์์ `DDL` ์คํ ์ Lock ๋ฐ์

#### ํ Lock
- ํ์ด๋ธ ์ ์ฒด๊ฐ ์๋ ์ผ๋ถ ํ์ lock
- `InnoDB` ๋ ํ lock
```sql
# ํ lock
create table my_inno_t(
	id int auto_increment primary key,
    name varchar(10)
);

insert into my_inno_t(name) values('a');
insert into my_inno_t(name) values('b');
insert into my_inno_t(name) values('c');

select * from my_inno_t;

update my_inno_t set name = sleep(200) where id = 1; # 200์ด ๊ฑธ๋ฆผ
```

๋ค๋ฅธ ์ธ์ ์ด๊ณ  
`select * from my_inno_t;` ํ๋ฉด **๊ฐ๋ฅ**
`update my_inno_t set name='aaa' where id=1;` ํ๋ฉด **๋ถ๊ฐ๋ฅ** ํ์ lock ์ด ๊ฑธ๋ ค์

<br>


### ํธ๋์ญ์
- MySQL ์ ์คํ ๋ฆฌ์ง ์์ง ๋ ๋ฒจ์ผ๋ก ํธ๋์ญ์
	- ```select @@autocommit;``` ์ผ๋ก ํธ๋์ฌ์ ์ฌ์ฉ ์ฌ๋ถ ํ์ธ
- **MySQL ํธ๋์ญ์**
	- ```start transaction;``` or ```begin;``` ์ผ๋ก ํธ๋์ญ์ ์์
	- ```commit;``` ์ผ๋ก ์ํ ์๋ฃ
	- ```rollback``` ์ผ๋ก ์ํ ์ทจ์

<br>

### ๋ฐ๋ Lock
- ๋ ๊ฐ ์ด์์ ํธ๋์ญ์์ด ๊ฐ๊ธฐ `Lock`์ ํ์ง ์๊ณ  ์๋ก์ `Lock`์ด ํ๋ฆฌ๊ธธ ๊ธฐ๋ค๋ฆฌ๋ ์ํฉ
- **ํ Lock** ์ ์ฌ์ฉํ๋ฉด ๋ฐ๋ Lock์ ํผํ  ์ ์๋ค.
- InnoDB์๋ ๋ฐ๋ Lock ํ์ง๊ธฐ๊ฐ ์๋ค.

<br>

### ์๋ฒ ํ๋
#### ์๋ฒ ํ๋์ ์ดํด
- ๋ง๋ฅ ์ํ์ ์๋ค.
- ์คํ ๋ฆฌ์ง ์์ง์ ์ ์ ํ ์ ํํ๋ ๊ฒ์ด ์ค์
- H/W OS ์ ์ํฅ์ ๋ฐ๋๋ค.
- **์ฟผ๋ฆฌ ํ๋์ด ์ต์ฐ์ **

#### ์๋ฒ ์ต์ ๋ฐฉ๋ฒ
- `my.ini` ์ค์ 
	- **C:\ProgramData\MySQL\MySQL Server 5.7**
	- **/etc/my.cnf** or **/etc/mysql/my.cnf**
- ์๋ฒ ์คํ ์ ๋ช๋ น์ด ์๋ ฅ
- ์คํ ์ค ์ธ์์ด๋ ๊ธ๋ก๋ฒ ๋ณ์ ์ค์ 


#### ์ค์  ์ ๋ณด ํ์ธ
- ```show status;```
- ```show global status;```
- ```show session status;```
- ```show status like 'Key%';```

<br>

### ์ค์  ํ์ผ
- ์๋ฒ๊ฐ ๊ธฐ๋๋  ๋ `์ค์  ํ์ผ`์ ํตํด MySQL ์๋ฒ ์ํ
- `my.ini` or `my.inf`
- ์๋ฒ ๊ธฐ๋ ์ค์๋ `SET GLOBAL` ๋ช๋ น์ผ๋ก ๋ณ๊ฒฝ ๊ฐ๋ฅ. **์๋ฒ ์ฌ์์ ํ๋ฉด ๋ฆฌ์๋จ**
	- ๋ณ๊ฒฝ ํ, cmd(๊ด๋ฆฌ์)์์ `net stop MYSQL57` `net start MYSQL57` restart ๋ช๋ น์ด ์์


#### my.ini
- general-log=0
	- ์ผ๋ฐ ๋ก๊ทธ๋ ๋จ๊ธฐ์ง ์์
- slow-query-log=1
	- ๋๋ฆฐ ์ฟผ๋ฆฌ๋ ๋ก๊ทธ ๋จ๊น
- long_query_time=10
	- ๋๋ฆฐ ์ฟผ๋ฆฌ๋ 10์ด ์ด์ ๊ฑธ๋ฆฌ๋ ์ฟผ๋ฆฌ
- **innodb_buffer_pool_size**
	- ํ์ด๋ธ, ์ธ๋ฑ์ค ๋ฑ์ ์ ์ฅํ๋ ์ํ ๊ณต๊ฐ
	- **InnoDB ์ฑ๋ฅ์ ๊ฐ์ฅ ์ค์**
	- ํ์ด๋ธ ๋ฐ์ดํฐ๊ฐ `pool` ์ `cache` ๋๋ฉด **DB I/O ์์ด** ์บ์ฑ๋ ๋ฐ์ดํฐ์ ์ ๊ทผํ  ์ ์๋ค
	- [buffer_pool_size ๊ณ์ฐ ์ฟผ๋ฆฌ](http://www.dbrnd.com/2015/11/mysql-script-to-determine-the-size-of-innodb_buffer_pool_size/)
- innodb_autoinc_lock_mode
- innodb_lock_wait_timeout

```sql
# buffer_pool_size ๊ณ์ฐ ์ฟผ๋ฆฌ
SELECT 
	CEILING(Total_InnoDB_Bytes*1.6/POWER(1024,3)) AS RIBPS 
FROM
(
	SELECT SUM(data_length+index_length) Total_InnoDB_Bytes
	FROM information_schema.tables WHERE engine='InnoDB'
) AS T;
```

<br>

### ์๋ฒ๊ฐ ์๋ต์ด ์๋ ๊ฒฝ์ฐ
```powershell
mysqladmin ping -u root -p

-> mysqld is alive
```

<br>

### ๋ก๊ทธ
- MySQL ์ ๊ธฐ๋ณธ์ ์ผ๋ก `data` ํด๋์ ๋ก๊น
	- **C:\ProgramData\MySQL\MySQL Server 5.7\Data**
- ๋ก๊ทธ ๊ธฐ๋ก์ `๋นํ์ฑํ`๊ฐ ๋ํดํธ
- ๋ก๊ทธ ์ข๋ฅ
	- ์๋ฌ
	- ์ผ๋ฐ ์ฟผ๋ฆฌ
	- ๋๋ฆฐ ์ฟผ๋ฆฌ
	- DDL

```sql
# ๋ก๊ทธ ๋จ๊ธฐ๊ธฐ ๋ํดํธ๋ ๋นํ์ฑํ(0) ์ด๊ธฐ ๋๋ฌธ์ ๋ค์๊ณผ ๊ฐ์ด ๋ณ๊ฒฝ
select @@general_log;
set global general_log=1;
```

<br>

### ํ์ด๋ธ ์ ์ง๋ณด์
#### analyze
```sql
analyze table table_name;
```

#### check
```sql
check table table_name;
```

#### checksum
- ๋ ํ์ด๋ธ์ ๋ด์ฉ์ด ์ผ์นํ๋์ง ํ์ธ
```sql
CHECKSUM TABLE table_name [, table_name ...]

create table my_emp
select * from employees;

select * from my_emp;

checksum table employees, my_emp;
```

#### optimize
- ๋๋ ๋ฐ์ดํฐ `CUD` ํ index ์ฌ๊ตฌ์ถ ๊ณผ์ 
```sql
optimize table table_name;
```

<br>

### ํํฐ์
- ๋ฌผ๋ฆฌ์ ์ผ๋ก ์ฌ๋ฌ ํ์ด๋ธ๋ก ๊ตฌ์ฑ๋๋ ํ๋์ ๋ผ๋ฆฌ์  ํ์ด๋ธ
- ์ด๋ ฅ / ๋ก๊ทธ ๋ฐ์ดํฐ ํ์ด๋ธ์ ์ฃผ๋ก ์ฌ์ฉ
- MySQL v5.0 ๋ถํฐ ์ง์

#### ํํฐ์ ์ฅ์ 
- ๋ฉ๋ชจ๋ฆฌ ์ ์ฌ์ ๋๋ฌด ํฐ ํ์ด๋ธ์ ์ฌ์ฉ
- ์ผ๋ฐ ํ์ด๋ธ๋ณด๋ค ๊ด๋ฆฌ ์ฉ์ด
- ๋ฌผ๋ฆฌ์  ๋ฐฐํฌ ์ฉ์ด, ์ฌ๋ฌ Disk ๋ถ์ฐ ๋ฐฐ์น ๊ฐ๋ฅ

#### ํํฐ์ ๋จ์ 
- `FK` ์ฌ์ฉ ๋ถ๊ฐ
- `Full Text index` ๋ถ๊ฐ
- ํ๋์ ํ์ด๋ธ์ `1024`๊ฐ ๊น์ง๋ง ์์ฑ ๊ฐ๋ฅ

#### ํํฐ์ ์ข๋ฅ
- **Range**
	- ๋ฒ์๋ก ์ง์ 
- **List**
	- ํํฐ์ key ๊ฐ์ผ๋ก key๋ค์ ๋ชฉ๋ก ์ง์ 
- **Hash**
	- Range ์ List ๋ก ๋ถํ ํ๊ธฐ ์ด๋ ค์ด ๊ฒฝ์ฐ Hash ํจ์ ์ฌ์ฉ
- **Key**
	- Hash ์ ๋์ผ key ๊ฐ ๊ฒฐ์  ๊ฐ๋ฅ
- **Sub**
	- ํํฐ์ ๋ด๋ถ์ ํ์ ํํฐ์ ์์ฑ ๊ฐ๋ฅ

#### Range ํํฐ์
- ๋ฒ์๋ก ์ง์ 
- ๋ ์ง ๊ฐ์ ๊ธฐ์ค์ผ๋ก ๋๋ ์ ์๋ ๋ฐ์ดํฐ์ ์ ํฉ
- Key ๋ก ๋ฐ์ดํฐ๋ฅผ ๊ฒ์ํ  ๋ ์ ์ฉ
```sql
create table my_member(
	first_name varchar(25) not null,
    last_name varchar(25) not null,
    email varchar(25) not null,
    joined_date date
)
partition by range columns(joined_date) (
	partition lessthan1990 values less than('1990-12-31'),
    partition p2000 values less than('2000-12-31'),
    partition p2020 values less than maxvalue
);

show table status where name='my_member';

select * from information_schema.PARTITIONS where table_name = 'my_member';
```

#### List ํํฐ์
- Range ์ ์ ์ฌ
- Range ์ ๋ค๋ฅธ์ ์ ์์๋๋ก ์ ๋ ฌ๋  ํ์๋ ์๋ค.

<br>

### ์ค์ต
#### ๋ฌธ์ 
- emp_salary๋ฅผ ํํฐ์์ผ๋ก ๋ถํ 
- ํ์ด๋ธ = partitioned_emp_salary
- from_date๋ฅผ ์กฐ์ฌํ์ฌ ํํฐ์ ์ ์ ํ๊ธฐ
- emp_salary์ ์ ์ฅ๋ ๋ฐ์ดํฐ๋ฅผ ํํฐ์์ผ๋ก ์ ์ฅ
- ๊ฐ ํํฐ์์ ์ ์ฅ๋ row ์ ํํฉ ์กฐํ
- ํํฐ์ ํํฉ ์กฐํ ์บก์ฒ
- ํต๊ณ ์ ๋ณด ์์ฑ ์บก์ฒ
- ๋ ํ์ด๋ธ์ ๋ฐ์ดํฐ ์ผ์น ์ฌ๋ถ ์ฒดํฌ

#### ๊ฒฐ๊ณผ
```sql
use myhr;

create table partitioned_emp_salary
select * from emp_salary; # 2844047 ๊ฐ

select from_date from partitioned_emp_salary order by from_date;
select DATE_FORMAT(from_date,'%Y-%m') m from partitioned_emp_salary group by m; # ์๋ณ ๋ถ๋ฅ ์ 212 ๊ฐ
select DATE_FORMAT(from_date,'%Y') y from partitioned_emp_salary group by y; # ๋๋ณ ๋ถ๋ฅ ์ 18 ๊ฐ
select count(*), DATE_FORMAT(from_date,'%Y') y from partitioned_emp_salary group by y; # ๋๋ณ ๋ถ๋ฅ ์ ๊ฐ ํ ์

select min(from_date) from partitioned_emp_salary; # 1985-01-01
select max(from_date) from partitioned_emp_salary; # 2002-08-01

# 1๋ ๋จ์ 
alter table partitioned_emp_salary partition by range(YEAR(from_date)) (
	partition p1985 values less than(1985),
    partition p1986 values less than(1986),
    partition p1987 values less than(1987),
	partition p1988 values less than(1988),
    partition p1989 values less than(1989),
    partition p1990 values less than(1990),
    partition p1991 values less than(1991),
    partition p1992 values less than(1992),
    partition p1993 values less than(1993),
    partition p1994 values less than(1994),
    partition p1995 values less than(1995),
    partition p1996 values less than(1996),
    partition p1997 values less than(1997),
    partition p1998 values less than(1998),
    partition p1999 values less than(1999),
    partition p2000 values less than(2000),
    partition p2001 values less than(2001),
    partition p2002 values less than(2002),
    partition pmax values less than maxvalue
);

select PARTITION_NAME, PARTITION_ORDINAL_POSITION, PARTITION_DESCRIPTION, TABLE_ROWS, AVG_ROW_LENGTH, DATA_LENGTH
from information_schema.PARTITIONS where table_name = 'partitioned_emp_salary'; #ํํฐ์ ์ฃผ์ ํํฉ

analyze table partitioned_emp_salary; # ํต๊ณ ์ ๋ณด

checksum table emp_salary, partitioned_emp_salary; # ์ฒดํฌ ์ฌ
```

![@ํํฐ์ ๊ฒฐ๊ณผ | day3-work1-parition-stats](https://cloud.githubusercontent.com/assets/9030565/24492157/5428337e-1565-11e7-8b65-3cc8610b415f.PNG)


### SQL ํ์ฉ
#### Auto Increment
- ํ์ด๋ธ row ๋ PK ์ ๋ฐ๋ผ ํด๋ฌ์คํฐํ ๋๋ค.
	- row ๊ฐ ์ถ๊ฐ๋  ๋๋ง๋ค ํ์ด์ง ํ์์ ์ค์ด๋ ๊ฒ์ด ์ข๋ค
	- PK๋ฅผ ์์์ ๊ฐ์ผ๋ก ํ๋ ๊ฒ์ ์ข์ง ์๋ค.
	- `int` ํ์์ด ์ ์ 
- ๋ณด์กฐ index ๋ PK ์ ํจ๊ป ์ ์ฅ
	- `index(name)` ์ `index(name, id)` ์ ๋์ผ
	- PK ๊ธธ์ด๋ ์์์๋ก ์ข๋ค.

#### INSERT ... ON DUPLICATE KEY UPDATE
- `oracle`์ `merge` ์ฒ๋ผ, **์กด์ฌํ๋ฉด update ์์ผ๋ฉด insert**
- ํด๋น ๊ธฐ๋ฅ์ application ๋จ์์ ์ํํ๋ฉด network ์์ด ์ฆ๊ฐํ๊ณ  ๋ถํ์ํ ๋ก์ง์ด ๋์ด๋๋ค.
- MySQL ์์๋  `INSERT ... ON DUPLICATE KEY UPDATE` ๋ก ํด๊ฒฐ

#### Limit & Offset
- `oracle`์ `Top N Query` ์ ์ ์ฌ 

#### COALESCE

#### CASE
```sql
# ์ฌ์์ ๊ธ์ฌ ๋ฑ๊ธ๋ณ ์ธ์ ์
select 
	case 
		when salary <= 4000 then '์ด๊ธ'
		when salary <= 7000 then '์ค๊ธ'
        when salary <= 10000 then '๊ณ ๊ธ'
        else 'ํน๊ธ'
    end sal_grade,
    count(*) ์ธ์์
from employees
group by
	case 
		when salary <= 4000 then '์ด๊ธ'
		when salary <= 7000 then '์ค๊ธ'
        when salary <= 10000 then '๊ณ ๊ธ'
        else 'ํน๊ธ'
    end
order by
	case 
		when salary <= 4000 then 1
		when salary <= 7000 then 2
        when salary <= 10000 then 3
        else 4
    end
;
```

#### ์คํ ๋ฆฌ์ง ์์ง ํน์ฑ ๊ณ ๋ ค
- MySQL ์คํ ๋ฆฌ์ง ์์ง์ ์ ์ฌ์ ์์ ์ฌ์ฉํด์ผ ํ๋ค
- ํธ๋์ญ์์ด ํ์ํ  ๋, `InnoDB`
	- `innodb-buffer-pool-size` ๊ฐ ์ฑ๋ฅ์ ํฐ ์ํฅ์ ๋ฏธ์น๋ค.
- ์์ ๋ก๊ทธ ๋ฐ์ดํฐ ์ ์ฅ์์ `Archive` ์คํ ๋ฆฌ์ง๊ฐ ์ ์ 
	- ๋ฐ์ดํฐ๊ฐ ์์ถ๋ ์ํ๋ก ์ ์ฅ
	- ์ ์ฅ๋ ๋ฐ์ดํฐ๋ `UD` ๋ถ๊ฐ
	- ํํฐ์ ์ง์
PK 
    ่~Jsธ[_  _    )               MySQL ์ฟผ๋ฆฌ ํ๋ ์ต์ ํ.mdup% d`ZMySQL ์ฟผ๋ฆฌ ํ๋ ์ต์ ํ.mdPK      w   ฦ    