### **Select Statements**

Select the entire table 

```
Select *
from public.example_table;
```

Select specific columns 
```
Select first_name, last_name
from public.example_table;
```

Select the top rows
```
Select *
from public.example_table
limit 3;
```

Distinct values
```
select distinct first_name
from public.example_table;
```

Where values satisfy conditions

```
select first_name, last_name, weight
from public.example_table 
where   first_name = 'Anders'; 
```
```
select first_name, height, weight 
from public.example_table 
where height > 169;
```

Comparison and Logical operators (and, or)

```
select first_name, height, weight
from public.example_table
where (height > 170) and (first_name = 'Anders'); 
```

```
select first_name, height, weight
from public.example_table
where (height > 170) or (first_name = 'Anders')
```

Comparison and Logical operators (between, not)

```
select first_name, height, date_birth
from public.example_table
where date_birth between '1989-06-21' and '2001-06-22';
```

```
select first_name, height, date_birth
from public.example_table
where date_birth not between '1989-06-21' and '2001-06-22';
```

Order by values in column 

```
select first_name, date_birth
from public.example_table
order by date_birth;
```

```
select first_name, date_birth
from public.example_table
order by date_birth desc;
```

Order by values across two columns

```
select first_name, height
from public.example_table
order by first_name, height;
```

```
select first_name, height
from public.example_table
order by height, first_name;
```

Group By and find Mix/Max within groupings 

```
select first_name, max(height)
from public.example_table
group by first_name;
```

```select first_name, min(height)
from public.example_table
group by first_name;
```

Group By and taking sum within groupings 

```
select first_name, sum(height)
from public.example_table
group by first_name;
```

```
select first_name, sum(height)
from public.example_table
group by first_name, last_name;
```

Group By and Count Number of Values within groupings 

```
select first_name, count(first_name)
from public.example_table
group by first_name;
```

```
select first_name, count(height)
from public.example_table
group by first_name;
```

Having with Group By

```
select first_name, count(height)
from public.example_table
group by first_name
having count(height) > 1;
```

```
select first_name, count(height)
from public.example_table
group by first_name
having sum(height) > 200;
```

CASE WHEN, new values based on logical condition

```
select first_name, gender,
    case gender
        when '0' then 'Male'
        when '1' then 'Female'
    end as gender_new
from public.example_table;
```

```
select first_name, gender,
    case 
        when height > 175 then 'Higher'
        when height = 175 then 'Average'
        when height < 175 then 'Lower'
        else 'No Data'
    end as in_relation_to_average
from public.example_table;
```

Math across columns for each row

```
select first_name, last_name, height,
height/100 as height_meters
from public.example_table
```

```
select first_name, last_name, height, weight, 
(weight/(power(height/100, 2))) as bmi
from public.example_table;
```

Using aliases

```
select first_name as fi, last_name as la
from public.example_table;
```

```
select first_name as fi, last_name as la
from public.example_table;
```

