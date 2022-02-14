Stream processing applications process data continuously in real-time, even before it is stored. Streaming data can come in at a blistering pace and data volumes can vary up and down at any time. Stream data processing platforms have to be able to handle the speed and variability of incoming data and process it as it arrives, often millions to hundreds of millions of events per hour.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yfydl3mgckz5wyzmarip.png)

> Streaming data solutions on AWS is a series containing different articles that review several scenarios for streaming workflows. In these scenarios, streaming data & processing it provides the example companies with the ability to add new features and functionality. By analyzing data as it gets created, they can gain insights into what their business is doing.

* AWS streaming services enable you to focus on your application to make time-sensitive business decisions, rather than deploying and managing the infrastructure.

# Scenario 5: Real time telemetry data monitoring with Apache Kafka

* ABC1Cabs is an online cab booking services company. All the cabs have IoT devices that gather telemetry data from the vehicles. Currently, ABC1Cabs is running Apache Kafka clusters that are designed for real-time event consumption, gathering system health metrics, activity tracking, and feeding the data into Apache Spark Streaming platform built on a Hadoop cluster on-premises.

* ABC1Cabs use OpenSearch Dashboards for business metrics, debugging, alerting, and creating other dashboards. They are interested in Amazon MSK, Amazon EMR with Spark Streaming, and OpenSearch Service with OpenSearch Dashboards. 

* Their requirement is to reduce admin overhead of maintaining Apache Kafka and Hadoop clusters, while using familiar open-source software and APIs to orchestrate their data pipeline. The following architecture diagram shows their solution on AWS.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8xhfnpxgsufordn2e2e8.png)
*Real-time processing with Amazon MSK and Stream processing using Apache Spark Streaming on Amazon EMR and Amazon OpenSearch Service with OpenSearch Dashboards*
      
* The cab IoT devices collect telemetry data and send to a source hub. The source hub is configured to send data in real time to Amazon MSK. Using the Apache Kafka producer library APIs, Amazon MSK is configured to stream the data into an Amazon EMR cluster. The Amazon EMR cluster has a Kafka client and Spark Streaming installed to be able to consume and process the streams of data.

* Spark Streaming has sink connectors which can write data directly to defined indexes of Elasticsearch. Elasticsearch clusters with OpenSearch Dashboards can be used for metrics and dashboards. 

* Amazon MSK, Amazon EMR with Spark Streaming, and OpenSearch Service with OpenSearch Dashboards are all managed services, where AWS manages the undifferentiated heavy lifting of infrastructure management of different clusters, which enables you to build your application using familiar open-source software with few clicks. The next section takes a closer look at these services.

## Amazon Managed Streaming for Apache Kafka (Amazon MSK)

* Apache Kafka is an open-source platform that enables customers to capture streaming data like click stream events, transactions, IoT events, and application and machine logs. 

* With this information, you can develop applications that perform real-time analytics, run continuous transformations, and distribute this data to data lakes and databases in real-time.

* You can use Kafka as a streaming data store to decouple applications from producer and consumers and enable reliable data transfer between the two components. 

* While Kafka is a popular enterprise data streaming and messaging platform, it can be difficult to set up, scale, and manage in production.

* Amazon MSK takes care of these managing tasks and makes it easy to set up, configure, and run Kafka, along with Apache Zookeeper, in an environment following best practices for high availability and security. You can still use Kafka's control-plane operations and data-plane operations to manage producing and consuming data.

* Because Amazon MSK runs and manages open-source Apache Kafka, it makes it easy for customers to migrate and run existing Apache Kafka applications on AWS without needing to make changes to their application code.

### Scaling

* Amazon MSK offers scaling operations so that user can scale the cluster actively while its running. When creating an Amazon MSK cluster, you can specify the instance type of the brokers at cluster launch. 

* You can start with a few brokers within an Amazon MSK cluster. Then, using the AWS Management Console or AWS CLI, you can scale up to hundreds of brokers per cluster.

