# ☁️ Local Serverless Data Pipeline (Colab Edition)

![Pipeline Demo](https://media.tenor.com/LCJMufd4bDkAAAAi/satu-data-animation.gif)

A fully working **serverless-style data pipeline** built entirely with **Python**, designed to run right inside **Google Colab** - no AWS account, no setup, and no edits required.


It behaves like an **AWS S3 → Lambda → DynamoDB → S3 (processed)** workflow but runs locally using folders and JSON files to emulate the cloud.

-----------

## 🚀 Overview

Whenever you drop a file (like `data.txt`) into the **`inbox/`** folder:
1. A “Lambda ingestion” function is triggered automatically 🧩  
2. Metadata about the file (like name, timestamp, and status) is stored in **`data/metadata.jsonl`** 🗂️  
3. A “Lambda transform” function reads that file, converts its text to uppercase, and saves it in **`processed/`** 💾  

All this happens in real time inside Colab — no dependencies, no cloud costs, no setup hassles.

---

## 🧱 Architecture

┌─────────────┐
│ inbox/ │ ← "S3 Bucket" for uploads
└──────┬──────┘
│
(event detected)
│
▼
┌────────────────────┐
│ ingestion_handler │ → writes metadata
└──────┬─────────────┘
│
▼
┌────────────────────┐
│ data/metadata.jsonl│ ← "DynamoDB Table"
└──────┬─────────────┘
│
▼
┌────────────────────┐
│ transform_handler │ → uppercases content
└──────┬─────────────┘
│
▼
┌─────────────┐
│ processed/ │ ← "Processed output bucket"
└─────────────┘

yaml
Copy code

---

## 🧰 Tech Stack

| Component | Technology Used |
|------------|-----------------|
| Language | Python 3 (no dependencies) |
| Cloud Concept | AWS Lambda, S3, DynamoDB |
| Runtime | Google Colab or local Python |
| Data Storage | Local JSON Lines file |
| Triggering | Polling-based event detection |

---

## 📁 Folder Structure

📦 project-root
┣ 📂 inbox/ # Incoming "uploads"
┣ 📂 processed/ # Output folder
┣ 📂 data/ # Stores metadata file
┣ 📜 main.py # Main watcher script
┗ 📜 README.md

markdown
Copy code

---

## 🧪 Quickstart (Colab-Friendly)

### 1️⃣ Open Google Colab
- Create a new notebook.
- Copy and paste the full Python code from the main script into a cell.
- Run the cell — it starts the watcher automatically.

### 2️⃣ Upload a File
In Colab’s **left sidebar → Files tab**:
- Find or create the folder `inbox/`
- Upload any `.txt` file (for example `test.txt`)

The pipeline detects the upload instantly and:
- Creates `processed/test.txt` with uppercased content
- Logs metadata to `data/metadata.jsonl`

You’ll see output like:

✅ Processed 'test.txt' at 14:32:19

yaml
Copy code

---

## 🧾 Example Metadata (data/metadata.jsonl)

Each line is a JSON record:
```json
{"fileKey": "test.txt", "bucket": "inbox", "status": "RECEIVED", "ingestedAt": "2025-10-16T14:32:19.123Z"}
This acts like a DynamoDB table — one record per processed file.

⚙️ Configuration (Environment Variables)
Variable	Default	Description
INBOX_DIR	inbox	Folder for incoming files
PROCESSED_DIR	processed	Folder for transformed output
DATA_DIR	data	Folder where metadata lives
METADATA_FILE	data/metadata.jsonl	Metadata log file
POLL_INTERVAL	1.5	Seconds between folder checks

You don’t need to set any of these in Colab — defaults are perfect for testing.

🧭 How to Stop the Pipeline
In Colab, just click Stop (■) in the toolbar — it halts the watch loop safely.

🧹 Cleanup
When you’re done, remove generated folders:

bash
Copy code
!rm -rf inbox processed data
💡 Extend This Project
Here are some cool ways to make it more realistic or advanced:

🪣 Replace the folder-based system with real S3 uploads using boto3

 Add a data validation step before writing metadata

📈 Stream metrics to CloudWatch or Grafana

🔄 Convert the watcher into a Flask API or FastAPI endpoint

☁️ Deploy the same logic to AWS Lambda and test with actual S3 triggers

🎥 Demo GIF (Placeholder)
Replace this with your own demo GIF when you record the pipeline running:


🪪 License
MIT License © 2025 — free to use, modify, and share.

❤️ Author Notes
This Colab edition is made to teach cloud concepts without AWS setup.
It’s a great base to practice serverless thinking, event-driven design, and data engineering logic — all inside your browser.

yaml
Copy code
