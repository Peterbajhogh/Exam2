1 Create tables according to the
create tables for update delete triggers.sql file. Update the department table such that Computer Science is changed to Computer and Data Science.

**Solution**
```
update bi_four.department    
set department_name = 'Computer and Data Science'
where department_code = 1;
```

2 Update the department code of the student Pia such that she is associated with the Computer and Data Science department.

**Solution**

```
update bi_four.student  
set department_code = (select d.department_code from bi_four.department d
				       where d.department_name  = 'Computer and Data Science')
where  
student_name  = 'Pia';
```

3 Delete the student Pia from the student table.

**Solution**
```
delete 
from bi_four.student_course sc   
where  
sc.student_number  = 
		(select s.student_number 
		from bi_four.student s 
		where s.student_name = 'Pia');  

delete
from bi_four.student s
where s.student_name = 'Pia'
```



4 Create a history table, of the SCD2 type, which is related to the course table. Then create a trigger such that this history table automatically gets updated whenever the course name in the course table gets updated. Test that the trigger works correctly (i.e. check that the history table gets updated).

**Solution**

Create table 
```
create table bi_four.course_hist(
	course_hist_sk serial primary key,
	course_code_durable integer, 
	course_name varchar(250),
	course_major varchar(250),
	effective_date timestamp,
	ineffective_date timestamp,
	current_indicator boolean,
	constraint fk_course_hist foreign key (course_code_durable)
	references bi_four.course (course_id));

insert into bi_four.course_hist 
	VALUES (default, 1, 'Programming I', 'BSc in Computer Science', current_timestamp(0), '3022-10-30 23:19:23', '1')
		  ,(default, 2, 'Principles of Economics', 'BSc in Economics', current_timestamp(0), '3022-10-30 23:19:23', '1')
		  ,(default, 3, 'Distributed systems', 'BSc in Computer Science', current_timestamp(0), '3022-10-30 23:19:23', '1')
		  ,(default, 4, 'Animal Law', 'Bsc in Law', current_timestamp(0), '3022-10-30 23:19:23', '1')
		  ,(default, 5, 'Biochemistry', 'Bsc in Medicine', current_timestamp(0), '3022-10-30 23:19:23', '1');
```

Create function to be used in trigger
```
create or replace function log_course_changes()  
  returns trigger   
  language PLPGSQL  
  as  
$$  
begin  
    if new.course_name <> old.course_name then  
    	 --- Update ineffective date and current indicator on old row
         update bi_four.course_hist    
		 set ineffective_date = now(),
		 	 current_indicator = '0'
         where course_hist_sk = (select max(course_hist_sk) 
		                             from bi_four.course_hist  ch 
                                	 where (ch.course_code_durable = old.course_id));
    	 --- Insert new values in new row
    	 insert into bi_four.course_hist(course_hist_sk, course_code_durable,
         							 course_name, course_major, 
         							 effective_date, ineffective_date, current_indicator)  
         values(default, old.course_id, new.course_name, 
                old.course_major, now(), '3022-10-30 23:19:23', '1');           
    end if;  
return new;  
end;  
$$
```

Create trigger
```
create or replace trigger log_course_changes
 before update  
 on bi_four.course  
 for each row  
 execute procedure log_course_changes();  
 ```

Make an update
```
update bi_four.course    
set course_name= 'Programming 1.01'
where course_id = 1;
```


5 Return all rows in the bi five.training table where rows in the
text column contains ”Henry”.

**Solution**
```
select *  
from bi_five.training t  
where t.text like '%Henry%'; 
```

6 Return all rows in the bi five.training table where rows in the text column has the second last character being ”p”.

**Solution**

```
select *  
from bi_five.training t  
where t.text like '%p_';
```

7 Compare the query time between bi five.turkers and
bi five.turkers indexes when ordering on the phone column.

**Solution**

```
explain analyze select *  
from bi_five.turkers t
order by t.phone;

explain analyze select *  
from bi_five.turkers_indexes ti
order by ti.phone;
```

8 Compare the query time between bi five.training and
bi five.training indexes when ordering on the line and chapter columns simultaneously. Use point-and-click in DBeaver to figure out which columns hold the index.

**Solution**
```
explain analyze select *  
from bi_five.training t  
order by t.line, t.chapter;

explain analyze select *  
from bi_five.training_indexes ti
order by ti.line, ti.chapter;
```

9 Do a left join with bi five.training being the left table and bi five.turkers being the right table. Also, do a left join with bi five.training indexes being the left table and
bi five.turkers indexes being the right table. In both cases, after the join is completed you should sort on the phone column. Can you generally say that one operation is faster than another? Do we still use an index after the join operation?


**Solution**

After the join operation the index on the phone column is no longer used.
We cannot say that one query is in general faster than another.
```
explain analyze select *
from bi_five.training t
left join bi_five.turkers ts 
on t.turk_fk = ts.turk_pk
order by ts.phone;

explain analyze select *
from bi_five.training_indexes ti 
left join bi_five.turkers_indexes ti2 
on ti.turk_fk = ti2.turk_pk
order by ti2.phone;
```



10 Create a small table with an id column and a text column with some random text. Create an index on the text column. Verify that the index is created by point-and-click in DBeaver.

**Solution**

```
create table bi_four.Department_index (
	department_code serial primary key,
	department_name varchar(255),
	department_location varchar(255),
	last_update timestamp(0) without time zone default current_timestamp(0)
);

insert into bi_four.Department_index
	values (default, 'Computer Science', 'Aarhus C')
		  ,(default, 'Economics and Business Economics', 'Aarhus V')
		  ,(default, 'Law', 'Aarhus C')
		  ,(default, 'Medicine', 'Aarhus C');
		 
create index idx_department_name   
ON bi_four.department_index(department_name);
```