* Alternatively, you can scale your clusters by changing the size or family of your Apache Kafka brokers. Changing the size or family of your brokers gives you the flexibility to adjust your Amazon MSK cluster’s compute capacity for changes in your workloads. 

* Use the Amazon MSK Sizing and Pricing spreadsheet (file download) to determine the correct number of brokers for your Amazon MSK cluster. This spreadsheet provides an estimate for sizing an Amazon MSK cluster and the associated costs of Amazon MSK compared to a similar, self-managed, EC2-based Apache Kafka cluster.

* After creating the Amazon MSK cluster, you can increase the amount of EBS storage per broker with exception of decreasing the storage. Storage volumes remain available during this scaling-up operation. It offers two types of scaling operations: Auto Scaling and Manual Scaling.

* Amazon MSKsupports automatic expansion of your cluster's storage in response to increased usage using Application Auto Scaling policies. Your automatic scaling policy sets the target disk utilization and the maximum scaling capacity.

* The storage utilization threshold helps Amazon MSK to trigger an automatic scaling operation. To increase storage using manual scaling, wait for the cluster to be in the ACTIVE state. Storage scaling has a cooldown period of at least six hours between events. 

* Even though the operation makes additional storage available right away, the service performs optimizations on your cluster that can take up to 24 hours or more.

* The duration of these optimizations is proportional to your storage size. Additionally, it also offers multi–Availability Zones replication within an AWS Region to provide High Availability.

### Configuration

* Amazon MSK provides a default configuration for brokers, topics, and Apache Zookeeper nodes. You can also create custom configurations and use them to create new Amazon MSK clusters or update existing clusters. 

* When you create an MSK cluster without specifying a custom Amazon MSK configuration, Amazon MSK creates and uses a default configuration. For a list of default values, see this Apache Kafka Configuration.

* For monitoring purposes, Amazon MSK gathers Apache Kafka metrics and sends them to Amazon CloudWatch, where you can view them. The metrics that you configure for your MSK cluster are automatically collected and pushed to CloudWatch. 

* Monitoring consumer lag enables you to identify slow or stuck consumers that aren't keeping up with the latest data available in a topic. When necessary, you can then take remedial actions, such as scaling or rebooting those consumers.

## Migrating to Amazon MSK

* Migrating from on premises to Amazon MSK can be achieved by one of the following methods.

 * **MirrorMaker2.0** — MirrorMaker2.0 (MM2) MM2 is a multi-cluster, data replication engine based on Apache Kafka Connect framework. MM2 is a combination of an Apache Kafka source connector and a sink connector. You can use a single MM2 cluster to migrate data between multiple clusters. 

* MM2 automatically detects new topics and partitions, while also ensuring the topic configurations are synced between clusters. MM2 supports migrations ACLs, topics configurations, and offset translation. For more details related to migration, see Migrating Clusters Using Apache Kafka's MirrorMaker. 

* MM2 is used for use cases related to replication of topics configurations and offset translation automatically.

 * **Apache Flink** — MM2 supports at least once semantics. Records can be duplicated to the destination and the consumers are expected to be idempotent to handle duplicate records. In exactly-once scenarios, semantics are required customers can use Apache Flink. It provides an alternative to achieve exactly once semantics.

* Apache Flink can also be used for scenarios where data requires mapping or transformation actions before submission to the destination cluster. Apache Flink provides connectors for Apache Kafka with sources and sinks that can read data from one Apache Kafka cluster and write to another. 

* Apache Flink can be run on AWS by launching an Amazon EMR cluster or by running Apache Flink as an application using Amazon Kinesis Data Analytics.

 * **AWS Lambda** — With support for Apache Kafka as an event source for AWS Lambda, customers can now consume messages from a topic via a Lambda function. The AWS Lambda service internally polls for new records or messages from the event source, and then synchronously invokes the target Lambda function to consume these messages. 

* Lambda reads the messages in batches and provides the message batches to your function in the event payload for processing. Consumed messages can then be transformed and/or written directly to your destination Amazon MSK cluster.

