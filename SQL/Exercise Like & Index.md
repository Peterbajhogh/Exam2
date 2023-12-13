1 Create tables according to the
create tables for update delete triggers.sql file. Update the department table such that Computer Science is changed to Computer and Data Science.
2 Update the department code of the student Pia such that she is associated with the Computer and Data Science department.
3 Delete the student Pia from the student table.
4 Create a history table, of the SCD2 type, which is related to the course table. Then create a trigger such that this history table automatically gets updated whenever the course name in the course table gets updated. Test that the trigger works correctly (i.e. check that the history table gets updated).
5 Return all rows in the bi five.training table where rows in the
text column contains ”Henry”.
6 Return all rows in the bi five.training table where rows in the text column has the second last character being ”p”.
7 Compare the query time between bi five.turkers and
bi five.turkers indexes when ordering on the phone column.
8 Compare the query time between bi five.training and
bi five.training indexes when ordering on the line and chapter columns simultaneously. Use point-and-click in DBeaver to figure out which columns hold the index.
9 Do a left join with bi five.training being the left table and bi five.turkers being the right table. Also, do a left join with bi five.training indexes being the left table and
bi five.turkers indexes being the right table. In both cases, after the join is completed you should sort on the phone column. Can you generally say that one operation is faster than another? Do we still use an index after the join operation?
10 Create a small table with an id column and a text column with some random text. Create an index on the text column. Verify that the index is created by point-and-click in DBeaver.
