![image](https://github.com/Peterbajhogh/Exam2/assets/144667170/0c523430-ade4-4f44-af54-58d838c90bbe)

1 Consider the tables given in the figure above. Make an inner join between Student course and Student
```
select *
from bi.Student as s 
inner join
bi.Student_course sc 
on s.student_number = sc.student_number;
```

2 Make the same inner join as in the previous exercise, but only select the columns accepted and student name
```
select s.student_name,
	   sc.accepted	   
from bi.Student as s 
inner join bi.Student_course sc 
on s.student_number = sc.student_number; 
```


3 Consider the tables given in the figure above. Make an inner join between Student course, Student and Course but only select the columns accepted, student name and course name
```
select s.student_name,
	   c.course_name, 
	   sc.accepted	   
from bi.Student as s 
inner join
bi.Student_course sc 
on s.student_number = sc.student_number
inner join
bi.Course c  
on sc.course_id = c.course_id; 
```

![image](https://github.com/Peterbajhogh/Exam2/assets/144667170/0c523430-ade4-4f44-af54-58d838c90bbe)

4 Consider the tables given in the figure above. Make a view that gives the number of students applied for the different course majors (i.e. replicate the table below)

![image](https://github.com/Peterbajhogh/Exam2/assets/144667170/d34572ca-6724-4373-b06c-2b5fb2ad23df)

```
create view bi.vw_students_course_major as
select count(sc.student_number) as "Students applied", c.course_major 
from bi.course c 
left join bi.student_course sc 
on c.course_id = sc.course_id 
group by c.course_major;

select * from bi.vw_students_course_major;

drop view bi.vw_students_course_major;
```
![image](https://github.com/Peterbajhogh/Exam2/assets/144667170/0c523430-ade4-4f44-af54-58d838c90bbe)

5 Consider the tables given in the figure above. Make a view (call the view vw students all info) that gives the number of students who applied for each course with information on department name, and course major (i.e. replicate the table below).

![image](https://github.com/Peterbajhogh/Exam2/assets/144667170/08a04ab7-c390-43f7-a47a-7bab5d9056a6)
```
create view bi.vw_students_all_info as
select count(s.student_number) as "Students applied", d.deparment_name, c.course_name, c.course_major   
from bi.department d
left join bi.student s  
on d.department_code  = s.department_code 
left join bi.student_course sc 
on sc.student_number = s.student_number 
left join bi.course c 
on c.course_id = sc.course_id 
group by d.deparment_name, c.course_name, c.course_major;

select * from bi.vw_students_all_info;
```


6 A new student, Magnus, is accepted to take the course Biochemistry at Bsc in Medicine in the Medicine department. Update the basetable(s) accordingly. Run the view
vw students all info) and confirm it is updated correctly (i.e. the view should look like the table below)

![image](https://github.com/Peterbajhogh/Exam2/assets/144667170/a3bf9c12-89a7-42da-9174-2663683e2fdf)

```
insert into bi.student 
	values (default, 'Magnus', 4);
insert into bi.student_course 
	values (5, 5, 'Accepted');

select * from bi.vw_students_all_info;
```

**For the exercises on this slide, you should use the dvdrental database**

7 What is the customer id and first name of the customer with most dvd-rentals?
```
select count(c.customer_id) as number_rentals, c.customer_id, c.first_name  
from public.customer c 
left join public.rental r 
on c. customer_id = r.customer_id
group by c.customer_id, c.first_name 
order by number_rentals desc; 
```

8 How many dvdrentals has each staff member made to the customer with customer id = 148?
```
select count(c.customer_id), s.first_name as "Staff firstname", s.staff_id 
from public.customer c 
left join public.rental r 
on c.customer_id = r.customer_id 
left join public.staff s 
on r.staff_id = s.staff_id 
where c.customer_id = 148
group by "Staff firstname", s.staff_id;
```

9 Which customer (indicated by customer id, first name, and last name) has the highest average film length on their rented dvds?
```
select avg(f.length) as "Average film length", c.customer_id, c.first_name, c.last_name
from public.customer c
left join public.rental r 
on c.customer_id = r.customer_id 
left join inventory i 
on r.inventory_id = i.inventory_id 
left join film f 
on i.film_id = f.film_id
group by c.customer_id, c.first_name, c.last_name
order by "Average film length" desc;
```

10 Inspect the view definition of view public.sales by film category and describe in words what this view shows. Hint: Inspect Source in the Properties pane of the view.
```
select * from public.sales_by_film_category sbfc; 
```
Shows the sales per film category. The sales is given in descending order.


**Consider the dim mod Schema for the exercises on this slide**

11 What is the first and the last date of a unit sold
```
select min(dd.Date) as min_date, max(dd.Date) as max_date 
from dim_mod.Fact_OnlineSales fos 
left join dim_mod.DimDate dd 
on fos.DateKey_FK = dd.DateNum_PK;
```
12 Define a sale as salesquantity · unitprice. Make a query that gives the total sale per week day over the entire period. Sort the results such that the weekday with the highest total sale is listed first in the returned table.
```
select sum(UnitPrice*SalesQuantity) as "Total sales per day", dayname 
from dim_mod.Fact_OnlineSales fos 
inner join dim_mod.DimDate dd 
on fos.DateKey_FK = dd.DateNum_PK 
group by DayName
order by "Total sales per day" desc;
```
