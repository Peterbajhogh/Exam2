![image](https://github.com/Peterbajhogh/Exam2/assets/144667170/0c523430-ade4-4f44-af54-58d838c90bbe)
1 Consider the tables given in the figure above. Make an inner join between Student course and Student
2 Make the same inner join as in the previous exercise, but only select the columns accepted and student name
3 Consider the tables given in the figure above. Make an inner join between Student course, Student and Course but only select the columns accepted, student name and course name
![image](https://github.com/Peterbajhogh/Exam2/assets/144667170/0c523430-ade4-4f44-af54-58d838c90bbe)
4 Consider the tables given in the figure above. Make a view that gives the number of students applied for the different course majors (i.e. replicate the table below)
![image](https://github.com/Peterbajhogh/Exam2/assets/144667170/d34572ca-6724-4373-b06c-2b5fb2ad23df)
5 Consider the tables given in the figure above. Make a view (call the view vw students all info) that gives the number of students who applied for each course with information on department name, and course major (i.e. replicate the table below).
![image](https://github.com/Peterbajhogh/Exam2/assets/144667170/08a04ab7-c390-43f7-a47a-7bab5d9056a6)
6 A new student, Magnus, is accepted to take the course Biochemistry at Bsc in Medicine in the Medicine department. Update the basetable(s) accordingly. Run the view
vw students all info) and confirm it is updated correctly (i.e. the view should look like the table below)
![image](https://github.com/Peterbajhogh/Exam2/assets/144667170/a3bf9c12-89a7-42da-9174-2663683e2fdf)

**For the exercises on this slide, you should use the dvdrental database**
7 What is the customer id and first name of the customer with most dvd-rentals?
8 How many dvdrentals has each staff member made to the customer with customer id = 148?
9 Which customer (indicated by customer id, first name, and last name) has the highest average film length on their rented dvds?
10 Inspect the view definition of view
public.sales by film category and describe in words what this view shows. Hint: Inspect Source in the Properties pane of the view.

**Consider the dim mod Schema for the exercises on this slide**
11 What is the first and the last date of a unit sold
12 Define a sale as salesquantity · unitprice. Make a query that gives the total sale per week day over the entire period. Sort the results such that the weekday with the highest total sale is listed first in the returned table.
