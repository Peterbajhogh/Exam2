Creating trigger function for location_postcode
```
create or replace function postcode_change()  
  returns trigger   
  language PLPGSQL  
  as  
$$  
begin  
    if new.location_postcode <> old.location_postcode then

-- Update ineffective date and current indicator on old row
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
      
--- Create the trigger
create or replace trigger location_postcode_change
before update
on bi_trigger.customer_current
for each row 
execute procedure postcode_change();

--- Make updates to customer_current of Donald
update bi_trigger.customer_current
set location_postcode = 32584,
    last_update = default
where customer_durable_sk = 1;    

--- Make updates to customer_current of Victoria
update bi_trigger.customer_current
set location_postcode = 73611,
    last_update = default
where customer_durable_sk = 2;
```
