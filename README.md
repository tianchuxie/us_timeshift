# us_timeshift
record time-shift 

# Entity Insert
0. `cd src` <br/>
   
1. Prepration, config the file route: <br/>
   if nesseary, please back-up current [entity], ec2 should do automatically;<br/>
   the following variables need to be set up before run <br/><br/>
   a. **[entityRoute] Required**JSON foramt entity files, should less than 200,000 KB, or allocate the extra memory. <br/>
   b. [mongoRoute]: default as `mongodb://localhost:27017/`<br/>
   c. [formatArray]: use future for temporary divide files<br/>
2. python EntityInsert.py


# DBX / PMS 
File Name: server\server\src\_data\ein-pmsData-parse.js <br/>
The function is called `async insertMonthlyRecord()` should be in the DBX system. 
`    

async insertMonthlyRecord() {

        fileName = "D:/X/test/ACF.HHS_SYNC_TRANS.20190830.txt";
        const lastNegativeDigit = {
            "}": 0,
            "J": 1,
            "K": 2,
            "L": 3,
            "M": 4,
            "N": 5,
            "O": 6, 
            "P": 7,
            "Q": 8,
            "R": 9
        }

        const LastPositiveDigit = {
            "{": 0, 
            "A": 1,
            "B": 2,
            "C": 3, 
            "D": 4,
            "E": 5, 
            "F": 6, 
            "G": 7,
            "H": 8,
            "I": 9
        }

        var fs = require("fs");
        var titleChar = "ACF";
        var text = fs.readFileSync(fileName, "utf-8");
        var textByLine = text.split("\n");
        var k = new Date();
        k.setMonth(7);
        k.setDate(30);
        const map3 = textByLine.map(function(row) {
            let authorizedAmt = parseInt(row.slice(62,76).replace(/^0+/, '').replace(/\D/g,'')) || 0;
            authorizedAmt = authorizedAmt * 0.1;
            if (lastNegativeDigit[row.slice(76, 77)]) {
                authorizedAmt = - 1 * (authorizedAmt + lastNegativeDigit[row.slice(76, 77)] * 0.01);
            } else if (LastPositiveDigit[row.slice(76, 77)]){
                authorizedAmt = authorizedAmt + LastPositiveDigit[row.slice(76, 77)] * 0.01;
            }

            let disbursedAmt = parseInt(row.slice(77,91).replace(/^0+/, '').replace(/\D/g,'')) || 0;
            disbursedAmt = disbursedAmt * 0.1;
            if (lastNegativeDigit[row.slice(91, 92)]) {
                disbursedAmt = - 1 * (disbursedAmt + lastNegativeDigit[row.slice(91, 92)] * 0.01);
            } else if (LastPositiveDigit[row.slice(91, 92)]){
                disbursedAmt = disbursedAmt + LastPositiveDigit[row.slice(91, 92)] * 0.01;
            }

            let chargedAmt = parseInt(row.slice(92,106).replace(/^0+/, '').replace(/\D/g,'')) || 0;
            chargedAmt = chargedAmt * 0.1;
            if (lastNegativeDigit[row.slice(106, 107)]) {
                chargedAmt = - 1 * (chargedAmt + lastNegativeDigit[row.slice(106, 107)] * 0.01);
            } else if (LastPositiveDigit[row.slice(106, 107)]){
                chargedAmt = chargedAmt + LastPositiveDigit[row.slice(106, 107)] * 0.01;
            }
            var elt = {
                opDiv: titleChar,
                ein: row.slice(13, 22),
                einType: row.slice(12,13), 
                einSuffix: row.slice(22, 24), 
                docNumPrefix: row.slice(24, 25), 
                docNumCore: row.slice(24, 34), 
                docNumExtension: row.slice(34, 36), 
                docNumSuffix: row.slice(37, 44),
                awardStartDate: row.slice(120,128), 
                awardEndDate: row.slice(128,136), 
                authorizedAmt: authorizedAmt.toFixed(2),
                disbursedAmt: disbursedAmt.toFixed(2),
                chargedAmt: chargedAmt.toFixed(2),
                fiscalYear: parseInt(row.slice(47,51)) || 0, 
                statusCode: row.slice(119,120), 
                time: k
                //time stamp
            };
            // if (elt !== "" && elt.einType === "1" && (elt.statusCode === "C" ||  elt.statusCode === 1900 + currentTime.getYear())) {
            if (elt !== "" && elt.einType === "1") {
                return elt;
            }
        });
        var map4 = this.clearArray(map3);
        await db.collection("source.pmsMont").insertMany(map4);
        console.log('successfully updated');
        return map3;
    }
  `
