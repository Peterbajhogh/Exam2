## Update 

**Update values**
```
update bi_four.course    
set course_name = 'Programming 1.01',
	last_update = default
where course_id = 1;
```

**Update where null** 
```
update bi_four.student    
set department_code = 3,
	last_update = default
where department_code is null;
```


**Update student department_code based on department_name and student_name**

Alternative 1
```
update bi_four.student  
set department_code = d.department_code,   
	last_update = default  
from  
bi_four.department d
where  
(d.department_name = 'Medicine') and (student_name  = 'Henrik'); 
```


Alternative 2
```
update bi_four.student  
set department_code = (select d.department_code from bi_four.department d
				       where d.department_name  = 'Medicine'),   
	last_update = default  
where  
student_name  = 'Henrik'; 
```
-----------------
## Delete 

**1. Remove from table holding foreign key - to not violate fk constraint**

We always start be deleting values in the child entity (the table holding the FK).

```
delete 
from bi_four.student_course sc   
where  
sc.student_number  = 
		(select s.student_number 
		from bi_four.student s 
		where s.student_name = 'Henrik');
```


**2. Now we can remove from table holding primary key**
```
delete
from bi_four.student s
where s.student_name = 'Henrik'
```
-----------------
## Trigger

**Create function to be used in trigger**
```
create or replace function log_department_changes()  
  returns trigger   
  language PLPGSQL  
  as  
$$  
begin  
    if new.department_name <> old.department_name then
```

**Update ineffective date and current indicator on old row**
```
         update bi_four.department_hist    
		 set ineffective_date = now(),
		 	 current_indicator = '0'
         where department_hist_sk = (select max(department_hist_sk) 
		                             from bi_four.department_hist  dh 
                                	 where (dh.department_code_durable = old.department_code));
```


**Insert new values in new row**
```
    	 insert into bi_four.department_hist(department_hist_sk, department_code_durable,
         							 department_name, department_location, 
         							 effective_date, ineffective_date, current_indicator)  
         values(default, old.department_code, new.department_name, 
                old.department_location, now(), '3022-10-30 23:19:23', '1');           
    end if;  
return new;  
end;  
$$ 
```

**Create trigger**
```
create or replace trigger department_name_changes 
 before update  
 on bi_four.department  
 for each row  
 execute procedure log_department_changes();  
```


**Make update to name of a department**
```
update bi_four.department    
set department_name = 'Clinical Medicine',
	last_update = default
where department_code = 4;

update bi_four.department    
set department_name = 'Law and Justice',
	last_update = default
where department_code = 3; 

```
