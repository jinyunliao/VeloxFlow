🚀 High-Performance PostgreSQL Data Ingestor
A streamlined, production-ready Python utility designed to migrate massive CSV datasets into PostgreSQL at scale. By bypassing standard INSERT statements in favor of the PostgreSQL Native COPY protocol, this tool achieves ingestion speeds up to 50x faster than traditional methods.

⚡ The Performance Gap
Standard libraries like pandas.to_sql often rely on individual or batched INSERT statements, which incur heavy overhead due to SQL parsing and transaction logging.

This implementation utilizes psycopg2's copy_expert combined with memory-mapped StringIO buffers to stream data directly into the database engine.

Method,Performance,Scaling
df.to_sql(),🐌 Slow,Poor (CPU bound)
df.to_sql(method='multi'),🚀 Moderate,Average
COPY FROM STDIN (This Repo),🔥 Blazing Fast,Excellent (I/O bound)

✨ Key Features
Memory Efficient: Processes data in configurable chunks (e.g., 10k rows) to ensure low RAM overhead even with 10GB+ files.

Zero Temporary Files: Streams data through an in-memory io.StringIO buffer—no need to write intermediate CSVs to disk.

Encoding Safe: Uses Tab-separated (\t) streaming to prevent common delimiter collisions found in standard CSVs.

Transaction Safe: Built-in commit/rollback logic ensures your database stays consistent if a specific chunk fails.

🛠️ Architecture
Plaintext

Local CSV ──▶ Pandas (Chunked) ──▶ In-Memory Buffer (TSV) ──▶ Postgres COPY Protocol
🚀 Quick Start
1. Requirements
Python 3.9 or higher

A running PostgreSQL instance

2. Installation
pip install pandas sqlalchemy psycopg2-binary

3. Usage
Configure your credentials in the script and run:
```python
from ingestor import FastIngestor

# Example configuration
config = {
    'file_path': 'large_dataset.csv',
    'table_name': 'users_analytics',
    'chunk_size': 20000
}

# Run the ingestion
FastIngestor.run(config)
```

⚙️ Configuration Parameters

Variable,Description,Default
CHUNK_SIZE,Number of rows processed per database transaction.,10000
DB_URI,SQLAlchemy formatted connection string.,postgresql://...
SEP,Internal stream delimiter (Tab recommended).,\t

⚠️ Requirements for Success
To maintain high performance, this tool makes a few "Zen of Python" assumptions:

Pre-existing Schema: The target table must be created in PostgreSQL before running the script.

Column Alignment: The order of columns in your CSV (or your processed DataFrame) must strictly match the order of columns in the database table.

Data Types: Ensure date and numeric formats are PostgreSQL-compatible during the Pandas processing phase.

📝 License
Distributed under the MIT License. See LICENSE for more information.
