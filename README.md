Auto-Batcher

This file takes in the first 248 items from the SCMS Delivery History dataset located on Kaggle as orders. Regardless of the number of items requested, it breaks the order down to be picked by a batch size that does not exceed 3000 cases per batch. Up to 7 batches can be picked based on the case size and item size. This file uses multiple vlookups but the main formula(s) use index matching to group the items as well as arrays containing the items, their case quantities and their totals. The formulas change slightly per batch.

Formulas:

• =Q4:OFFSET(Q4,CELL("contents",U1)-1,1,1)

• =CONCATENATE("Case Count: ",SUM(T4:T1000))

• =IFERROR(INDEX($A$4:$N$1000,SMALL(IF((INDEX(MID($A$4:$N$1000,1,2),,11)<>"NN")(INDEX($A$4:$N$1000,,13)>=100)(INDEX($A$4:$N$1000,,13)<>""),MATCH(ROW($A$4:$N$1000),ROW($A$4:$N$1000)),""),ROWS($A$4:A4)),COLUMNS($A$1:A1)),"")

• =IF(INDIRECT(CELL("address",OFFSET(INDEX(S:S,MAX((S:S<>"")(ROW(S:S)))),1,-2))):OFFSET(INDIRECT(CELL("address",OFFSET(INDEX(S:S,MAX((S:S<>"")(ROW(S:S)))),1,-2))),CELL("contents",X1)-1,1,1)<>LOOKUP(2,1/(ISNUMBER(S4:S1000)),V4:V1000),INDIRECT(CELL("address",OFFSET(INDEX(S:S,MAX((S:S<>"")(ROW(S:S)))),1,-2))):OFFSET(INDIRECT(CELL("address",OFFSET(INDEX(S:S,MAX((S:S<>"")(ROW(S:S)))),1,-2))),CELL("contents",X1)-1,1,1),"Duplicate")
