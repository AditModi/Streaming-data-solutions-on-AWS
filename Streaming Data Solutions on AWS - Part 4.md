Processing real-time data as it arrives can enable you to make decisions much faster than is possible with traditional data analytics technologies.

You need a different set of tools to collect, prepare, and process real-time streaming data than those tools that you have traditionally used for batch analytics. With traditional analytics, you gather the data, load it periodically into a database, and analyze it hours, days, or weeks later. Analyzing real-time data requires a different approach.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/a50lcssy2hba5g9k13o5.png) 

> Streaming data solutions on AWS is a series containing different articles that review several scenarios for streaming workflows. In these scenarios, streaming data & processing it provides the example companies with the ability to add new features and functionality. By analyzing data as it gets created, they can gain insights into what their business is doing.

* AWS streaming services enable you to focus on your application to make time-sensitive business decisions, rather than deploying and managing the infrastructure.

# Scenario 4: Device sensors real-time anomaly detection and notifications

* Company ABC4Logistics transports highly flammable petroleum products such as gasoline, liquid propane (LPG), and naphtha from the port to various cities. There are hundreds of vehicles which have multiple sensors installed on them for monitoring things such as location, engine temperature, temperature inside the container, driving speed, parking location, road conditions, and so on. 

* One of the requirements ABC4Logistics has is to monitor the temperatures of the engine and the container in real-time and alert the driver and the fleet monitoring team in case of any anomaly. To detect such conditions and generate alerts in real-time, ABC4Logistics implemented the following architecture on AWS.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j1hr3695htd6n3degzjq.png)
*ABC4Logistics’s device sensors real-time anomaly detection and notifications architecture*

* Data from device sensors is ingested by AWS IoT Gateway, where the AWS IoT rules engine will make the streaming data available in Amazon Kinesis Data Streams. Using Kinesis Data Analytics, ABC4Logistics can perform the real-time analytics on streaming data in Kinesis Data Streams.

* Using Kinesis Data Analytics, ABC4Logistics can detect if temperature readings from the sensors deviate from the normal readings over a period of ten seconds, and ingest the record onto another Kinesis Data Streams instance, identifying the anomalous records. Amazon Kinesis Data Streams then invokes Lambda functions, which can send the alerts to the driver and the fleet monitoring team through Amazon SNS.

* Data in Kinesis Data Streams is also pushed down to Amazon Kinesis Data Firehose. Amazon Kinesis Data Firehose persists this data in Amazon S3, allowing ABC4Logistics to perform batch or near-real time analytics on sensor data. 

* ABC4Logistics uses Amazon Athena to query data in S3, and Amazon QuickSight for visualizations. For long-term data retention, the S3 Lifecycle policy is used to archive data to Amazon S3 Glacier.

* Important components of this architecture are detailed next.

## Amazon Kinesis Data Analytics

* Amazon Kinesis Data Analytics enables you to transform and analyze streaming data and respond to anomalies in real time. It is a serverless service on AWS, which means Kinesis Data Analytics takes care of provisioning, and elastically scales the infrastructure to handle any data throughput. 

* This takes away all the undifferentiated heavy lifting of setting up and managing the streaming infrastructure, and enables you to spend more time on writing steaming applications.

* With Amazon Kinesis Data Analytics, you can interactively query streaming data using multiple options, including Standard SQL, Apache Flink applications in Java, Python and Scala, and build Apache Beam applications using Java to analyze data streams.

* These options provide you with flexibility of using a specific approach depending on the complexity level of streaming application and source/target support. The following section discusses Kinesis Data Analytics for Flink Applications option.

## Amazon Kinesis Data Analytics for Apache Flink applications

* Apache Flink is a popular open-source framework and distributed processing engine for stateful computations over unbounded and bounded data streams. Apache Flink is designed to perform computations at in-memory speed and at scale with support for exactly-one semantics. Apache Flink-based applications help achieve low latency with high throughput in a fault tolerant manner.

* With Amazon Kinesis Data Analytics for Apache Flink, you can author and run code against streaming sources to perform time series analytics, feed real-time dashboards, and create real-time metrics without managing the complex distributed Apache Flink environment. 

