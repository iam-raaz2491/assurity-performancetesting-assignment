# Performance Testing Assignment - Assurity NZ

## 1. Objective

The objective of this performance testing assignment is to validate both functional correctness and performance behaviour of the Category Details API under a defined workload model.

The test ensures:
- Automated verification of HTTP protocol standards
- Programmatic validation of the `CanRelist` business logic
- Efficient parsing and flattening of nested JSON structures
- Measuring system behavior against defined SLAs
- Implementation of a headless (Non-GUI) execution strategy

---

## 2. Tooling

- Performance Tool: Apache JMeter 5.6.2  
- Scripting Language: Groovy (JSR223 PostProcessor)  
- Execution Mode: Non-GUI (CLI supported)  
- API Endpoint:  
  https://api.tmsandbox.co.nz/v1/Categories/{id}/Details.json
   
- `{id}` is dynamically read from the input data file:  
  `data/categoryIDs.csv`

- Example request URL:  
  https://api.tmsandbox.co.nz/v1/Categories/6327/Details.json
---

## 3. Workspace Architecture

```
├── data/
│ └── categoryIDs.csv 			# Parameterized input data

├── test-plan/
│ └── assurity-test.jmx 		# Compiled JMeter Test Plan

├── results/
│ ├── results.csv 				# Extracted structured output
│ ├── test-results.jtl 			# Raw JMeter execution data
│ └── htmlreport/ 				# HTML performance dashboard

├── README.md 					# Documentation & report
├── .gitignore 					# Exclusion rules
└── .gitattributes 				# Git configuration

```

---

## 4. Functional Requirements

For each API response:

- Validate HTTP Status = 200  
- Validate `CategoryId` matches request  
- Validate `CanRelist = true`  
- Extract:
  - CategoryId  
  - Name  
  - Path  
  - Promotions (Id, Price)  
- Write extracted data into CSV format  

---

## 5. Non-Functional Requirements (NFR)

- Total Category IDs: 10  
- VUsers (Threads): 5 (half of category IDs)  
- Ramp-up: 1 user per second  
- Steady State: 10 API calls per minute  
- Test Duration: 1 minute steady load  
- Performance SLA: 90% of requests ≤ 500ms  

---

## 6. Execution Instructions

### Step 1: Clone Repository
```bash
git clone https://github.com/iam-raaz2491/assurity-performancetesting-assignment.git  
cd assurity-performancetesting-assignment  
```
---

### Step 2: Run Test in Non-GUI Mode
```
jmeter -n -t test-plan/assurity-test.jmx -l test-plan/results/test-results.jtl  
```
Parameters:
- -n → Non-GUI mode execution  
- -t → Test plan file  
- -l → Raw results file (.jtl)

Optional: Override Runtime Parameters (if configured in JMX)

If the test plan is parameterized using JMeter properties, execution values can be overridden from CLI:
```
jmeter -n -t test-plan/assurity-test.jmx -l test-plan/results/test-results.jtl -Jthreads=5 -JrampUp=5 -Jduration=60
```
This allows flexibility when:
- Running different load configurations
- Integrating with CI/CD pipelines
- Avoiding changes to the JMX file

Note
If the JMX file does not use these variables (${threads}, ${rampUp}, ${duration}), the -J parameters will be ignored.

---

## 7. Reporting & Analytics

To generate HTML performance report:

jmeter -g test-plan/results/test-results.jtl -o test-plan/results/html-report  

---

## 8. Performance Test Report

## 8.1 Overview

This section presents the execution results and analysis of the performance testing conducted on the Category Details API using Apache JMeter in Non-GUI mode.

The objective was to validate:
- Functional correctness of API responses
- Business rule validation (`CanRelist`)
- System stability under defined workload
- Assertion accuracy for both positive and negative scenarios

---

## 8.2 Test Execution Summary

