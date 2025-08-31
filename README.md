# Apache Spark Essential Training: Big Data Engineering

This repository contains the complete exercise files and implementation examples from the LinkedIn Learning course "Apache Spark Essential Training: Big Data Engineering" instructed by Kumaran Ponnambalam.

## Overview

The course materials demonstrate production-grade streaming analytics pipelines using Apache Spark integrated with modern big data technologies including Kafka, Redis, and MariaDB. Through hands-on exercises, you'll build end-to-end data engineering solutions that handle both batch and real-time processing scenarios.

## Architecture

The exercise implementations showcase a Lambda architecture pattern combining:

- **Stream Processing**: Real-time analytics with Apache Spark Structured Streaming
- **Message Queuing**: Apache Kafka for reliable data streaming and event sourcing
- **In-Memory Storage**: Redis for low-latency analytics and caching
- **Persistent Storage**: MariaDB for historical data warehousing
- **Containerization**: Docker for reproducible deployment environments

## Prerequisites

### Software Requirements

- Python 3.11+
- Apache Spark 3.5.1
- Apache Kafka 3.6.0
- Redis Server
- MariaDB/MySQL 8.0+
- Docker (optional but recommended)

### Python Dependencies

```bash
pip install pyspark kafka-python redis mariadb
```

### Hadoop Dependencies (Windows)

The repository includes Windows-specific Hadoop utilities:
- `hadoop/bin/hadoop.dll`
- `hadoop/bin/winutils.exe`

## Exercise Structure

### 1. Environment Setup (`code_00_03_Spark_BDE_Setup_Prerequisites.ipynb`)

Initial configuration and dependency verification for the Spark ecosystem.

### 2. Batch Data Engineering (`code_03_XX_Spark_BDE_Batch_Data_Engg.ipynb`)

Foundation course covering:
- Spark DataFrame operations
- ETL pipeline construction
- Data transformation patterns

### 3. Real-time Streaming Pipeline

#### 3.1 Data Generation (`code_04_03_Spark_BDE_Generate_a_visits_stream.ipynb`)

Simulates e-commerce website activity with:
- Multi-country user sessions (USA, India, Brazil, Australia, Russia)
- User journey tracking (Catalog, FAQ, Order, ShoppingCart)
- Realistic temporal patterns with randomized durations

```python
# Sample generated record structure
{
  "country": "USA",
  "last_action": "Order", 
  "visit_date": "2024-08-12 13:01:16",
  "duration": 11
}
```

#### 3.2 Streaming Analytics (`code_04_04_Spark_BDE_Build_a_streaming_analytics_job.ipynb`)

Implements three concurrent streaming pipelines:

**Abandoned Cart Detection**
- Filters shopping cart events in real-time
- Publishes to dedicated Kafka topic for marketing automation

**Geographic Analytics**
- Maintains country-wise visit duration statistics in Redis
- Enables real-time geographic performance monitoring

**Windowed Aggregations**
- 5-second rolling windows for action-based metrics
- Persistent storage in MariaDB with watermark handling

#### 3.3 Pipeline Orchestration (`code_04_05_Spark_BDE_Execute_the_pipeline.ipynb`)

Real-time monitoring dashboard that demonstrates:
- Multi-source data consumption (Kafka, Redis, MariaDB)
- Cross-platform analytics integration
- Live performance metrics visualization

### 4. Advanced Analytics

#### 4.1 Batch-Stream Integration (`code_06_03_Spark_BDE_Extracting_long_last_actions.ipynb`)

Demonstrates Lambda architecture implementation:
- JDBC-based parallel database queries
- Distributed processing with configurable partitions
- Stream republishing for downstream analytics

#### 4.2 Performance Monitoring (`code_06_04_Spark_BDE_scorecard_for_last_action.ipynb`)

Real-time dashboard for tracking:
- Long-duration user sessions (>15 seconds)
- Action-based engagement metrics
- Redis-backed analytics scorecard

## Infrastructure Components

### Kafka Topics

- `spark.streaming.website.visits` - Primary user activity stream
- `spark.streaming.carts.abandoned` - Filtered shopping cart events
- `spark.exercise.lastaction.long` - Long-duration user actions

### Redis Keys

- `last-action-stats` - Country-wise visit duration aggregates
- `long-last-action-stats` - Long-duration action counters

### Database Schema

