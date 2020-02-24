# us_timeshift
record time-shift 

# Entity Insert
0. `cd src` <br/>
   
1. Prepration, config the file route: <br/>
   if nesseary, please back-up current [entity], ec2 should do automatically;
   the following variables need to be set up before run
   a. **[entityRoute] Required**JSON foramt entity files, should less than 200,000 KB, or allocate the extra memory. 
   b. [mongoRoute]: default as "mongodb://localhost:27017/"
   c. [formatArray]: use future for temporary divide files
2. python EntityInsert.py
