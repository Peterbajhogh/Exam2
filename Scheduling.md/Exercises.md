## From Exam Assignment 2

**Start by making the script we want to schedule**

To get the script running, we have to select what script we want to run. We use the ```increment.one.R```as template. 
We have to get the variable "date_time", so we start by running this inside the ```increment_one.R``` 
```
date_time <- format(Sys.time(), digits = 0)
```

After this, we edit the ```increment.one.R```file so it fits the current folder and the output we want etc.
```
# Check if "increment_one.rds" exists
if(file.exists("/home/rstudio/exam/increment_one.rds")){

  # If "increment_one.rds exists then we read it into memory
  increment_one <- readRDS(file = "/home/rstudio/exam/increment_one.rds")

  # The R object is saved to the disk
  saveRDS(increment_one, file = "/home/rstudio/exam/increment_one.rds")

  # We print the datetime and the value of increment_one.
  # This will be captured by the cronR logger and written to the .log file
  print(paste0(" Process complete at: ", date_time))
```

The final output for the file we wish to schedule is therefor:
```
# Get the current datetime
date_time <- format(Sys.time(), digits = 0)
 
# Check if "increment_one.rds" exists
if(file.exists("/home/rstudio/exam/increment_one.rds")){

  # If "increment_one.rds exists then we read it into memory
  increment_one <- readRDS(file = "/home/rstudio/exam/increment_one.rds")

  # The R object is saved to the disk
  saveRDS(increment_one, file = "/home/rstudio/exam/increment_one.rds")

  # We print the datetime and the value of increment_one.
  # This will be captured by the cronR logger and written to the .log file
  print(paste0(" Process complete at: ", date_time))
```

**Then open seperate rscript to run the file we just made**

Assuming the name of our file is ```increment.one.R```


```
library(cronR)

# We will execute the increment_one.R script in the following cron jobs
cmd <- cron_rscript(rscript = "increment_one.R")

# Assuming the current day is Saturday
cron_add(cmd, frequency = 'minutely', id = 'job7')
```

## From lectures
```
library(cronR)
# We will execute the increment_one.R script in the following cron jobs
cmd <- cron_rscript(rscript = "increment_one.R")
```

10. Schedule the script “increment_one.R” to execute every minute for this current weekday (i.e., for the weekday you are currently at).
```
# Assuming the current day is Saturday
cron_add(cmd, frequency = 'minutely', id = 'job7', days_of_week = c(6))
```

11. Schedule the script “increment_one.R” to execute at 04:00 but only on the 15th of each month.
```
cron_add(cmd, frequency = 'daily', id = 'job8', days_of_month = c(15),
         at = "04:00")
```

12. Save the active jobs in a crontab file called “my_BI_schedule”. Then remove all active jobs. Open the “increment_one.log” and inspect its content. Load all the cronR jobs saved in the “my_BI_schedule” file, and verify that they are active. Finally, remove all active jobs.
```
cron_save(file = "my_BI_schedule")
cron_clear()
cron_load(file = "my_BI_schedule")
cron_ls()
cron_clear()
```