**MariaDB Table: `website_stats.visit_stats`**
```sql
CREATE TABLE visit_stats (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    INTERVAL_TIMESTAMP TIMESTAMP,
    LAST_ACTION VARCHAR(50),
    DURATION INT
);
```

## Quick Start

### 1. Infrastructure Setup

Start the required services using Docker:

```bash
docker-compose -f spark-docker.yml up -d
```

### 2. Database Initialization

Create the required database and user:

```sql
CREATE DATABASE website_stats;
CREATE USER 'spark'@'localhost' IDENTIFIED BY 'spark';
GRANT ALL PRIVILEGES ON website_stats.* TO 'spark'@'localhost';
```

### 3. Execute Pipeline

Run the notebooks in sequence:

1. **Setup**: `code_00_03_Spark_BDE_Setup_Prerequisites.ipynb`
2. **Data Generation**: `code_04_03_Spark_BDE_Generate_a_visits_stream.ipynb`
3. **Streaming Analytics**: `code_04_04_Spark_BDE_Build_a_streaming_analytics_job.ipynb`
4. **Monitoring**: `code_04_05_Spark_BDE_Execute_the_pipeline.ipynb`

## Key Learning Outcomes

### Streaming Data Processing
- Apache Spark Structured Streaming fundamentals
- Real-time data transformation and aggregation
- Watermarking and late data handling
- Multi-sink streaming architectures

### Data Integration Patterns
- Kafka producer/consumer implementations
- JDBC-based distributed database queries
- Redis integration for high-performance caching
- Cross-platform data synchronization

### Production Considerations
- Fault tolerance and checkpointing
- Resource optimization and parallel processing
- Schema evolution and data quality
- Monitoring and observability

## Performance Optimizations

The implementations include several production-ready optimizations:

- **Partitioning Strategy**: Country-based Kafka partitioning ensures data locality
- **Resource Management**: Configurable parallelism (2 cores) for resource-constrained environments
- **Connection Pooling**: Efficient database connection management
- **Checkpointing**: Fault-tolerant processing with configurable checkpoint locations

## Troubleshooting

### Common Issues

**Kafka Connection Errors**
```bash
# Verify Kafka is running
kafka-topics.sh --list --bootstrap-server localhost:9092
```

**Database Connection Issues**
```bash
# Test MariaDB connectivity
mysql -u spark -p -h localhost website_stats
```

**Port Conflicts**
The Spark UI automatically finds available ports (4040-4043) if default ports are occupied.

## Course Information

**Instructor**: Kumaran Ponnambalam  
**Duration**: 1 hour 4 minutes  
**Level**: Intermediate  
**Platform**: LinkedIn Learning  
**Release Date**: October 1, 2024

## Repository Structure

```
├── code_00_03_Spark_BDE_Setup_Prerequisites.ipynb
├── code_03_XX_Spark_BDE_Batch_Data_Engg.ipynb
├── code_04_03_Spark_BDE_Generate a visits stream.ipynb
├── code_04_04_Spark_BDE_Build_a_streaming_analytics_job.ipynb
├── code_04_05_Spark_BDE_Execute_the_pipeline.ipynb
├── code_06_03_Spark_BDE_Extracting_long_last_actions.ipynb
├── code_06_04_Spark_BDE_scorecard_for_last_action.ipynb
├── hadoop/
│   ├── bin/hadoop.dll
│   └── bin/winutils.exe
├── jars/
│   ├── commons-pool2-2.12.0.jar
│   ├── kafka-clients-3.6.0.jar
│   ├── mysql-connector-j-8.4.0.jar
│   └── spark-sql-kafka-0-10_*.jar
└── spark-docker.yml
```

## Contributing

This repository contains educational materials from a LinkedIn Learning course. For questions about the course content, please refer to the official course page or contact the instructor through LinkedIn Learning.

## License

Educational materials provided under LinkedIn Learning's terms of service. Check with LinkedIn Learning for specific usage rights and redistribution policies.

## Additional Resources

- [Apache Spark Documentation](https://spark.apache.org/docs/latest/)
- [Kafka Streaming Guide](https://kafka.apache.org/documentation/streams/)
- [Redis Commands Reference](https://redis.io/commands/)
- [LinkedIn Learning Course](https://www.linkedin.com/learning/apache-spark-essential-training-big-data-engineering-23165395/)

---

*This repository demonstrates enterprise-grade streaming analytics implementations suitable for production data engineering environments.*
