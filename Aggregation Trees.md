# Aggregation Trees

Computing aggregation statistics on raw data is expensive as it involves visiting every record. However if you use n-ary trees then it becomes super fast to compute statistics. Your task is to implement such a tree on incoming json data and expose a function that will compute statistics by reading these trees.

## Input

You will be given records in json format as follows:  
```
{  
    {"fname": “sam”, "bnum": "batch-1", "os": “iOS”,   "pr": 23},  
    {"fname": “john”, "bnum": "batch-2", "os": “iOS”,   "pr": 14},  
    {"fname": “sam”, "bnum": "batch-2", "os": “win”,   "pr": 15},  
    {"fname": “sam”, "bnum": "batch-1", "os": “linux”, "pr": 22},  
}
```

## Output

1. Build a tree like below as shown below  
2. The “pr” column is a numeric/measure column whose value are stored at each node based on which path you take  
3. Expose a function `computeGroupby(groupByColumnName)` where `groupByColumnName`  is any of  `fname, bnum, os`   
4. The expected value for each of the above groupby column is shown in below diagram.

## Aggregation Tree
[aggtree-img.pdf](https://github.com/user-attachments/files/18402090/aggtree-img.pdf)