## Amazon EMR with Spark streaming

* Amazon EMR is a managed cluster platform that simplifies running big data frameworks, such as Apache Hadoop and Apache Spark on AWS, to process and analyze vast amounts of data.

* Amazon EMR provides the capabilities of Spark and can be used to start Spark streaming to consume data from Kafka. Spark Streaming is an extension of the core Spark API that enables scalable, high-throughput, fault-tolerant stream processing of live data streams.

* You can create an Amazon EMR cluster using the AWS Command Line Interface (AWS CLI) or on the AWS Management Console and select Spark and Zeppelin in advanced configurations while creating the cluster. 

* As shown in the following architecture diagram, data can be ingested from many sources such as Apache Kafka and Kinesis Data Streams, and can be processed using complex algorithms expressed with high-level functions such as map, reduce, join and window. For more information, see Transformations on DStreams.

* Processed data can be pushed out to filesystems, databases, and live dashboards.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fswfho0s9n4pw9i4i75m.png)
*Real-time streaming flow from Apache Kafka to Hadoop ecosystem*

* By default, Apache Spark Streaming has a micro-batch run model. However, since Spark 2.3 came out, Apache has introduced a new low-latency processing mode called Continuous Processing, which can achieve end-to-end latencies as low as one millisecond with at-least-once guarantees.

* Without changing the Dataset/DataFrames operations in your queries, you can choose the mode based on your application requirements. Some of the benefits of Spark Streaming are:

* It brings Apache Spark's language-integrated API to stream processing, letting you write streaming jobs the same way you write batch jobs.

* It supports Java, Scala and Python.

* It can recover both lost work and operator state (such as sliding windows) out of the box, without any extra code on your part.

* By running on Spark, Spark Streaming lets you reuse the same code for batch processing, join streams against historical data, or run ad hoc queries on the stream state and build powerful interactive applications, not just analytics.

* After the data stream is processed with Spark Streaming, OpenSearch Sink Connector can be used to write data to the OpenSearch Service cluster, and in turn, OpenSearch Service with OpenSearch Dashboards can be used as consumption layer.

## Amazon OpenSearch Service with OpenSearch Dashboards

* OpenSearch Service is a managed service that makes it easy to deploy, operate, and scale OpenSearch clusters in the AWS Cloud. OpenSearch is a popular open-source search and analytics engine for use cases such as log analytics, real-time application monitoring, and clickstream analysis.

* OpenSearch Dashboards is an open-source data visualization and exploration tool used for log and time-series analytics, application monitoring, and operational intelligence use cases. 

* It offers powerful and easy-to-use features such as histograms, line graphs, pie charts, heat maps, and built-in geospatial support.

* OpenSearch Dashboards provides tight integration with OpenSearch, a popular analytics and search engine, which makes OpenSearch Dashboards the default choice for visualizing data stored in OpenSearch. 

* OpenSearch Service provides an installation of OpenSearch Dashboards with every OpenSearch Service domain. You can find a link to OpenSearch Dashboards on your domain dashboard on the OpenSearch Service console.

# Summary

* With Apache Kafka offered as a managed service on AWS, you can focus on consumption rather than on managing the coordination between the brokers, which usually requires a detailed understanding of Apache Kafka. 

* Features such as high availability, broker scalability, and granular access control are managed by the Amazon MSK platform.

* ABC1Cabs utilized these services to build production application without needing infrastructure management expertise. They could focus on the processing layer to consume data from Amazon MSK and further propagate to visualization layer.

* Spark Streaming on Amazon EMR can help real-time analytics of streaming data, and publishing on OpenSearch Dashboards on Amazon OpenSearch Service for the visualization layer.

---

Hope this guide helps you understand how to Design a Streaming Data Solution on AWS for a "Internet offering based on location" scenario.

Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.

{% user aditmodi %}

---

[Reference Guide](https://docs.aws.amazon.com/whitepapers/latest/streaming-data-solutions-amazon-kinesis/scenario-5.html)