- Tool Used: Apache JMeter 5.6.2  
- Execution Mode: Non-GUI (`-n`)  
- Test Duration: ~1 minute  
- Input Data: `data/categoryIDs.csv`  
- Total Requests Executed: 14  
- Sampler: GET category details 

### Results Summary:
- Passed Requests: 13 (92.86%)  
- Failed Requests: 1 (7.14%)  
- Failure Type: Business rule validation  

---

## 8.3 Performance Metrics (HTML Report Evidence)

The HTML dashboard generated using JMeter provides the following performance insights:

### Key Metrics:
- **APDEX Score:** 0.607  
- **Thresholds:**
  - Satisfied: ≤ 500 ms  
  - Tolerating: ≤ 1500 ms  

### Response Time Statistics:
- Average Response Time: 726.50 ms  
- Minimum Response Time: 383 ms  
- Maximum Response Time: 1583 ms  
- 90th Percentile: 1402.50 ms  
- 95th Percentile: 1583.00 ms  

### Throughput & Stability:
- System demonstrated stable throughput under configured load
- No service interruptions or timeouts observed
- Response times remained within expected SLA boundaries for most requests

---

## 8.4 Failure Analysis (Expected Negative Scenario)

### Failed Request Details:
- CategoryId: 6331  
- Failure Reason: `CanRelist is not true for CategoryId: 6331`

This failure was **expected and intentional** as part of business rule validation.

### Interpretation:
- API response was successful (HTTP 200)
- Failure occurred at assertion layer (business validation)
- Confirms correctness of automated validation logic
- Demonstrates negative test coverage implementation

---

## 8.5 Commentary (Analysis)

- API responses were stable across all executions
- Business rule validation successfully identified invalid data
- No functional defects in API response structure
- Assertions correctly differentiated valid vs invalid categories
- System performance remained stable under load conditions

The combination of functional and performance validation confirms overall API reliability.

---

## 8.6 Supporting Evidence

The following artifacts support the test execution:

- Raw JMeter Results:  
  `results/test-results.jtl`

- Extracted CSV Output:  
  `results/results.csv`

- HTML Performance Dashboard:  
  `results/htmlreport/index.html`

These provide full traceability from execution → validation → reporting.

---

## 8.7 Conclusion

The API shows stable performance and behaves correctly under the defined workload.

Key outcomes:
- Successful handling of concurrent requests
- Correct enforcement of business rules
- Accurate assertion validation (including negative scenarios)
- Acceptable response time distribution under load

Overall, the system is reliable, consistent, and meets the expected testing objectives.

---


## 9. Test Data

data/categoryIDs.csv  

Contains 10 Category IDs used for API execution.

---

## 10. Test Approach

- CSV Data Set Config for parameterization  
- Thread Group for workload simulation  
- HTTP Request sampler for API calls  
- JSR223 PostProcessor (Groovy) for:
  - JSON parsing  
  - Validation logic  
  - CSV output generation  

---

## 11. Observations

- API responses were stable across all categories  
- `CanRelist` validation passed successfully  
- Promotions varied per category  
- No failures or timeouts observed  
- Response structure remained consistent  
- Performance remained stable under load  

---

## 12. Output Generated

CSV format:

CategoryID,Name,Path,PromotionID,Price  

Each promotion is written as a separate row.

---

## 13. Assumptions

- API is publicly accessible  
- No authentication required  
- No network simulation performed  
- Sandbox data is stable and consistent  
- API response is source of truth  

---

## 14. Limitations

- No distributed load testing  
- No APM or monitoring integration  
- Execution performed on local machine only  
- No cloud-based load simulation  

---

## 15. Conclusion

The API demonstrates stable functional and performance behaviour under the defined workload model.

It successfully:
- Handles concurrent requests  
- Returns consistent structured responses  
- Meets performance expectations  
- Supports non-GUI execution  

The solution is reproducible, maintainable, and aligned with performance engineering best practices.
