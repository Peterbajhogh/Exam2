## Like 
**Upper case vs. lower case. match beginning of string**

% placed last, indicate we are matching on the beginning of the string.

<img width="519" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/fa2dceae-b1d3-4cce-9aa7-85c5a8fa4049">

```
select *  
from bi_five.training t  
where t.text like 'Whi%';  
```
```
select *  
from bi_five.training t  
where t.text like 'whi%';
```

**Match end of string**

<img width="510" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/ab2d173b-afbb-47c2-9b4a-feaedebe0cd6">

```
select *  
from bi_five.training t  
where t.text like '%Dashwood';  
```

**Match anywhere in the string**

```
select *  
from bi_five.training t  
where t.text like '%Dashwood%';
```

**Match on any one character. Beginning of string**

<img width="484" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/2e39983d-1b15-4825-9f46-3899aa249497">

```
select *  
from bi_five.training t  
where t.text like '_s%';
```


**Match on any one character. End of string**

```
select *  
from bi_five.training t  
where t.text like '%_s';
```


**Use escape character to match e.g. % and _**

<img width="489" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/a6f457c1-63a0-4fd1-af66-9cf0bb2705a4">

Match %

```
select *  
from bi_five.training t  
where t.text like '%\%';
```

Match _
```
select *  
from bi_five.training t  
where t.text like '\_%';
```

**Match /**

<img width="342" alt="image" src="https://github.com/Peterbajhogh/Exam2/assets/144667170/f78ff86f-585c-44a9-a6cd-7d9f8fccfeba">

```
select *  
from bi_five.training t  
where t.text like '%\\%';
```
____

## Indexes

if you want to delete schema: ```drop schema bi cascade;```

Recreating bi schema: ```create schema bi;```

Create a table

```
create table bi.department (
	department_code serial primary key,
	department_name varchar(255),
	department_location varchar(255)
);
```

Insert into table

```
insert into bi.Department
	values (default, 'Computer Science', 'Aarhus C')
		  ,(default, 'Economics and Business Economics', 'Aarhus V')
		  ,(default, 'Law', 'Aarhus C');
```


**Create index**

```
create index idx_department_name   
on bi.department(department_name);
```

Drop index ```drop index bi.idx_department_name;``

Testing on id column - already indexes as it is primary key

```
explain analyze select *  
from bi_five.training t  
where t.text_sk > 132 and t.text_sk < 52000
order by t.text_sk;
```

```
explain analyze select *  
from bi_five.training_indexes ti
where ti.text_sk > 132 and ti.text_sk < 52000
order by ti.text_sk;
```

Testing on text column

```
explain analyze select *  
from bi_five.training t  
order by t.text;
```

explain analyze select *  
from bi_five.training_indexes ti
order by ti.text;
