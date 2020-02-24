# us_timeshift
record time-shift 

# Entity Insert
0. `cd src` <br/>
   
1. Prepration, config the file route: <br/>
   if nesseary, please back-up current [entity], ec2 should do automatically;<br/>
   the following variables need to be set up before run<br/>
   a. **[entityRoute] Required**JSON foramt entity files, should less than 200,000 KB, or allocate the extra memory. <br/>
   b. [mongoRoute]: default as `mongodb://localhost:27017/`<br/>
   c. [formatArray]: use future for temporary divide files<br/>
2. python EntityInsert.py
