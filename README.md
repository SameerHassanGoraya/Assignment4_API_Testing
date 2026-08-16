# Assignment 4 – API Testing (Postman + JMeter)

**Intern:** Sameer Hassan
**Program:** 10Pearls Shine Internship – QA (Manual + Automation)
**API Under Test:** [JSONPlaceholder](https://jsonplaceholder.typicode.com/) – free fake REST API for testing

## 1. Overview

This project covers functional API testing with **Postman** (GET, POST, PUT, DELETE against JSONPlaceholder, with automated test scripts) and basic performance/load testing with **Apache JMeter** (10 virtual users, 5s ramp-up, 5 loops on `GET /posts/1`).

## 2. Files in this submission

| File | Purpose |
|---|---|
| `JSONPlaceholder_API_Testing.postman_collection.json` | Postman collection – all API requests + test scripts |
| `JSONPlaceholder_Environment.postman_environment.json` | Postman environment – defines `base_url` variable |
| `JSONPlaceholder_LoadTest.jmx` | JMeter test plan for load testing |
| `README.md` | This file |
| `screenshots/` | Postman test run results and JMeter Summary Report / View Results Tree screenshots (add your own after running) |

## 3. Postman Collection Structure

```
JSONPlaceholder API Testing - 10Pearls Assignment 4
├── 1. GET Requests
│   ├── Get All Posts            → GET  {{base_url}}/posts
│   └── Get Single Post (ID 1)   → GET  {{base_url}}/posts/1
├── 2. POST Requests
│   └── Create New Post          → POST {{base_url}}/posts
├── 3. PUT Requests
│   └── Update Existing Post     → PUT  {{base_url}}/posts/1
└── 4. DELETE Requests
    └── Delete Post (ID 1)       → DELETE {{base_url}}/posts/1
```

Each request includes automated tests (Postman `Tests` tab) covering four categories, as required by the assignment:

1. **Status code validation** – e.g. `pm.response.to.have.status(200)`
2. **Response time validation** – e.g. `pm.expect(pm.response.responseTime).to.be.below(500)`
3. **Response structure validation** – e.g. checking the response is an array / has the expected keys
4. **Field & value validation** – e.g. checking `title`, `body`, `userId`, `id` values match expectations

## 4. How to run the Postman tests

1. Open Postman (download from [postman.com/downloads](https://www.postman.com/downloads/) if not installed).
2. Click **Import** → select both:
   - `JSONPlaceholder_API_Testing.postman_collection.json`
   - `JSONPlaceholder_Environment.postman_environment.json`
3. In the top-right environment dropdown, select **"JSONPlaceholder Environment"**.
4. Run requests individually (click **Send** on each), or run the whole collection:
   - Click the collection name → **Run** → **Run JSONPlaceholder API Testing**.
   - Postman's Collection Runner will execute all requests in order and show a pass/fail summary for every test assertion.
5. Take a screenshot of the Collection Runner results (all tests passing) for submission.

**Note on JSONPlaceholder behavior:** This is a mock/fake API. POST, PUT, and DELETE requests are simulated by the server — it returns realistic responses (e.g., a newly generated `id` on POST) but does not actually persist changes. This is expected and documented behavior of JSONPlaceholder, and the test scripts are written to validate the response format/values it is designed to return.

## 5. JMeter Load Test

**Test Plan:** `JSONPlaceholder_LoadTest.jmx`

**Configuration:**
| Setting | Value |
|---|---|
| Target endpoint | `GET https://jsonplaceholder.typicode.com/posts/1` |
| Threads (Users) | 10 |
| Ramp-Up Period | 5 seconds |
| Loop Count | 5 |
| Total requests | 10 × 5 = 50 |
| Assertions | Response Code = 200, Duration < 2000ms |
| Listeners | View Results Tree, Summary Report |

### How to run

1. Install Apache JMeter from [jmeter.apache.org](https://jmeter.apache.org/download_jmeter.cgi) (requires Java 8+).
2. Launch JMeter:
   - Windows: run `bin\jmeter.bat`
   - macOS/Linux: run `bin/jmeter.sh`
3. Open the test plan: **File → Open** → select `JSONPlaceholder_LoadTest.jmx`.
4. Click the green **Start** (▶) button to run the test.
5. View results:
   - **View Results Tree** – inspect individual request/response pairs, confirm all are green (pass).
   - **Summary Report** – review aggregate metrics (see below).
6. Take screenshots of both listeners after the run completes and save them in a `screenshots/` folder for submission.

### Metrics to analyze (from Summary Report)

| Metric | What it tells you |
|---|---|
| **Average / Min / Max response time** | How fast the API responds under load |
| **Throughput** | Requests handled per second/minute |
| **Error %** | Percentage of failed requests (should be 0% for a healthy public API under this light load) |
| **Std. Dev.** | Consistency of response times across requests |

For this assignment's load (10 users, 50 total requests against a lightweight public mock API), expect **0% error rate** and consistent response times, since JSONPlaceholder is a lightweight, CDN-backed service. Document your actual observed numbers in the screenshot/report you submit, since real-world results vary by network and time of day.

## 6. Summary of what this assignment demonstrates

- Structuring a Postman collection into logical, named folders/requests
- Writing automated assertions across status code, timing, structure, and data
- Covering all 4 core HTTP methods (GET, POST, PUT, DELETE) against a REST API
- Setting up a basic JMeter performance test with a Thread Group and HTTP Sampler
- Interpreting load test results (response time, throughput, error rate) via listeners

## 7. Submission checklist

- [x] Postman Collection (`.json`)
- [x] Postman Environment (`.json`)
- [x] JMeter Test Plan (`.jmx`)
- [x] Screenshots of Postman Collection Runner results *(add after running locally)*
- [x] Screenshots of JMeter View Results Tree & Summary Report *(add after running locally)*
- [x] README (this file)
