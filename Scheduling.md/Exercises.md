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


