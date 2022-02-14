Businesses today receive data at massive scale and speed due to the explosive growth of data sources that continuously generate streams of data. Whether it is log data from application servers, clickstream data from websites and mobile apps, or telemetry data from Internet of Things (IoT) devices, it all contains information that can help you learn about what your customers, applications, and products are doing right now.

Having the ability to process and analyze this data in real-time is essential to do things such as continuously monitor your applications to ensure high service uptime and personalize promotional offers and product recommendations Processing streams of data with AWS Lambda.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ta1koe1ixdzagungqrts.png) 

> **Streaming data solutions on AWS** is a series containing different articles that review several scenarios for streaming workflows. In these scenarios, streaming data & processing it provides the example companies with the ability to add new features and functionality. By analyzing data as it gets created, they can gain insights into what their business is doing.

* AWS streaming services enable you to focus on your application to make time-sensitive business decisions, rather than deploying and managing the infrastructure.

# Scenario 1: Internet offering based on location

* Company "InternetProvider" provides internet services with a variety of bandwidth options to users across the world. When a user signs up for internet, company "InternetProvider" provides the user with different bandwidth options based on user’s geographic location. 

* Given these requirements, company "InternetProvider" implemented an Amazon Kinesis Data Streams to consume user details and location. The user details and location are enriched with different bandwidth options prior to publishing back to the application. AWS Lambda enables this real-time enrichment.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qqb1ng3rz6f48a4gwv7w.png)

# Amazon Kinesis Data Streams

* Amazon Kinesis Data Streams enables you to build custom, real-time applications using popular stream processing frameworks and load streaming data into many different data stores. 

* A Kinesis stream can be configured to continuously receive events from hundreds of thousands of data producers delivered from sources like website click-streams, IoT sensors, social media feeds and application logs. Within milliseconds, data is available to be read and processed by your application.

* When implementing a solution with Kinesis Data Streams, you create custom data-processing applications known as Kinesis Data Streams applications. A typical Kinesis Data Streams application reads data from a Kinesis stream as data records.

* Data put into Kinesis Data Streams is ensured to be highly available and elastic, and is available in milliseconds. You can continuously add various types of data such as clickstreams, application logs, and social media to a Kinesis stream from hundreds of thousands of sources. Within seconds, the data will be available for your Kinesis Applications to read and process from the stream.

* Amazon Kinesis Data Streams is a fully managed streaming data service. It manages the infrastructure, storage, networking, and configuration needed to stream your data at the level of your data throughput.

# Sending data to Amazon Kinesis Data Streams

* There are several ways to send data to Kinesis Data Streams, providing flexibility in the designs of your solutions.

* You can write code utilizing one of the AWS SDKs that are supported by multiple popular languages.

* You can use the Amazon Kinesis Agent, a tool for sending data to Kinesis Data Streams.

* The Amazon Kinesis Producer Library (KPL) simplifies the producer application development by enabling developers to achieve high write throughput to one or more Kinesis data streams.

* The KPL is an easy to use, highly configurable library that you install on your hosts. It acts as an intermediary between your producer application code and the Kinesis Streams API actions. For more information about the KPL and its ability to produce events synchronously and asynchronously with code examples, see Writing to your Kinesis Data Streams Using the KPL

* There are two different operations in the Kinesis Data Streams API that add data to a stream: PutRecords and PutRecord. The PutRecords operation sends multiple records to your stream per HTTP request while, PutRecord submits one record per HTTP request. To achieve higher throughput for most applications, use PutRecords.

* For more information about these APIs, see Adding Data to a Stream. The details for each API operation can be found in the Amazon Kinesis Data Streams API Reference.

# Processing data in Amazon Kinesis Data Streams

* To read and process data from Kinesis streams, you need to create a consumer application. There are varied ways to create consumers for Kinesis Data Streams. Some of these approaches include using Amazon Kinesis Data Analytics to analyze streaming data using KCL, using AWS Lambda, AWS Glue streaming ETL jobs, and using the Kinesis Data Streams API directly.

* Consumer applications for Kinesis Data Streams can be developed using the KCL, which helps you consume and process data from Kinesis Data Streams The KCL takes care of many of the complex tasks associated with distributed computing such as load balancing across multiple instances, responding to instance failures, checkpointing processed records, and reacting to resharding. The KCL enables you to focus on the writing record-processing logic. For more information on how to build your own KCL application, see Using the Kinesis Client Library.

* You can subscribe Lambda functions to automatically read batches of records off your Kinesis stream and process them if records are detected on the stream. AWS Lambda periodically polls the stream (once per second) for new records and when it detects new records, it invokes the Lambda function passing the new records as parameters. The Lambda function is only run when new records are detected. You can map a Lambda function to a shared-throughput consumer (standard iterator)

* You can build a consumer that uses a feature called enhanced fan-out when you require dedicated throughput that you do not want to contend with other consumers that are receiving data from the stream. This feature enables consumers to receive records from a stream with throughput of up to two MB of data per second per shard.

* For most cases, using Kinesis Data Analytics, KCL, AWS Glue, or AWS Lambda should be used to process data from a stream. However, if you prefer, you can create a consumer application from scratch using the Kinesis Data Streams API. The Kinesis Data Streams API provides the GetShardIterator and GetRecords methods to retrieve data from a stream.

* In this pull model, your code extracts data directly from the shards of the stream. For more information about writing your own consumer application using the API, see Developing Custom Consumers with Shared Throughput Using the AWS SDK for Java. Details about the API can be found in the Amazon Kinesis Data Streams API Reference.

# Processing streams of data with AWS Lambda

* AWS Lambda enables you to run code without provisioning or managing servers. With Lambda, you can run code for virtually any type of application or backend service with zero administration. Just upload your code, and Lambda takes care of everything required to run and scale your code with high availability. You can set up your code to automatically trigger from other AWS services, or call it directly from any web or mobile app.

* AWS Lambda integrates natively with Amazon Kinesis Data Streams . The polling, checkpointing, and error handling complexities are abstracted when you use this native integration. This allows the Lambda function code to focus on business logic processing.

* You can map a Lambda function to a shared-throughput (standard iterator), or to a dedicated-throughput consumer with enhanced fan-out. With a standard iterator, Lambda polls each shard in your Kinesis stream for records using HTTP protocol. To minimize latency and maximize read throughput, you can create a data stream consumer with enhanced fan-out. Stream consumers in this architecture get a dedicated connection to each shard without competing with other applications reading from the same stream. Amazon Kinesis Data Streams pushes records to Lambda over HTTP/2.

* By default, AWS Lambda invokes your function as soon as records are available in the stream. To buffer the records for batch scenarios, you can implement a batch window for up to five minutes at the event source. If your function returns an error, Lambda retries the batch until processing succeeds or the data expires.

# Summary

* Company "InternetProvider" leveraged Amazon Kinesis Data Streams to stream user details and location. The stream of record was consumed by AWS Lambda to enrich the data with bandwidth options stored in the function’s library. 

* After the enrichment, AWS Lambda published the bandwidth options back to the application. Amazon Kinesis Data Streams and AWS Lambda handled provisioning and management of servers, enabling company "InternetProvider" to focus more on business application development.

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

[Reference Guide](https://docs.aws.amazon.com/whitepapers/latest/streaming-data-solutions-amazon-kinesis/streaming-data-solutions-examples.html)