* You can use the high-level Flink programming features in the same way that you use them when hosting the Flink infrastructure yourself.

* Kinesis Data Analytics for Apache Flink enables you to create applications in Java, Scala, Python or SQL to process and analyze streaming data. A typical Flink application reads the data from the input stream or data location or source, transforms/filters or joins data using operators or functions, and stores the data on output stream or data location, or sink.

* The following architecture diagram shows some of the supported sources and sinks for the Kinesis Data Analytics Flink application. 

* In addition to the pre-bundled connectors for source/sink, you can also bring in custom connectors to a variety of other source/sinks for Flink Applications on Kinesis Data Analytics.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/adfy8ephzaifh53f1fuf.png)
*Apache Flink application on Kinesis Data Analytics for real-time stream processing*

* Developers can use their preferred IDE to develop Flink applications and deploy them on Kinesis Data Analytics from AWS Management Console or DevOps tools.

### Amazon Kinesis Data Analytics Studio

* As part of Kinesis Data Analytics service, Kinesis Data Analytics Studio is available for customers to interactively query data streams in real time, and easily build and run stream processing applications using SQL, Python, and Scala. Studio notebooks are powered by Apache Zeppelin.

* Using Studio notebook, you have the ability to develop your Flink Application code in a notebook environment, view results of your code in real time, and visualize it within your notebook. You can create a Studio Notebook powered by Apache Zeppelin and Apache Flink with a single click from Kinesis Data Streams and Amazon MSK console, or launch it from Kinesis Data Analytics Console.

* Once you develop the code iteratively as part of the Kinesis Data Analytics Studio, you can deploy a notebook as a Kinesis data analytics application, to run in streaming mode continuously, reading data from your sources, writing to your destinations, maintaining long-running application state, and scaling automatically based on the throughput of your source streams. 

* Earlier, customers used Kinesis Data Analytics for SQL Applications for such interactive analytics of real-time streaming data on AWS.

* Kinesis Data Analytics for SQL applications is still available, but for new projects, AWS recommends that you use the new Kinesis Data Analytics Studio. Kinesis Data Analytics Studio combines ease of use with advanced analytical capabilities, which makes it possible to build sophisticated stream processing applications in minutes.

* For making the Kinesis Data Analytics Flink application fault-tolerant, you can make use of checkpointing and snapshots, as described in the Implementing Fault Tolerance in Kinesis Data Analytics for Apache Flink.

* Kinesis Data Analytics Flink applications are useful for writing complex streaming analytics applications such as applications with exactly-one semantics of data processing, checkpointing capabilities, and processing data from data sources such as Kinesis Data Streams, Kinesis Data Firehose, Amazon MSK, Rabbit MQ, and Apache Cassandra including Custom Connectors.

* After processing streaming data in the Flink application, you can persist data to various sinks or destinations such as Amazon Kinesis Data Streams, Amazon Kinesis Data Firehose, Amazon DynamoDB, Amazon OpenSearch Service, Amazon Timestream, Amazon S3, and so on. The Kinesis Data Analytics Flink application also provides sub-second performance guarantees.

### Apache Beam applications for Kinesis Data Analytics

* Apache Beam is a programming model for processing streaming data. Apache Beam provides a portable API layer for building sophisticated data-parallel processing pipelines that may be run across a diversity of engines, or runners such as Flink, Spark Streaming, Apache Samza, and so on.

* You can use the Apache Beam framework with your Kinesis data analytics application to process streaming data. Kinesis data analytics applications that use Apache Beam use Apache Flink runner to run Beam pipelines.

# Summary

* By making use of the AWS streaming services Amazon Kinesis Data Streams, Amazon Kinesis Data Analytics, and Amazon Kinesis Data Firehose,

* ABC4Logistics can detect anomalous patterns in temperature readings and notify the driver and the fleet management team in real-time, preventing major accidents such as complete vehicle breakdown or fire.

---

Hope this guide helps you understand how to Design a Streaming Data Solution on AWS for a "Device sensors real-time anomaly detection and notifications" scenario.

Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.

{% user aditmodi %}

---

[Reference Guide](https://docs.aws.amazon.com/whitepapers/latest/streaming-data-solutions-amazon-kinesis/scenario-4.html)