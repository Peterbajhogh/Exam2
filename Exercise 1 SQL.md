**1 Return the first 10 rows of the Customer table (include all columns)**
```
select *
from customer
limit 10;
```

**2 Return the distinct postal codes from the address table**
```
select distinct postal_code
from address;
```

**3 Return the title of the films (from the film table) with a length above 90 minutes.**
```
select title
from film
where length > 90;
```

**4 Return the title, length, and rental rate of all films which has a length below 90 minutes and a rental rate above 4 dollars.**
```
select title, length, rental_rate
from film
where length < 90 and rental_rate > 4;
```

**5 Return the payment id, the payment date, and the amount (from the payment table) where the payment date falls in the interval [19 February 2007 at 7 p.m.; 20 February 2007 at 7 p.m.].**
```
select payment_id, payment_date, amount
from payment
where payment_date between '2007-02-19 19:00:00' and '2007-02-20 19:00:00';
```

**6 You can use brackets, (), to nest logical operators. Make the same query as in exercise 5, but only return rows where the amount is above 7 dollars.**
```
select payment_id, payment_date, amount
from payment
where (payment_date between '2007-02-19 19:00:00' and '2007-02-20 19:00:00') and amount > 7;
```

**7 Make two queries that return the rental table. The first query returns the rows sorted with respect to the rental date column such that the first rental date is in the first row. The second query is similar to the first but returns the opposite sorting of the rental date column.**
```
select * 
from rental
order by rental_date;
```
```
select * 
from rental
order by rental_date desc;
```

**8 Explain in words what the following query does:**
select customer_id, amount, payment_id
   from payment
   order by customer_id, amount;

First order in terms of customer_id, and then for each uniqe value of customer_id, payment_id is ordered. Both ordering is ascending by default.
   
**9 Using the payment table: Write a query that gets the number of payments for each customer. The returned table should have the customer id and the number of payments as columns and only show the 10 rows which have the highest number of payments.**
```
select customer_id, count(payment_id) as n_pay_customer
from payment
group by customer_id
order by n_pay_customer desc 
limit 10;
```

**10 Using the payment table: Write a query that gets the number of payments for each customer. The query should only return customer id where the number of payments is between 15 and 17. Use the <= and >= comparison operators in your query.**
```
select customer_id, count(payment_id) as n_pay_customer
from payment
group by customer_id
having count(payment_id) >= 15 and count(payment_id) <= 17;
```

**11 Compute the average value of the amount column in the payment table.**
```
select avg(amount)
from payment;
```

**12 Assume the average amount in the payment table is approximately 4.2. Write a query that returns a table with a column that indicates whether a payment is either ’At the average’, ’Above average’, ’Below average’, or if there is ’no data available’.**
```
select amount,  
	case
		when amount > 4.2 then 'Above average'
		when amount = 4.2 then 'At the average'
		when amount < 4.2 then 'Below the average'
		else 'No data available'
	end as in_relation_to_average
from payment;
```

**13 The query given below can be written shorter (and easier to read). Rewrite the query below into a new and shorter query that returns the same result.**
select sum(a.after_vat)
from (select amount*0.8 as after_vat
         from payment) as a;
```
select sum(amount*0.8) as after_vat
from payment;
```  
         
**14 The following query counts the number of distinct postal codes from the address table**
select count(distinct postal_code)
from address;
write an alternative (and longer) query that returns the same output, but uses aliases. This code should first get a table of distinct values of postal codes, and secondly, use that table to count the number of rows. Use the previous exercise as inspiration.
```
select count(a.postal_code)
from (select distinct postal_code
	  from address) as a;
```

**15 Create your own table in one of your personal databases. This table should have an autoincrementing id column, an integer column, a text column where each cell can have a maximum of 200 characters, a numeric column that allows a total 10 digits and 3 digits after the decimal point, a column that contains dates and times, and a boolean column. After the table is created, insert two rows of data. Run the insert statement two times and check that your id column increments.**

Create table in public schema (you must be connected to your own database to create a table)
```
create table public.my_shopping_list (
	id serial,
	item varchar(200),
	quantity integer,
	price decimal(10, 3),
	expected_buy_date timestamp,
	is_bought boolean
```
Insert values into the table (rowwise insertion)
```
insert into public.my_shopping_list(item, quantity, price, expected_buy_date, is_bought) 
	values ('Banana', '5', 3.5, '2022-10-05', '0')
           ,('House', '1', 3500000.532, '2030-01-15', '0');
```



