# VeloxFlow
High-performance ETL pipeline for streaming large CSV files into PostgreSQL. Built with Python 3.9 and Pandas, it utilizes the native COPY FROM STDIN command for maximum throughput. Optimized for low-memory environments using smart chunking and in-memory buffering. Simple, robust, and blazingly fast. Up to 50x faster than standard to_sql. 
