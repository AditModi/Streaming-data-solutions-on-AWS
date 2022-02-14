You can use streaming data services for real-time and near-real-time applications such as application monitoring, fraud detection, and live leaderboards. 

Real-time use cases require millisecond end-to-end latencies– from ingestion, to processing, all the way to emitting the results to target data stores and other systems.

For example, Netflix uses Amazon Kinesis Data Streams to monitor the communications between all its applications so it can detect and fix issues quickly, ensuring high service uptime and availability to its customers. 

While the most commonly applicable use case is application performance monitoring, there are an increasing number of real-time applications in ad tech, gaming, and IoT that fall under this category.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1olsuqk5u1jadqv7wcmv.png)

> Streaming data solutions on AWS is a series containing different articles that review several scenarios for streaming workflows. In these scenarios, streaming data & processing it provides the example companies with the ability to add new features and functionality. By analyzing data as it gets created, they can gain insights into what their business is doing.

* AWS streaming services enable you to focus on your application to make time-sensitive business decisions, rather than deploying and managing the infrastructure.
 
# Scenario 3: Preparing clickstream data for data insights processes

* Fast Sneakers is a fashion boutique with a focus on trendy sneakers. The price of any given pair of shoes can go up or down depending on inventory and trends, such as what celebrity or sports star was spotted wearing brand name sneakers on TV last night. It is important for Fast Sneakers to track and analyze those trends to maximize their revenue.

* Fast Sneakers does not want to introduce additional overhead into the project with new infrastructure to maintain. They want to be able to split the development to the appropriate parties, where the data engineers can focus on data transformation and their data scientists can work on their ML functionality independently.

* To react quickly and automatically adjust prices according to demand, Fast Sneakers streams significant events (like click-interest and purchasing data), transforming and augmenting the event data and feeding it to a ML model. 

* Their ML model is able to determine if a price adjustment is required. This allows Fast Sneakers to automatically modify their pricing to maximize profit on their products.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/31bx8mhqcfsat0d2s77k.png)
*Fast Sneakers real-time price adjustments*

* This architecture diagram shows the real-time streaming solution Fast Sneakers created utilizing Kinesis Data Streams, AWS Glue, and DynamoDB Streams. By taking advantage of these services, they have a solution that is elastic and reliable without needing to spend time on setting up and maintaining the supporting infrastructure. 

* They can spend their time on what brings value to their company by focusing on a streaming extract, transform, load (ETL) job and their machine learning model.

* To better understand the architecture and technologies that are used in their workload, the following are some details of the services used.

## AWS Glue and AWS Glue streaming

* AWS Glue is a fully managed ETL service that you can use to catalog your data, clean it, enrich it, and move it reliably between data stores. With AWS Glue, you can significantly reduce the cost, complexity, and time spent creating ETL jobs. AWS Glue is serverless, so there is no infrastructure to set up or manage. You pay only for the resources consumed while your jobs are running.

* Utilizing AWS Glue, you can create a consumer application with an AWS Glue streaming ETL job. This enables you to utilize Apache Spark and other Spark-based modules writing to consume and process your event data. The next section of this document goes into more depth about this scenario.

### AWS Glue Data Catalog

* The AWS Glue Data Catalog contains references to data that is used as sources and targets of your ETL jobs in AWS Glue. The AWS Glue Data Catalog is an index to the location, schema, and runtime metrics of your data. 

* You can use information in the Data Catalog to create and monitor your ETL jobs. Information in the Data Catalog is stored as metadata tables, where each table specifies a single data store. 

* By setting up a crawler, you can automatically assess numerous types of data stores, including DynamoDB, S3, and Java Database Connectivity (JDBC) connected stores, extract metadata and schemas, and then create table definitions in the AWS Glue Data Catalog.

* To work with Amazon Kinesis Data Streams in AWS Glue streaming ETL jobs, it is best practice to define your stream in a table in an AWS Glue Data Catalog database. You define a stream-sourced table with the Kinesis stream, one of the many formats supported (CSV, JSON, ORC, Parquet, Avro or a customer format with Grok). You can manually enter a schema, or you can leave this step to your AWS Glue job to determine during runtime of the job.

### AWS Glue streaming ETL job

* AWS Glue runs your ETL jobs in an Apache Spark serverless environment. AWS Glue runs these jobs on virtual resources that it provisions and manages in its own service account. In addition to being able to run Apache Spark based jobs, AWS Glue provides an additional level of functionality on top of Spark with DynamicFrames.

* DynamicFrames are distributed tables that support nested data such as structures and arrays. Each record is self-describing, designed for schema flexibility with semi-structured data. 

* A record in a DynamicFrame contains both data and the schema describing the data. Both Apache Spark DataFrames and DynamicFrames are supported in your ETL scripts, and you can convert them back and forth. DynamicFrames provide a set of advanced transformations for data cleaning and ETL.

