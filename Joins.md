**NOTES**
- Always use aliases 
- Always be aware of what table is on the left and right side
  - In below 'left join' example, we have 'left.table' on the left table, so we keep all information from this table as we are doing a left join
  - If we did a right join, we would keep everything from 'right.table'
````
select *
from bi.left_table lt 
left join bi.right_table rt 
on lt.ID = rt.ID;
````
- If we do a 'inner join' on ID, then only rows where we can match ID in two different tables will be kept 
-----------------
**JOINS**
-----------------
Inner join
````
select *
from bi.left_table lt 
inner join bi.right_table rt 
on lt.ID = rt.ID;
````
Inner join
````
select * 
from bi.Student as s 
inner join
bi.Department as d 
on s.department_code = d.department_code;
````

Inner join - only show some columns
````
select s.student_name, d.deparment_name, d.department_location 
from bi.Student as s 
inner join
bi.Department as d 
on s.department_code = d.department_code;
````

Inner join - using aliases for the column names
````
select s.student_name as student, 
	   d.deparment_name as department, 
	   d.department_location as location
from bi.Student as s 
inner join
bi.Department as d 
on s.department_code = d.department_code;
````
Inner join - multiple tables
````
select s.student_name as student,
	   d.deparment_name as department, 
	   s.student_number,
	   sc.course_id
from bi.Student as s 
inner join
bi.Department as d
on s.department_code = d.department_code
inner join 
bi.Student_course sc 
on s.student_number = sc.student_number;
````

-----------------
**LEFT AND RIGHT JOINS**
-----------------

Left join
````
select *
from bi.left_table lt 
left join bi.right_table rt 
on lt.ID = rt.ID;
````

Left join - department and students tables
````
select d.department_code, d.deparment_name, s.student_name  
from bi.Department d 
left join bi.Student s 
on d.department_code = s.department_code;
````

Right join
````
select *
from bi.left_table lt 
right join bi.right_table rt 
on lt.ID = rt.ID;
````

Right join - department and students tables
````
select d.department_code, d.deparment_name, s.student_name  
from bi.Department d 
right join bi.Student s 
on d.department_code = s.department_code;
````
-----------------
**FULL JOIN**
-----------------
Full Join
````
select *
from bi.left_table lt 
full join bi.right_table rt 
on lt.ID = rt.ID;
````

Full Join - department and students tables
````
select s.student_name, d.deparment_name, d.department_location 
from bi.Department_1 d 
full join bi.Student_1 s 
on d.department_code = s.department_code;
````
