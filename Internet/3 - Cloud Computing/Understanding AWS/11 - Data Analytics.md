
- Athena
	- **Serverless** query service to analyze data stored in **S3** without infrastructure management. Ideal for ad-hoc querying and analysis of large datasets. **If analyze data in S3 using serverless SQL, use Athena**

- Redshift
	- **Fully managed** data warehouse optimized for online analytical processing (OLAP) workloads. Analyze large volumes of **structured data** with **high performance and scalability.**

- OpenSearch
	- Managed **Elasticsearch** service for real-time search, log analytics, and visualization of data. Full-text search, log analysis, and monitoring.

- EMR: Elastic MapReduce
	- Managed Hadoop(分布式计算) framework for processing big data workloads. Run distributed processing frameworks like Apache Hadoop, **Apache Spark,** and Apache Hive on AWS infrastructure.

- QuickSight
	- Business intelligence service for creating **interactive dashboards and visualizations** from various data sources. Gain insights from your data and share visualizations with stakeholders.

- Glue
	- ETL (Extract, Transform, Load) service for preparing and transforming data for analytics. Automate data ingestion, transformation, and loading tasks, making data analytics pipelines more efficient.

- Lake Formation
	- A data lake creation and management service that simplifies the process of building and managing data lakes on AWS. Centrally manage data access, security, and governance in your data lake environment.

- Kinesis Data Analytics
	- A service for analyzing **streaming data** in real-time. Process and analyze streaming data from various sources, such as IoT devices and application logs.

- MSK: Managed Streaming for Kafka
	- A fully managed Apache Kafka service for building and running real-time data streaming applications. Deploy and manage Apache Kafka clusters without the need for infrastructure management.

- Big Data Ingestion Pipeline
	- A custom solution or architecture for ingesting big data into AWS. Depending on the specific requirements, it could involve services like Kinesis Data Streams, Glue, or other data ingestion tools to collect, process, and load data into analytics systems.


Use Kinesis Data Analytics with Kinesis Data Streams as the underlying source of data.
