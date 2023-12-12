## 1 Create the schema.
```
--- SCD type 7

drop schema if exists bi_trigger cascade;

create schema bi_trigger;


--- Create customer_history table
create table bi_trigger.customer_history (
	customer_sk serial primary key,
	customer_durable_sk integer, 
	effective_date date,
	ineffective_date date,
	current_indicator boolean,
	firstname varchar(50),
	location_postcode varchar(20)
);

insert into bi.customer_history 
values (default, 1, '2023-02-01', '2023-10-09', '0', 'Donald', 98052),
       (default, 2, '2023-10-10', '9999-10-10', '1', 'Victoria', 46908),
       (default, 1, '2023-11-09', '9999-11-09', '1', 'Donald', 67133);


      
--- Create dimproduct table
create table bi_trigger.products (
	product_sk serial primary key,
	name text,
	color varchar(50)
);

insert into bi.products 
values (default, 'HL Road Frame - Black, 58', 'Black'),
       (default, 'AWC Logo Cap', 'Multi'),
       (default, 'Long-Sleeve Logo Jersey, L', 'Multi'),
       (default, 'Road-150, 52', 'Red')


---- Create customer_current table
create table bi_trigger.customer_current (
	customer_durable_sk serial primary key,
	firstname varchar(50),
	location_postcode varchar(50),
	last_update date default current_date
);

insert into bi.customer_current
values (1, 'Donald', 67133, '2023-11-09'),
       (2, 'Victoria', 46908, '2023-10-10');


---- Create fact table
create table bi_trigger.fact_sale (
	customer_sk integer,
	customer_durable_sk integer,
	product_sk integer,
	date_sk integer,
	sales_quantity integer,
	unit_price decimal(10,3),
	constraint primary_key_fact primary key (customer_sk, customer_durable_sk, product_sk, date_sk),
	constraint customer_sk_fk foreign key (customer_sk) references bi_trigger.customer_history(customer_sk),
	constraint customer_durable_sk_fk2 foreign key (customer_durable_sk) references bi_trigger.customer_current(customer_durable_sk),
	constraint products_sk_fk foreign key (product_sk) references bi_trigger.products(product_sk)
);

insert into bi.fact_sale 
values (1, 1, 2, 20230202, 1, 349),
       (2, 2, 3, 20231810, 1, 20),
       (3, 1, 4, 20231509, 1, 503);
```

## 2 Create a trigger function and the trigger 

**Creating the trigger function for location_postcode**
```
create or replace function postcode_change()  
  returns trigger   
  language PLPGSQL  
  as  
$$  
begin  
    if new.location_postcode <> old.location_postcode then


    	 --- Update ineffective date and current indicator on old row
         update bi_trigger.customer_history    
		 set ineffective_date = current_date,
		 	 current_indicator = '0'
         where customer_sk = (select max(customer_sk) 
		                             from bi_trigger.customer_history  ch 
                                	 where (ch.customer_durable_sk = old.customer_durable_sk));


    	 --- Insert new values in new row
    	 insert into bi_trigger.customer_history(customer_sk, customer_durable_sk,
         							 effective_date, ineffective_date, 
         							 current_indicator, firstname, location_postcode)  
         values(default, old.customer_durable_sk, current_date, '9999-01-01', '1', 
                old.firstname, new.location_postcode);           
    end if;  
return new;  
end;  
$$
```

**Creating the trigger**

```
create or replace trigger location_postcode_change
before update
on bi_trigger.customer_current
for each row 
execute procedure postcode_change();
```
## 3 Make updates to customer_current table 

Make updates to customer_current of Donald
```
update bi_trigger.customer_current
set location_postcode = 32584,
    last_update = default
where customer_durable_sk = 1;
```

Make updates to customer_current of Victoria
```
update bi_trigger.customer_current
set location_postcode = 73611,
    last_update = default
where customer_durable_sk = 2;
```


