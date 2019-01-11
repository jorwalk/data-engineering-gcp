## 1) Test format
- The exam consists of 50 questions that must be answered in 2 hours.
- The questions are either multiple choice (pick one correct answer) or multiple answer (pick M of N possible answers). Questions can be marked for review later.
- The multiple choice questions are "classic" in the sense that two of the answers can be eliminated immediately. Some detail in the question will bias the choice of the remaining answers.
- The exam is taken on a computer and you will not have access to pen and paper.
- The screen is split in half. The left side contains the questions while the right side contains the case studies.
- The two case studies that are published online were the two case studies used in the exam. The exam is setup so that all of the questions pertaining to a particular case study appear together.

## 2) Study Approach
1. Review the [exam guide](https://cloud.google.com/certification/guides/data-engineer/).
- Take the Data Engineering course. It is useful but inadequate for passing the exam.
- Memorize this [flow chart](https://cloud.google.com/storage-options/).
- Go through tutorials for the various technologies.
- Spend extra time investigating BigQuery.
- Gain a basic understanding of machine learning including:
  - Understand the difference between classification and regression.
  - Understand the difference between supervised learning and unsupervised learning.
  - Understand the complexity of machine learning techniques, e.g., Which requires more resources: linear regression or neural networks?
  - Understand how to handle data for machine learning including partitioning data between training, test, and validation and preprocessing techniques like dimensionality reduction.
- Gain a familiarity with other popular databases and understand which ones should be used for particular problems:
  - [Setting up Redis](https://cloud.google.com/community/tutorials/setting-up-redis)
  - [Cassandra](https://console.cloud.google.com/launcher/details/datastax-public)
  - [HBase](https://cloud.google.com/bigtable/docs/bigtable-and-hbase)
  - [MySQL](https://cloud.google.com/solutions/setup-mysql)
  - [Mongo](https://cloud.google.com/solutions/deploy-mongodb)
  - [HDFS and extensions](https://cloud.google.com/dataproc/docs/concepts/connectors/cloud-storage)
- There will be a question on [HIPAA](https://cloud.google.com/security/compliance/hipaa/), [COPPA](https://www.google.com/cloud/security/compliance/coppa/), etc.
- Read the "Concepts" page for each of the technologies listed in the table.
- [Migrating On-Premises Hadoop Infrastructure to Google Cloud Platform](./articles/data-engineering-gcp/articles/hadoop-gcp-migration-overview.md)

| Topic	| Theory	| Practice |
| ------| :------:| :-------:|
| [Cloud Storage](https://cloud.google.com/storage/docs/concepts) - [Options](https://cloud.google.com/storage-options) |	xxx |	xx |
| [Cloud Spanner](https://cloud.google.com/spanner/docs/) |	x | |	 
| [Cloud SQL](https://cloud.google.com/sql/docs/) |	xxx |	x |
| [Cloud Datastore](https://cloud.google.com/datastore/docs/) |	x |	 |
| [Cloud Bigtable](https://cloud.google.com/bigtable/docs/) |	xxx |	x |
| [BigQuery](https://cloud.google.com/bigquery/docs/) |	xxx |	xxx |
| [Pub/Sub](https://cloud.google.com/pubsub/docs/) | xxx |	x |
| [DataProc](https://cloud.google.com/dataproc/docs/) |	xx |	x |
| [DataFlow](https://cloud.google.com/dataflow/docs/) |	xx |	x |
| [IAM and Security](https://cloud.google.com/iam/docs/) |	xx |	x |
| [Datalab](https://cloud.google.com/datalab/docs/) | x |	x |
| [DataStudio](https://cloud.google.com/data-studio/) |	x |	x |

---
#### Storage Options Flow Chart
![Storage Options Flow Chart](https://cloud.google.com/images/storage-options/flowchart.svg?sanitize=true)

#### Footnotes:
1. Cloud Spanner was not on the exam. Perhaps it was too new.

## Technical Review from YouTube user

* Case studies were part of the exam and you needed to review and answer the appropriate solutions for the specific questions. Case study had numerous question similar but had a slight question or answer so you needed to pay attention.
* [DataProc](https://cloud.google.com/dataproc/docs/) - Questions about migrating onsite Hadoop and how Cloud DataProc could help.
* Cloud DataFlow - Numerous questions around knowing how Cloud DataFlow fits, know stream verse batch, it's managed service so that you don't need to deal with it, how you can use it for ETL. Lastly, you will see several questions related to how Cloud DataFlow can manage services in Google Cloud Storage and Google Cloud Compute Engine.
* Pipelines in DataFlow and how you could use graph objects. One question about why you would use JSON or Java related to Pipelines.
* Storage around every aspect and needed to discern between nearline and Coldline. Big data, regional and standard storage. Cloud storage is a must to know.
* Hadoop. Numerous questions around HDFS but also when you would need to use approaches like Hive, Spoop, Oozie with Hadoop.
* Stackdriver - not really what I would call data engineering, but they asked four questions about his Hybrid monitoring service. I remember a few questions focused on how you can debug, monitor and log using Stackdriver. I think they were checking to confirm if you knew how to use Stackdriver to help debug source code, etc.
* BigTable - numerous questions about why and how you could use BigTable. Know it's a high performance NoSQL database service for large analytical and operational workloads.
* [BigQuery](https://cloud.google.com/bigquery/docs/) Tested heavily around data ingestion and availability. Once again they want to confirm you know that it's a fully managed, petabyte scale, low cost enterprise data warehouse for analytics. BigQuery is serverless.
* Cloud SQL - they wanted to confirm you knew it and why over other services like BigTable, BigQuery, etc.
* Tensorflow - Several questions also focused on Neural Networks, review this page.
* CloudLab - which is a powerful interactive tool created to explore, analyze, transform and visualize data and build machine learning models on Google cloud platform. Numerous questions here around how to visualize and create models.
* Pub/Sub. Numerous questions on Pub/Sub and how you would integrate it with other services, how to use it for publisher apps, messaging, resource types and models.
