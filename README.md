🚀 Bulk Assessment Simulation Tool
📌 Project Title – Bulk Assessment Simulation Framework
📖 Project Overview
This project implements a bulk assessment simulation framework to emulate many candidates attempting an online test concurrently. It reproduces realistic candidate workflows (login → start test → fetch questions → answer loop → submit) while collecting request-level metrics such as latency, status codes, and errors. The outcome is a repeatable setup (mock server + async simulator + analyzer) that produces CSV/JSON logs and charts to identify bottlenecks and guide performance improvements.

🏗️ Architecture
- FastAPI → Local mock API for safe testing
- aiohttp → Asynchronous HTTP client for concurrent traffic
- asyncio → Task scheduling and concurrency control
- pandas & numpy → Aggregation of logs and latency percentiles
- matplotlib → Latency distribution charts
- Docker (optional) → Containerized runs for reproducibility
Flow:
Candidate → FastAPI Mock API → aiohttp Requests → Telemetry Logs → Analyzer → CSV/JSON/Charts

📂 Project Structure
bulk-assessment-sim/
│── mock_server.py     
│── simulation.py      
│── analyze.py         
│── requirements.txt
│── README.md
│── outputs/           
🛠️ Technologies Used
- Python 3.10+
- FastAPI
- aiohttp
- asyncio
- pandas, numpy
- matplotlib
- Docker (optional)
- Uvicorn

🔄 Workflow
- Candidate logs in (POST /login)
- Candidate starts test (POST /tests/{test_id}/start)
- Candidate fetches questions (GET /tests/{test_id}/questions)
- Candidate answers questions (POST /tests/{test_id}/answer)
- Candidate submits test (POST /tests/{test_id}/submit)
- Telemetry collected → latency, status codes, errors
- Analyzer computes p50/p95/p99 latencies and generates charts

▶️ How to Run (Step-by-Step)
1. Create & Activate Virtual Environment
python -m venv venv
# Windows PowerShell
.\venv\Scripts\Activate.ps1
# Linux/Mac
source venv/bin/activate


2. Install Dependencies
pip install -r requirements.txt


3. Start Mock Server
python mock_server.py
# or
uvicorn mock_server:app --host 127.0.0.1 --port 8000


4. Configure Load Profile
Parameters:
- --users → total simulated candidates
- --concurrency → parallel sessions
- --questions → questions per candidate
- --think-mean, --think-jitter → human-like delays
- --output-prefix → output file prefix
5. Execute Simulator
python simulation.py \
  --base-url http://127.0.0.1:8000 \
  --users 200 \
  --concurrency 50 \
  --questions 10 \
  --think-mean 1.5 \
  --think-jitter 0.6 \
  --output-prefix sim_run1


6. Verify Output Files
- sim_run1_results.csv
- sim_run1_results.json
7. Analyze Results
python analyze.py --csv sim_run1_results.csv --out-prefix sim_run1


Outputs:
- Summary stats (success/error rate)
- Latency percentiles (p50/p95/p99)
- sim_run1_latencies.png

🧪 Testing
- Validation → Ensures session flow correctness and log integrity
- Count-based checks → Requests = users × (login + start + Q answers + submit)
- Error logging → Includes status codes and error text for debugging

📊 Example Output
- Total Requests: 2400
- Success Rate: 99.25%
- Error Rate: 0.75%
- Latency (p95):
- Login → 240 ms
- Start Test → 480 ms
- Answer → 120 ms
- Submit → 320 ms

⚠️ Notes for Production
- Monitor CPU/network to avoid simulator bottlenecks
- Use distributed runners (Locust/k6) for larger scale
- Containerize with Docker for reproducibility
- Add authentication, rate limiting, and input validation

📌 Conclusion
This tool provides a repeatable framework for stress-testing assessment platforms with concurrent candidate-like sessions. It generates structured logs and analytics that enable identification of slow endpoints, tail latency issues, and error patterns — supporting data-driven performance optimization and scaling decisions.
