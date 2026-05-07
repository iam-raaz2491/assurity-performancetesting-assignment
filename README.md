\# Performance Testing Assignment - Assurity NZ



\## 1. Objective



The objective of this performance testing assignment is to validate both functional correctness and performance behaviour of the Category Details API under a defined workload model.



The test ensures:

\- Automated verification of HTTP protocol standards.

\- Programmatic validation of the `CanRelist` business logic.

\- Efficient parsing and flattening of nested JSON structures.

\- Quantifying system behavior against defined Service Level Agreements (SLAs).

\- Implementation of a headless (Non-GUI) execution strategy.



\---



\## 2. Tooling



\- Performance Tool: Apache JMeter 5.6.2  

\- Scripting Language: Groovy (JSR223 PostProcessor)  

\- Execution Mode: Non-GUI (CLI supported)  

\- API Endpoint:  

&#x20; https://api.tmsandbox.co.nz/v1/Categories/{id}/Details.json  



\---



\## 3. Workspace Architecture

```text

├── data/

│   └── categoryIDs.csv        # Parameterized input data

├── test-plan/

│   └── assurity-test.jmx      # Compiled JMeter Test Plan

├── results/                   # Test execution artifacts

│   └── results.csv            # Extracted structured data output

├── README.md                  # Technical Documentation

├── .gitignore                 # Artifact exclusion rules

└── .gitattributes             # Repository configurations







\---



\## 4. Functional Requirements Covered



For each API response:



\- Validate HTTP Status = 200

\- Validate `CategoryId` matches request

\- Validate `CanRelist = true`

\- Extract:

&#x20; - CategoryId

&#x20; - Name

&#x20; - Path

&#x20; - Promotions (Id, Price)

\- Write extracted data into CSV format



\---



\## 5. Non-Functional Requirements (NFR)



\- Total Category IDs: 10  

\- VUsers (Threads): 5 (half of category IDs)  

\- Ramp-up: 1 user per second  

\- Steady State: 10 API calls per minute  

\- Test Duration: 1 minute steady load  

\- Performance SLA: 90% of requests ≤ 500ms  



\---



\## 6. Execution Instructions



\### Step 1: Clone Repository



git clone https://github.com/iam-raaz2491/assurity-performancetesting-assignment.git

cd assurity-performancetesting-assignment

### Step 2: Run Test in Non-GUI Mode



jmeter -n -t test-plan/assurity-test.jmx -l test-plan/results/test-results.jtl



Parameters:



\-n → Non-GUI mode execution

\-t → Test plan file

\-l → Raw results file (.jtl)



\## 7. Reporting \& Analytics

To transform raw binary data into a visual dashboard:


jmeter -g test-plan/results/test-results.jtl -o test-plan/results/html-report


## 8. Test Data



Input file:

data/categoryIDs.csv



Contains 10 Category IDs used for API execution.



\## 9. Test Approach



CSV Data Set Config for parameterization

Thread Group for workload simulation

HTTP Request sampler for API calls

JSR223 PostProcessor (Groovy) for:

&#x09;JSON parsing

&#x09;Validation logic

&#x09;CSV output generation



\## 10. Observations



API responses were stable across all categories

CanRelist validation passed successfully

Promotions varied per category

No failures or timeouts observed

Response structure remained consistent

Performance remained stable under load



\## 11. Output Generated



CSV format:



CategoryID,Name,Path,PromotionID,Price



Each promotion is written as a separate row.



\## 12. Raw Test Results (Evidence)



During non-GUI execution, the following file is generated:



test-plan/results/test-results.jtl



This file contains:



Response times

Success/failure status

Latency metrics

Throughput data



\## 13. Assumptions



Scope: The API is assumed to be publicly reachable without IP whitelisting.



Security: No bearer tokens or API keys were required for this specific sandbox endpoint.



Environment: Results reflect local network conditions and may vary if executed from different geographic regions.



Persistence: The sandbox environment is assumed to provide idempotent responses for the duration of the test.



\## 14. Limitations



No distributed load testing

No APM or monitoring integration

Execution performed on local machine only

No cloud-based load simulation



\## 15. Conclusion



The API demonstrates stable functional and performance behaviour under the defined workload model.



It successfully:



Handles concurrent requests

Returns consistent structured data

Meets performance expectations

Supports non-GUI execution



The solution is reproducible, maintainable, and aligned with performance engineering best practices.

