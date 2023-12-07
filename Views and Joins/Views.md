**NOTES**

View: View definition is stored as a database object but no data is stored in association with the view
Advantages of view
 - Can store complex queries
 - Instead of recreating queries - invoke the view
 - Handy way to restrict information available to users (e.g not displaying pay rate information to some users)
 - Hide complexity of database and only display user requested information
 - Views is automatically updated when persistent base tables is updated
______

Create View in same schema
```
create view bi.vw_Department as
select * 
from bi.Department; 
```
Create View in bi schema using table in dim_mod schema
```
create view bi.vw_dimcurrency as
select * 
from dim_mod.dimcurrency; 
```
See what is in each of the views
```
select * from bi.vw_dimcurrency;

select * from bi.vw_department;
```
Insert new value into the base table
```
insert into bi.Department 
	values (default, 'History', 'Aarhus C');
```
The view automatically runs the underlying query again
```
select * from bi.vw_department;
```
Delete the inserted value from the department table
```
delete
from bi.Department 
where department_code = 5;
```
Delete a View
```
drop view bi.vw_dimcurrency;
drop view bi.vw_Department; 
```
Give column names to view alternative I
```
create view bi.vw_Department as
select deparment_name as "Name of department" , department_location as "Location of department"
from bi.Department; 

select * from bi.vw_department vd;
drop view bi.vw_Department; 
```


Give column names to view alternative II
```
create view bi.vw_Department ("Name of department", "Location of department") as
select deparment_name, department_location 
from bi.Department; 

drop view bi.vw_Department;
```
-----------------
**VIEWS AND JOINS **
-----------------
Build query to see number of students applied - step 1
```
select *
from bi.Course c
left join bi.Student_course sc 
on sc.course_id = c.course_id; 
```
Build query to see number of students applied - step 2
```
select count(sc.student_number) as student_applied, c.course_name
from bi.Course c
left join bi.Student_course sc 
on sc.course_id = c.course_id 
group by c.course_name;
```
Put the query in a view for easy access
```
create view bi.vw_student_applied as
select count(sc.student_number) as student_applied, c.course_name
from bi.Course c
left join bi.Student_course sc 
on sc.course_id = c.course_id 
group by c.course_name; 
```
Return View
```
select *
from bi.vw_student_applied;
```
Delete view
```
drop view bi.vw_student_applied;
```
