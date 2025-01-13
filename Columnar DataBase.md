# Columnar DataBase

Implement a columnar database.The database runs as a HTTP server that receives each record as a json with random key-value pairs. For each key create a separate file that will store the successive values in TLV (Type-Length-Value) format. In a record if an existing key is missing then use BACKFILL as the type. Once any of the files reaches the MaxSegFileSize limit of 1 GB then rename it to cname.v0.tlv (e.g.: c1.v0.tlv) and start a new set of files.

## Input

PUT /elastic/\_bulk  
{  
    {"c1": 123, "c2": "txt1", "c3": 1.23, "c4": "txt2", "c5": 12},  
    {"c1": 456, "c2": "txt3", "c3": 4.56, "c4": "txt4", "c5": 34},  
    {"c1": 789, "c2": "txt5", "c3": 7.89, "c4": "txt6", "c5": 56},  
    { "c2": "txt7", "c3": 1.01, "c4": "txt8", "c5": 78},  
    {"c1": 121, "c2": "txt9", "c3": 1.21, "c5": 90},  
    {"c1": 314, "c2": "txt11", "c3": 3.14, "c4": "txt12"}  
}

## Modules:

## Output:

A dir with following files created  
c1.tlv, c2.tlv, c3.tlv, c4.tlv, c5.tlv,  ….cN.tlv 

c1.tlv  
11123  
12456  
12789  
1???  
11121  
12314

## Testing:

Use this tool to send the data to your columnar DB:  
[https://github.com/siglens/siglens/tree/develop/tools/sigclient\#es-bulk](https://github.com/siglens/siglens/tree/develop/tools/sigclient#es-bulk) OR you can use your own tool or pick some from the web.  
If you pick the above tool, it will send data in following format:

PUT /elastic/\_bulk  
{  
    "{"index": {"\_index": "myidx", "\_type": "\_doc"}}, \#\# Action Line  
    {"c1": 123, "c2": "txt1", "c3": 1.23, "c4": "txt2", "c5": 12},  
    "{"index": {"\_index": "myidx", "\_type": "\_doc"}},  \#\# Action Line  
    {"c1": 456, "c2": "txt3", "c3": 4.56, "c4": "txt4", "c5": 34},  
    "{"index": {"\_index": "myidx", "\_type": "\_doc"}},  \#\# Action Line  
    {"c1": 789, "c2": "txt5", "c3": 7.89, "c4": "txt6", "c5": 56},  
    "{"index": {"\_index": "myidx", "\_type": "\_doc"}},  \#\# Action Line  
    {"c1": 101, "c2": "txt7", "c3": 1.01, "c4": "txt8", "c5": 78},  
    "{"index": {"\_index": "myidx", "\_type": "\_doc"}}, \#\# Action Line  
    {"c1": 121, "c2": "txt9", "c3": 1.21, "c4": "txt10", "c5": 90},  
    "{"index": {"\_index": "myidx", "\_type": "\_doc"}},  \#\# Action Line  
    {"c1": 314, "c2": "txt11", "c3": 3.14, "c4": "txt12", "c5": 112}  
}  
The Action line can be ignored in your columnar DB