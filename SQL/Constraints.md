### Data integrity
To ensure data integrity we can provide integrity constraints
We will consider the following constraints: **check, primary key, not null, unique, and foreign key as well as the serial property related to primary keys.**
___
#### Check 
Check: Values must satisfy a Boolean (true/false) constraint.
In the example below:

Name of constraint = positive_price check
Boolean constraint = (price >= 0) 

```
create table bi.sale (
    sale_id integer,
    sale_person_id integer,
    price numeric constraint positive_price check (price >= 0) 
);
```

#### Primary Key
Primary key: Must be unique and cannot contain null values.
```
create table bi.Student (
	student_id serial primary key,
	student_name varchar(255),
	department_code int);

insert into bi.Student
	values (default, 'Birgit', 2)
		  ,(default, 'Anders', 1)
		  ,(default, 'Pia', 3)
		  ,(default, 'Henrik', 3);
```

#### Serial 
Serial: When a new row is inserted, a new value is automatically inserted in the column specified with serial (typically increments with 1).

Uniqueness of values must be enforced with a unique or primary key constraint.
```
create table bi.Student (
	student_id serial primary key,
	student_name varchar(255),
	department_code int);

insert into bi.Student
	values (default, 'Birgit', 2)
		  ,(default, 'Anders', 1)
		  ,(default, 'Pia', 3)
		  ,(default, 'Henrik', 3);
```

#### Composite Primary Key
A composite primary key consist of multiple columns, and rows are uniquely identified across the columns holding the primary key.

Name of primary constraint = PK_student_course.

Primary Key is held by two columns = Student_nummber, course_id 
```
create table bi.Student_course(
	student_number int,
	course_id int,
	accepted varchar(50),
	constraint PK_student_course primary key (student_number, course_id)
	);

insert into bi.student_course
	values (1, 2, 'Accepted')
		  ,(1, 3, 'Not accepted')
		  ,(2, 1, 'Accepted')
		  ,(3, 4, 'Accepted')
		  ,(3, 3, 'Awaiting')
		  ,(4, 2, 'Not accepted');
```

#### Not Null
- Null indicates that a value is absent (not the same as zero)
- By default columns accept null values
- By using a not null constraint, a column will not accept null values
- For example a table containing personal identification numbers and names might have not null constraints on both columns

#### Unique - Alternate Key
- The column holding an alternate key uniquely identifies instances in an entity, but is not chosen as the primary key
- We can create an alternate key by creating a unique constraint

The column with the unique constraint = password
```
create table bi.app_logins (
	employee_ID serial primary key,
	username varchar(100) not null,
	password varchar(100) not null,
	constraint AK_password unique(password));

insert into bi.app_logins
	values (default, 'Simon', 'sms123#dd')
          ,(default, 'Mette', 'myunbreakablepassword123')
          ,(default, 'Christian', 'mysecretpassword11$');
```

#### Foreign Key
- If we have referential integrity, then the relations between tables are meaningful 
- Foreign keys are used to maintain referential integrity
- A Foreign key is a referential constraint - it is concerned with how data in one table relates to another table
- Foreign Key: Any value in the column that holds the foreign key must exist in the column that the foreign key points to (references).
- The foreign key must point to (reference) either a primary or a candidate key

Table and column that the foreign key is pointing to = bi.student(student_id).

The foreign key column in the created table = student_id.

The table 'bi.student_contact' hold the FK and the table 'bi.student' hold the PK. 
```
 create table bi.student_contact (
	student_id int primary key,
	City varchar(100),
	Phone varchar(100),
	constraint FK_student foreign key (student_id)
	references bi.Student(student_id));


insert into bi.student_contact
   values (2,'Aarhus', '61818192')
       	  ,(3,'Vejle', '51729824')
	      ,(1,'Aarhus N', '31838102')
	      ,(4,'Aarhus V', '55018190'); 
```
Foreign Key and Referential Integrity: 

we cannot insert the value of 5 (fx) in the student_id column of the student_contact table, because all the values of the column holding the FK does not exist in the referenced column. 

We can also delete a row, but it must not break referential integrity:
```
delete from bi.student_contact
where student_id = 1;
```
The above is possible because all the values of the column holding the foreign key, still excist in the references column (holding the PK). 







