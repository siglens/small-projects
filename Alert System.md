# Alert System

#### **Overview**

The Scalable Alert Notification System is designed to handle the evaluation and notification of millions of alerts in real-time, with strict guarantees to prevent evaluation lag. Alerts are defined in JSON format, containing parameters for evaluation intervals, metric queries, and conditions. The system ensures timely processing of alerts and notifications while addressing challenges like high throughput, lag detection

---

#### **Alert Format**

Alerts are stored in a database or file and follow this structure:

```{  
  "id1": {
     "interval": 1 to 60000,  // Evaluation interval in minutes
     "metric_query": "-1h:now:some_me_name:avg", // Query for metric data
     "condition_operator": ">",  // Comparison operator
     "condition_value": some_number  // Threshold for the condition
  },
  "id2": {
      ... 
  }  
}
```

---

#### **Proposed Solution**

##### **1\. Core Alert Evaluation System**

* **Parallel Processing**:  
  * Use a worker pool/threading pattern to distribute alert evaluation across multiple threads  
  * Partition alerts based on their `interval` or `id` to balance the workload.  
* **Efficient Scheduling**:  
  * Employ a scheduler with precise timing (e.g., `cron-like` scheduling) to ensure alerts are evaluated within their intended interval.  
  * Track the processing time for each alert batch to detect lag  
* **Lag Detection and Debugging**:  
  * Maintain a timestamp for each alert’s scheduled and actual execution.  
  * Log discrepancies and alert administrators when lag exceeds a configurable threshold.

##### **3\. Data Query Optimization**

* **Metric Queries**:  
  * Assume a function is already implemented for you that does the metric query. For your test purposes write a mock function `run_metric_query(...)`   that will randomly return true/false if the condition has been met with some random sleep in it to simulate real-time lags.

**.**

---

#### **Debugging and Lag Monitoring**

1. **Logging**:  
   * Log execution times for alert evaluation and notification dispatch.  
2. **Lag Detection**:  
   * Add a timestamp check before evaluating each alert to determine if it has fallen behind its schedule.  
   * Emit some error logs when lag exceeds predefined thresholds.

---

#### **Implementation Plan**

1. **Alert Scheduler**:  
   * Build a scheduler that triggers alert evaluation based on defined intervals.  
2. **Evaluation Engine**:  
   * Implement a highly parallelized engine to process metric queries and evaluate conditions.  
3. **Monitoring and Debugging**:  
   * Implement local logging for lag detection and debugging.  
4. **Testing**:  
   * Simulate high-load scenarios to validate system performance and scalability.

---

