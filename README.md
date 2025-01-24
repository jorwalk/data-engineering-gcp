# Google - Professional Data Engineer

A Professional Data Engineer enables data-driven decision making by collecting, transforming, and visualizing data. The Data Engineer designs, builds, maintains, and troubleshoots data processing systems with a particular emphasis on the security, reliability, fault-tolerance, scalability, fidelity, and efficiency of such systems.

The Data Engineer also analyzes data to gain insight into business outcomes, builds statistical models to support decision-making, and creates machine learning models to automate and simplify key business processes.

The Google Cloud Certified - Professional Data Engineer exam assesses your ability to:

- Build and maintain data structures and databases.
- Design data processing systems.
- Analyze data and enable machine learning.
- Model business processes for analysis and optimization.
- Design for reliability.
- Visualize data and advocate policy.
- Design for security and compliance.

**This repository contains a collection of resources that will help you prepare.**

## Review of tips
- **TIP 1:** Create your own custom preparation plan using the resources in this course.

- **TIP 2:** Use the Exam Guide outline to help identify what to study.
Read through the exam guide outline.

[https://cloud.google.com/certification/guides/data-engineer/](https://cloud.google.com/certification/guides/data-engineer/)

The outline communicates how an authority thinks about and organizes the skills required of a Professional Data Engineer.

Training tends to be organized in a ramp; basic concepts first, building into more complex and difficult concepts later. The Exam Guide is not training. So it is not organized for learning, or by importance, or by process. It is organized according to how a group of experts thinks about the job.

There is no additional information about what any particular line means; no explanation.

TIP - Use the Exam Guide outline to help identify what to study.
The recommendation is for you to read through each line and think about what it actually means, what do you think it is saying about the job? Then ask yourself if you understand that aspect of the job and if you feel you have the skills required to be successful in that part of the job. If yes -- great. It is an indicator that you are prepared. If no -- take note. You may want to study additionally in that part or develop specific skills for that item in the Exam Guide.

- **TIP 3:** Product and technology knowledge.
You need to know the basic information about each product that might be covered on the exam.

**You need to know:**

* What it does, why it exists.
* What is special about its design, for what purpose or purposes was it optimized?
* When do you use it, and what are the limits or bounds when it is time to consider an alternative?
* What are the key features of this product or technology?
* Is there an Open Source alternative? If so, what are the key benefits of the cloud-based service over the Open Source software?

**Which products and technologies**

Training and Certification meet at the JTA -- the Job Task Analysis -- the skills required of the job.

The scope of the exam matches the learning track and specialization in training. So a great place to derive a list of the technologies and products that might be on the exam is to look at all the products and technologies that are covered in the related training. The training might not cover everything. But it is a good place to start.

**Study methods**

Training is great. Digging into the online documentation can be very instructive and covers more detail than can be covered in a class, so documentation tends to have more equal coverage of features, whereas training has to prioritize its time. Getting hands on experience can help you understand a product or technology much better than reading and is the kind of experience a professional in the job would have. So labs can be a great way to prepare.


- **TIP 4:** This course has touchstone concepts for self-evaluation, not technical training. Seek training if needed.
https://www.coursera.org/learn/preparing-cloud-professional-data-engineer-exam/supplement/zIKFt/exam-tips-4
- **TIP 5:** Problem solving is the key skill.

- **TIP 6:** Practice evaluating your confidence in your answers.

- **TIP 7:** Practice case evaluation and creating proposed solutions.

- **Tip 8:** Use what you know and what you don't know to identify correct and incorrect answers.

- **Tip 9:** Review or rehearse labs to refresh your experience

- **Tip 10:** Prepare!

**Good luck!!**


## Certification Exam Guide
### Section 1: Designing data processing systems
**1.1 Designing flexible data representations. Considerations include:**

- [data-representations-pipelines-and-processing-infrastructure](https://www.coursera.org/learn/preparing-cloud-professional-data-engineer-exam/lecture/8qmyY/data-representations-pipelines-and-processing-infrastructure)

| Exam Outline  | Tips           |
| ------------- |-------------| 
| future advances in data technology | Tradeoffs between common formats and efficient formats |
| changes to business requirements   | Notice business changes that imply possible solution changes      |
| awareness of current state and how to migrate the design to a future state | What state/format is the data in and for what purpose, and where does it need to go, for what new purpose, and in what new state/format |
| data modeling | How do data items relate to one another? |
| trade-offs | JSON, XML, AVRO, CSV, Databases: SQL, NOSQL|
| distributed systems | Main points: Can the application deal with out-of-order data? Eventual consistency? Can the application handle the overhead and delay of ordering or synchronizing  |
| schema design | If the data is structured: organized for a purpose. |


**1.2 Designing data pipelines. Considerations include:**

| Exam Outline  | Tips          |
| ------------- |---------------| 
| future advances in data technology      |  |
| changes to business requirements      |       |
| awareness of current state and how to migrate the design to a future state |      |
| data modeling      |  |
| tradeoffs      |  |
| system availability      |  |
| distributed systems      |  |
| schema design      |  |
| common sources of error (eg. removing selection bias)      |  |


**1.3 Designing data processing infrastructure. Considerations include:**

| Exam Outline  | Tips          |
| ------------- |---------------| 
| future advances in data technology     |  |
| changes to business requirements      |       |
| awareness of current state, how to migrate the design to the future state |      |
| data modeling      |  |
| tradeoffs      |  |
| system availability      |  |
| distributed systems      |  |
| schema design      |  |
| capacity planning      |  |
| different types of architectures: message brokers, message queues, middleware, service-oriented      |  |


### Section 2: Building and maintaining data structures and databases
**2.1 Building and maintaining flexible data representations**

**2.2 Building and maintaining pipelines. Considerations include:**

- [building-and-maintaining-pipelines-and-processing-infrastructure](https://www.coursera.org/learn/preparing-cloud-professional-data-engineer-exam/lecture/xtSp8/)

| Exam Outline  | Tips          |
| ------------- |---------------| 
| data cleansing     |  |
| batch and streaming      |       |
| transformation |       |
| acquire and import data      |  |
| testing and quality control      |  |
| connecting to new data sources      |  |


**2.3 Building and maintaining processing infrastructure. Considerations include:**

| Exam Outline  | Tips          |
| ------------- |---------------| 
| provisioning resources |  |
| monitoring pipelines |       |
| adjusting pipelines |  |
| testing and quality control | |


### Section 3: Analyzing data and enabling machine learning

**3.1 Analyzing data. Considerations include:**

| Exam Outline  | Tips          |
| ------------- |---------------| 
| data collection and labeling     ||
| data visualization      |    |
| dimensionality reduction |    |
| data cleaning/normalization | |
| defining success metrics |  |




**3.2 Machine learning. Considerations include:**
https://www.coursera.org/learn/preparing-cloud-professional-data-engineer-exam/lecture/PlIKe/machine-learning-and-analysis

| Exam Outline  | Tips          |
| ------------- |---------------| 
| feature selection/engineering |  |
| algorithm selection |       |
| debugging a model |      |


**3.3 Machine learning model deployment. Considerations include:**

| Exam Outline  | Tips          |
| ------------- |---------------| 
| performance/cost optimization |  |
| online/dynamic learning |       |

### Section 4: Modeling business processes for analysis and optimization

**4.1 Mapping business requirements to data representations. Considerations include:**

| Exam Outline  | Tips          |
| ------------- |---------------| 
| working with business users |  |
| gathering business requirements |       |

**4.2 Optimizing data representations, data infrastructure performance and cost. Considerations include:**

| Exam Outline  | Tips          |
| ------------- |---------------| 
| resizing and scaling resources     |  |
| data cleansing, distributed systems      |       |
| high performance algorithms |     |
| common sources of error (eg. removing selection bias) |  |

### Section 5: Ensuring reliability

**5.1 Performing quality control. Considerations include:**

| Exam Outline  | Tips          |
| ------------- |---------------| 
| verification     | |
| building and running test suites      |    |
| pipeline monitoring |    |


**5.2 Assessing, troubleshooting, and improving data representations and data processing infrastructure.**

**5.3 Recovering data. Considerations include:**

| Exam Outline  | Tips          |
| ------------- |---------------| 
| planning (e.g. fault-tolerance)     |  |
| executing (e.g., rerunning failed jobs, performing retrospective re-analysis)      |       |
| stress testing data recovery plans and processes |      |

### Section 6: Visualizing data and advocating policy

**6.1 Building (or selecting) data visualization and reporting tools. Considerations include:**

| Exam Outline  | Tips          |
| ------------- |---------------| 
| automation    |  |
| decision support  |       |
| data summarization, (e.g, translation up the chain, fidelity, trackability, integrity) |  |


**6.2 Advocating policies and publishing data and reports.**

### Section 7: Designing for security and compliance

**7.1 Designing secure data infrastructure and processes. Considerations include:**

| Exam Outline  | Tips          |
| ------------- |---------------| 
| Identity and Access Management (IAM) | |
| data security  |       |
| penetration testing |      |
| Separation of Duties (SoD) |  |
| security control |  |

**7.2 Designing for legal compliance. Considerations include:**

| Exam Outline  | Tips          |
| ------------- |---------------| 
| legislation (e.g., Health Insurance Portability and Accountability Act (HIPAA), Children’s Online Privacy Protection Act (COPPA), etc.) |  |
| audits  |       |


## Acquire Hands-On Experience
Complete a set of self-paced labs around Data Engineering to gain hands-on experience.

### Qwiklabs Quests
Completion of the following Qwiklabs quests are highly recommended:
1. Advanced: [Machine Learning APIs](https://google.qwiklabs.com/quests/32?locale=en) (8 labs)
2. Advanced: [Data Science on the Google Cloud Platform](https://google.qwiklabs.com/quests/43?locale=en) (9 labs)
3. Advanced: [Scientific Data Processing](https://google.qwiklabs.com/quests/28?locale=en) (7 labs)
4. Expert: [Google Cloud Solutions II: Data and Machine Learning](https://google.qwiklabs.com/quests/38?locale=en) (10 labs)

### Gain Solution Design Experience
Review the data engineering solutions at [Google Cloud Solutions](https://cloud.google.com/solutions/) under the categories of
data processing, data warehousing, analytics and visualization, IoT, etc.

##### A. Data Processing
- [Data Lifecycle on Google Cloud Platform](https://cloud.google.com/solutions/data-lifecycle-cloud-platform)
- [Build a Data Lake on Google Cloud Platform](https://cloud.google.com/solutions/build-a-data-lake-on-gcp)
- [Migrating Hadoop Jobs from On-Premises to Google Cloud Platform](https://cloud.google.com/solutions/migration/hadoop/hadoop-gcp-migration-jobs)
- [Migrating HDFS Data from On-Premises to Google Cloud Platform](https://cloud.google.com/solutions/migration/hadoop/hadoop-gcp-migration-data)
- [Architecture: Apache Spark & Hadoop on Google Cloud Platform](https://cloud.google.com/solutions/architecture/hadoop)
- [Running RStudio® Server on a Cloud Dataproc Cluster](https://cloud.google.com/solutions/running-rstudio-server-on-a-cloud-dataproc-cluster)
- [Architecture: Complex Event Processing](https://cloud.google.com/solutions/architecture/complex-event-processing)

##### B. Data Warehouse
- [BigQuery for Data Warehouse Practitioners](https://cloud.google.com/solutions/bigquery-data-warehouse)
- [Performing ETL from a Relational Database into BigQuery](https://cloud.google.com/solutions/performing-etl-from-relational-database-into-bigquery)
- [Build a Marketing Data Warehouse on Google Cloud Platform](https://cloud.google.com/solutions/marketing-data-warehouse-on-gcp)
- [Schema Design for Time Series Data](https://cloud.google.com/bigtable/docs/schema-design-time-series)

##### C. Business Intelligence (Analytics and Visualization)
- [Creating an Authorized View in BigQuery](https://cloud.google.com/bigquery/docs/share-access-views)
- [Architecture: Optimizing Large-Scale Ingestion of Analytics Events and Logs](https://cloud.google.com/solutions/architecture/optimized-large-scale-analytics-ingestion)
- [Real-Time Data Analysis with Kubernetes, Cloud Pub/Sub, and BigQuery](https://cloud.google.com/solutions/real-time/kubernetes-pubsub-bigquery)
- [Building a Mobile Gaming Analytics Platform - a Reference Architecture](https://cloud.google.com/solutions/mobile/mobile-gaming-analysis-telemetry)
- [Creating Custom Interactive Dashboards with Bokeh and BigQuery](https://cloud.google.com/solutions/bokeh-and-bigquery-dashboards)
- [Visualizing BigQuery Data Using Google Cloud Datalab](https://cloud.google.com/bigquery/docs/visualize-datalab)
- [Visualizing BigQuery Data Using Google Data Studio](https://cloud.google.com/bigquery/docs/visualize-data-studio)

##### D. Machine Learning
- [Building a Serverless Machine Learning Model](https://cloud.google.com/solutions/building-a-serverless-ml-model)
- [Architecture of a Serverless Machine Learning Model](https://cloud.google.com/solutions/architecture-of-a-serverless-ml-model)
- [Using Cloud Dataflow for Batch Predictions with TensorFlow](https://cloud.google.com/solutions/using-cloud-dataflow-for-batch-predictions-with-tensorflow)
- [Running R at Scale on Compute Engine](https://cloud.google.com/solutions/running-r-at-scale)
- [Using Distributed TensorFlow with Cloud ML Engine and Cloud Datalab](https://cloud.google.com/ml-engine/docs/tensorflow/distributed-tensorflow-mnist-cloud-datalab)
- [Creating an Object Detection Application Using TensorFlow](https://cloud.google.com/solutions/creating-object-detection-application-tensorflow)
- [Using Machine Learning on Compute Engine to Make Product Recommendations](https://cloud.google.com/solutions/recommendations-using-machine-learning-on-compute-engine)
- [Optical Character Recognition (OCR) Tutorial](https://cloud.google.com/functions/docs/tutorials/ocr)
- [An Image Search Application that Uses the Cloud Vision API](https://cloud.google.com/solutions/image-search-app-with-cloud-vision)
- [Considerations for Sensitive Data within Machine Learning Datasets](https://cloud.google.com/solutions/sensitive-data-and-ml-datasets)

##### E. IoT
- [Overview of Internet of Things](https://cloud.google.com/solutions/iot-overview)
- [Architecture: Real-Time Stream Processing for IoT](https://cloud.google.com/solutions/architecture/real-time-stream-processing-iot)
- [Automating IoT Machine Learning: Bridging Cloud and Device Benefits with Cloud ML Engine](https://cloud.google.com/solutions/automating-iot-machine-learning)
- [Oil and Gas Equipment Monitoring and Analytics](https://cloud.google.com/solutions/oil-and-gas-equipment-monitoring-and-analytics)
- [Designing a Connected Vehicle Platform on Cloud IoT Core](https://cloud.google.com/solutions/designing-connected-vehicle-platform)

### Review Documentation, Blogs and Whitepapers
- Review the [Pricing Calculator](https://cloud.google.com/products/calculator/), [Product Pricing](https://cloud.google.com/pricing/), [Cost Comparison Calculator](https://cloud.google.com/pricing/#calculators) and the
[Always Free Usage Limits](https://cloud.google.com/free/docs/always-free-usage-limits).
- Read the Google Cloud Platform [security](https://cloud.google.com/security/) whitepapers. For example: [Infrastructure
Security](https://cloud.google.com/security/infrastructure/design/) and [Encryption at Rest](https://cloud.google.com/security/encryption-at-rest/default-encryption/).
- Read the [Site Reliability Engineering Book](https://landing.google.com/sre/book/index.html), especially the Chapter 25 (Data Processing
Pipelines), Chapter 26 (Data Integrity: What You Read Is What You Wrote).
- Explore the current Google Cloud Platform Marketplace solution offerings.
- In general, review the [Google Cloud Platform Documentation](https://cloud.google.com/docs/) and the [Google Cloud Platform Blogs](https://cloudplatform.googleblog.com/).


## Conceptual Knowledge Articles
- [Google Cloud Platform (GCP) services you can use to manage data throughout its entire lifecycle, from initial acquisition to final visualization.](articles/data-lifecycle-cloud-platform.md)
- [Migrating On-Premises Hadoop Infrastructure to Google Cloud Platform](./articles/hadoop-gcp-migration-overview.md)
- [User Contributed Study Guide](study-guide.md)
- [Certification Exam Guide](data-engineer-certificate-exam-guide.md)

## Case Studies
- [Flowlogistic Case Study](./case-study/flowlogistic.md)
- [Flowlogistic Video Brief](https://www.youtube.com/watch?v=r_yYDysfB-k)
- [MJTelco Case Study](./case-study/mjtelco.md)

## Need To Know
- [BigQuery](./know/bigquery.md)
- [BigTable](https://cloud.google.com/bigtable/docs/overview)

## Google Developers Codelabs
Provide a guided, tutorial, hands-on coding experience. Most codelabs will step you through the process of building a small application, or adding a new feature to an existing application. They cover a wide range of topics such as Android Wear, Google Compute Engine, Project Tango, and Google APIs on iOS.

[https://codelabs.developers.google.com/](https://codelabs.developers.google.com/)

### Labs and demos for courses for GCP Training
https://github.com/GoogleCloudPlatform/training-data-analyst

### In this lab you spin up a virtual machine, configure its security, and access it remotely.
https://codelabs.developers.google.com/codelabs/cpb100-compute-engine/

### In this lab you carry out the steps of an ingest-transform-and-publish data pipeline manually.
https://codelabs.developers.google.com/codelabs/cpb100-cloud-storage/

### Geographic data in Datalab
This notebook demonstrates how to use Datalab to display the earthquakes that have happened over the past 7 days. The data come from USGS, and we will use the Python module basemap to do the plotting.
https://github.com/GoogleCloudPlatform/datalab-samples/blob/master/basemap/earthquakes.ipynb

### Setup rentals data in Cloud SQL
https://codelabs.developers.google.com/codelabs/cpb100-cloud-sql/
* Create Cloud SQL instance
* Create database tables by importing .sql files from Cloud Storage
* Populate the tables by importing .csv files from Cloud Storage
* Allow access to Cloud SQL
* Explore the rentals data using SQL statements from CloudShell

### Setup rentals data in Cloud SQL
https://codelabs.developers.google.com/codelabs/cpb100-dataproc/
* Launch DataprocRun Spark
* ML jobs using Dataproc

### Create ML dataset with BigQuery
https://codelabs.developers.google.com/codelabs/cpb100-datalab
```
gcloud compute zones list
datalab connect mydatalabvm
datalab create mydatalabvm --zone us-central1-a
```
https://codelabs.developers.google.com/codelabs/cpb100-bigquery-dataset/
* Use BigQuery and Datalab to explore and visualize data
* Build a Pandas dataframe that will be used as the training dataset for machine learning using TensorFlow

https://codelabs.developers.google.com/codelabs/cpb100-tensorflow/
* Use TensorFlow to create a neural network model to forecast taxicab demand in NYC

Machine Learning APIs
* Learn how to invoke ML APIs from Python and use their results.
https://codelabs.developers.google.com/codelabs/cpb100-translate-api/

- Cloud Datastore: https://cloud.google.com/datastore/
- Cloud Bigtable: https://cloud.google.com/bigtable/
- Google BigQuery: https://cloud.google.com/bigquery/
- Cloud Datalab: https://cloud.google.com/datalab/
- TensorFlow: https://www.tensorflow.org/
- Cloud Machine Learning: https://cloud.google.com/ml/
- Vision API: https://cloud.google.com/vision/
- Translate API: https://cloud.google.com/translate/
- Speech API: https://cloud.google.com/speech/


## From around the web
```
I took the exam today and there were 50 questions. The notes in the blog helped me to focus my study a lot; thanks!

Here are my revisions as the exam isn’t a beta anymore:
Cloud Storage and Cloud Datastore – ~2 questions
Cloud SQL – ~2 questions
Bigtable – ~8 questions. How to optimize perf or troubleshoot slowdowns. What use cases would fit, etc.
BigQuery – ~8 questions. Data partitioning techniques, optimizing performance. Sharing data with other orgs. How to give list priv needed to users. How to avoid costly queries. Loading and differences of availability of data for streaming vs. batch.
Pub/Sub – ~3 questions. Basic knowledge was enough
Apache Hadoop – ~3 questions that were not GCP knowledge but Hadoop ecosystem knowledge; specifically pig and hive and what scenarios would push you to one or the other
Cloud Dataflow – ~8 questions. Understand batch and streaming designs. How to integrate with BigQuery and constraints you might have.
Cloud Dataproc – ~3 questions.
TensorFlow, Machine Learning, – ~10 questions that were mostly ML domain (and not TensorFlow specific) basically about training. Nothing about the Cloud ML service!
Cloud DataLab – ~2 questions about visualization, permissions/restricting access, creating dynamic dashboards
Stackdriver – ~1 question about auditing and viewing who did what in BigQuery

```

```
I have summarized and would like to post what I have felt after I failed the test. Hope that this will help some ones.

-	The content of the exam covers all the knowledge about Google Cloud Platform (GCP) for Data Engineering, including: Storage (20% of questions), Big Data Processing (35%), Machine Learning (18%), case studies (15%) and others (Hadoop and security about 12%).

-	GCP Storage (20%): Covering knowledge about Cloud Storage, Cloud SQL, Data Store, Big Table and Big Query. To answer these questions, we need to deeply understand in which situation which storage technology is used to give us the best optimal solution. There are some questions related to design schema for Data Store and Big Table.

-	Big Data Processing (35%): Covering knowledge about Big Query, Cloud Dataflow, Cloud Dataproc, Cloud Datalab and Cloud Pub/Sub. There are many questions related to Dataproc and Dataflow in my test. In each question, each choice usually is a combination of some GCP technologies in order to create a solution, then we need to choose which solution is the best suitable (in technically, other solutions may be possible but not the best choice).

-	Machine Learning (18%): Covering knowledge on GCP API (Vision API, Speech API, Natural Language API and Translate API) and Tensorflow. I was a little bit surprised when there were fewer Machine Learning (ML) questions in my test than I expected. Problems and targets of some ML questions are not very clear and I felt vague on selecting the correct answer.

-	Case Studies (15%): There are 2 case studies which are as same as in the GCP website: a logistic Flowlogistic company and a communications hardware MJTelco company. Each case study includes about 4 questions which ask how to transform current technologies of that company to use GCP technologies. We can learn details about these case studies in LinuxAcademy.

-	Others (12%): Covering knowledge on Hadoop, and Security Issues. There are some questions that are out of the scope of the GCP Data Engineering, in my opinion, such as questions on Google Cloud Architect and Encryption technology of Security. In my opinion, these are difficult questions for my because I don't have background and GCP architect. To answer these questions, we need to prepare some knowledge in Google Cloud Architect. One notice is that there are some questions where we need to select multiple choices. For example, we need to select 3 answers from 6 choices. In such of case, the first and second choices are usually easier to select than the last choice.
```

```
Introduction
Priocept consultants have recently been participating in the Google Cloud Platform beta certification exams.  We have been working with Google Cloud Platform for many years – since the original launch of Google App Engine – but the new certification scheme allows us to formalize our consultants’ expertise on the platform.

The “beta” nature of the exams means that our consultants have acted as Google guinea pigs to some degree.  Very little study material or practice questions are available at the moment for the certification exams, and you have to rely on prior practical experience and reading the core documentation.  So this blog article is intended to give an overview of the content for the Certified Data Engineer exam, as taken by our consultants in January 2017.

The Data Engineer certification covers a wide range of subjects including Google Cloud Platform data storage, analytical, machine learning, and data processing products.  Below we have given an overview, product-by-product, of what we were subjected to in the exam.

Cloud Storage and Cloud Datastore
Surprisingly, these products are not covered much in the exam, perhaps because they are covered more extensively in the Cloud Architect exam.  Just know the basic concepts of each product and when it is appropriate (or not appropriate) to use each product, and you should be fine.

Cloud SQL
There were surprisingly few questions on this product in the exam.  If you have practical experience using the product, you should be fine to answer any questions that do come up.  As with questions related to other data storage products, be sure to know in what scenarios it is appropriate to use Cloud SQL and when it would be more appropriate to use Datastore, Bigquery, Bigtable, etc.

Bigtable
This product is covered quite extensively in the exam.  You should at least know the basic concepts of the product, such as how to design an appropriate schema, how to define a suitable row key, whether Bigtable supports transactions and ACID operations, and you should also know (at least approximately) what the size limits for Bigtable are (cell and row size, maximum number of tables, etc).

BigQuery
Lots of questions on BigQuery in the Data Engineer exam, as expected.  You should know about the basic capabilities of BigQuery and what kind of problem domains it is suitable for.  You should also know about BigQuery security and the level at which security can be applied (project and datastore level, but not table or view level).  Partitioned tables, table wildcard queries (“backtick” syntax), streaming inserts, query planning and data skew are also covered.  You should also have an understanding of the methods available to connect external systems or tools to BigQuery for analytics purposes,  how the BigQuery billing model works, and who gets billed when queries cross project and billing account boundaries.

Pub/Sub
The exam contains lots of questions on this product, but all reasonably high level so it’s just important to know the basic concepts (topics, subscriptions, push and pull delivery flows, etc).  Most importantly you should know when it is appropriate to introduce Pub/Sub as a messaging layer in an architecture, for a given set of requirements.

Apache Hadoop
Technically not part of Google Cloud Platform, but there are a few questions around this technology in the exam, since it is the underlying technology for Dataproc.  Expect some questions on what HDFS, Hive, Pig, Oozie or Sqoop are, but basic knowledge on what each technology is and when to use it should be sufficient.

Cloud Dataflow
Lots of questions on this product, which is not surprising as it is a key area of focus for Google with regard to data processing on Google Cloud Platform.  In addition to knowing the basic capabilities of the product, you will also need to understand concepts like windowing types, triggers, PCollections, etc.

Cloud Dataproc
Not many questions on this besides the Hadoop questions mentioned above.  Just be sure to understand the differences between Dataproc and Dataflow and when to use one or the other.  Dataflow is typically preferred for a new development, whereas Dataproc would be required if migrating existing on-premise Hadoop or Spark infrastructure to Google Cloud Platform without redevelopment effort.

TensorFlow, Machine Learning, Cloud DataLab
The exam contains a significant amount of questions on this – more than we were expecting.  Fortunately we have been busy working with TensorFlow and Cloud Datalab at Priocept for a while now.  You should understand all the basic concepts of designing and developing a machine learning solution on TensorFlow, including concepts such data correlation analysis in Datalab, and overfitting and how to correct it.  Detailed TensorFlow or Cloud ML programming knowledge is not required but a good understanding of machine learning design and implementation is important.

Stackdriver
A surprising numbers of questions on this, given that Stackdriver is more of an “ops” product than a “data engineering” product.  Be sure to know the sub-products of Stackdriver (Debugger, Error Reporting, Alerting, Trace, Logging), what they do and when they should be used.

Conclusion
The Data Engineer certification exam is a fair assessment of the skills required if you want to be able to demonstrate the ability to work effectively with Google Cloud Platform on analytics, big data, data processing, or machine learning projects.  If you have used the majority of these products already on real-world products, the exam should not present you with too many problems.  If you haven’t yet used some of the products above, then get studying!

If you take the exam and get caught out in any areas that we haven’t covered above, please let us know.

Priocept provides both consultancy and bespoke training services for Google Cloud Platform, so please get in touch if we can help your organisation on your journey towards adopting the platform.
```

```
On both times I tried, it was heavy on BigQuery (20-25 questions) and TF/ML (10-15 questions), and spread out among the dataproc, dataflow and pubsub, with a couple of questions about SQL and datastore, and another couple of questions about general sysadmin/dba basic stuff.

All the questions are scenario simulations where you have to choose which option would be the best way to deal with the situation, according to google standards and documentation. Be it choosing the right tech or the right way to configure it.

They focus a lot on BigQuery's interaction with cloud storage and other solutions.

What I didnt do much when studying the last two times was exercises and practicing with the solutions.I focused mainly on the theoretical stuff, and really thought I was doing well on my second try, specially since I was comfortably answering the BigQuery questions. But still failed... :(

I'm focusing on hands-on practice now, and will try one last time on august.

```

* Avro vs Gzip for compression
* Dataflow troubleshooting
* ML Question
- outlier detection
- Supervised vs Unsupervised
- Reinforcement Learning
* Access to give Dataflow - you need to ask a consultant to help your
- developer role
- Dataflow Developer Role
- service account verse role