* By using Spark Streaming in your AWS Glue Job, you can create streaming ETL jobs that run continuously, and consume data from streaming sources like Amazon Kinesis Data Streams, Apache Kafka, and Amazon MSK. 

* The jobs can clean, merge, and transform the data, then load the results into stores including Amazon S3, Amazon DynamoDB, or JDBC data stores.

* AWS Glue processes and writes out data in 100-second windows, by default. This allows data to be processed efficiently, and permits aggregations to be performed on data arriving later than expected. 

* You can configure the window size by adjusting it to accommodate the speed in response vs the accuracy of your aggregation. AWS Glue streaming jobs use checkpoints to track the data that has been read from the Kinesis Data Stream. 

* For a walkthrough on creating a streaming ETL job in AWS Glue you can refer to Adding Streaming ETL Jobs in AWS Glue

## Amazon DynamoDB

* Amazon DynamoDB is a key-value and document database that delivers single-digit millisecond performance at any scale. It's a fully managed, multi-Region, multi-active, durable database with built-in security, backup and restore, and in-memory caching for internet-scale applications. 

* DynamoDB can handle more than ten trillion requests per day, and can support peaks of more than 20 million requests per second.

### Change data capture for DynamoDB streams

* A DynamoDB stream is an ordered flow of information about changes to items in a DynamoDB table. When you enable a stream on a table, DynamoDB captures information about every modification to data items in the table. 

* DynamoDB runs on AWS Lambda so that you can create triggers—pieces of code that automatically respond to events in DynamoDB streams. With triggers, you can build applications that react to data modifications in DynamoDB tables.

* When a stream is enabled on a table, you can associate the stream Amazon Resource Name (ARN) with a Lambda function that you write. Immediately after an item in the table is modified, a new record appears in the table's stream. AWS Lambda polls the stream and invokes your Lambda function synchronously when it detects new stream records.

### Amazon SageMaker and Amazon SageMaker service endpoints

* Amazon SageMaker is a fully managed platform that enables developers and data scientists with the ability to build, train, and deploy ML models quickly and at any scale. SageMaker includes modules that can be used together or independently to build, train, and deploy your ML models. 

* With Amazon SageMaker service endpoints, you can create managed hosted endpoint for real-time inference with a deployed model that you developed within or outside of Amazon SageMaker.

* By utilizing the AWS SDK, you can invoke a SageMaker endpoint passing content type information along with content and then receive real-time predictions based on the data passed. 

* This enables you to keep the design and development of your ML models separated from your code that performs actions on the inferred results.

* This enables your data scientists to focus on ML, and the developers who are using the ML model to focus on how they use it in their code. For more information on how to invoke an endpoint in SageMaker, see InvokeEnpoint in the Amazon SageMaker API Reference.

### Inferring data insights in real time

* The previous architecture diagram shows that Fast Sneakers’ existing web application added a Kinesis Data Stream containing click-stream events, which provides traffic and event data from the website. 

* The product catalog, which contains information such as categorization, product attributes, and pricing, and the order table, which has data such as items ordered, billing, shipping, and so on, are separate DynamoDB tables. 

* The data stream source and the appropriate DynamoDB tables have their metadata and schemas defined in the AWS Glue Data Catalog to be used by the AWS Glue streaming ETL job.

* By utilizing Apache Spark, Spark streaming, and DynamicFrames in their AWS Glue streaming ETL job, Fast Sneakers is able to extract data from either data stream and transform it, merging data from the product and order tables. 

* With the hydrated data from the transformation, the datasets to get inference results from are submitted to a DynamoDB table.

* The DynamoDB Stream for the table triggers a Lambda function for each new record written. The Lambda function submits the previously transformed records to a SageMaker Endpoint with the AWS SDK to infer what, if any, price adjustments are necessary for a product. 

* If the ML model identifies an adjustment to the price is required, the Lambda function writes the price change to the product in the catalog DynamoDB table.

# Summary

* Amazon Kinesis Data Streams makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information. 

* Combined with the AWS Glue serverless data integration service, you can create real-time event streaming applications that prepare and combine data for ML.

* Because both Kinesis Data Streams and AWS Glue services are fully managed, AWS takes away the undifferentiated heavy lifting of managing infrastructure for your big data platform, letting you focus on generating data insights based on your data.

* Fast Sneakers can utilize real-time event processing and ML to enable their website to make fully automated real-time price adjustments, to maximize their product stock. This brings the most value to their business while avoiding the need to create and maintain a big data platform.

---

Hope this guide helps you understand how to Design a Streaming Data Solution on AWS for a "Preparing clickstream data for data insights processes" scenario.

Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.

{% user aditmodi %}

---

[Reference Guide](https://docs.aws.amazon.com/whitepapers/latest/streaming-data-solutions-amazon-kinesis/scenario-3.html)