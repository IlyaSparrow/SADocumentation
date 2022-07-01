## Design Considerations 

## Status
Accepted

## Background

Development team is going to elaborate REST Service manipulating Event resource.

Event entity contains following fields:
-   id;
-   title;
-   place;
-   speaker;
-   eventType;
-   dateTime.

## Glossary

- AWS - Amazone Web Service;
- CRUD - Create Read Update Delete;

## Functional requirements
- CRUD operations;
- Filtering;
- Search;
- Sorting;
- Aggregation by specified field value.

## Non-functional requirements

1) Capacity...;
2) Number of critical failures...;
3) Quality of performance...;


## Constraints

- Use Amazon Web Services Cloud Provider deployment distributed between following regions: eu-west, eu-east;
- Cloud provider should be consistent.

## Quality attributes

- Data Storage should be provide calculation of query against 100 000 000 entries within ~700ms.

## Solution Options

- MySQL DB deployed to on-premis;
- AWS RDS Aurora MySQL DB;
- ElasticSearch NoSQL storage.

## Decision criteria
- Hosting and maintenance pricing should be optimal.
- Data storage should provide detailed logging for troubleshooting and process verification.
- Data storage for REST service should provide support multi-tenant content storage.
- Event Model to Data Storage Schema mapping complexity should be minimized.
- Instance of storage service should host several independent databases with tenant specific content.
- Data storage for Legal Analytics service should provide automated data backup every 30 days.

## Decision

| Requirenments  	| MySQL DB deployed to on-premis  	|  AWS RDS Aurora MySQL DB 	| ElasticSearch NoSQL storage  	|
|----------------:|----------------------------------:|--------------------------:|------------------------------:|
| Consistency     |                v                  |            v              |              v                |
| Structured      |                v                  |            v              |              x                |
| Maintaince      |                x                  |            v              |              v                |
| Reporting       |                x                  |            v              |              v                |
| Alerting        |                x                  |            v              |              v                |
| Security        |                x                  |            v              |              v                |
| Multi-tenant content storage|    x                  |            v              |              v                |
| Replication supported |          x                  |            v              |              v                |
| Schema mapping complexity|       x                  |            v              |              N/A              |
| Automated data backup|           x                  |            v              |              v                |
| Performance     | depends on the service deployed   | within 4.32 s for 1m rows | based on the number of nodes, around 1s for 1m|
| Pricing         | depends on the service deployed   | eu-east(0.1$ GB, 0.2$ 1m req.)|AWS EC2 Kubernetes|
|                 |                                   | eu-west-Oregon(0.1$ -GB, 0.2$ 1m req.) |

### MySQL DB deployed to on-premis
Database deployed on-premis requires hight qality of maintanence and it is difficult to troubleshoot such type of data storage.
Additional service might be implemented for reporting, troubleshooting, back-ups and replications. 
This type of database can't be applied in Event Service API baseed on the provided requirenments.

### AWS RDS Aurora MySQL DB
Performance formulas for MySQL database: 
log(row_count) / log(index_block_length / 3 * 2 / (index_length + data_pointer_length)) + 1;
index is around 1.024 bytes;
data pointer is around 4 bytes;
key value length(MEDIUMINT) 3 bytes;
log(1000000)/log(1024/3*2/(3+4)) + 1 = 4.32 s;

### ElasticSearch NoSQL storage
Requires deployed cluster, possible way - AWS EC2, pricing depends on the number of included properties/services(API Getway, VPC and e.c.);
To meet pricing and performance expectations calculations of the aavg data per/second required.

### Corns 
|ElasticSearch NoSQL storage | AWS RDS Aurora MySQL DB |
|-------------------|-----------------------------------|
|Joining data requires duplicate de-normalized documents that make parent child relationships. It is hard and requires a lot of synchronizations
Tracking errors in the data in the logs can be hard, and sometimes recurring errors blow up the error logs
Schema changes require complete reindexing of an index | MySQL doesn't provide good data wrangling functionalities, such as parsing JSON or XML. We had to transform them outside MySQL on the web application server side using JSP.
As we move forward to adopt more genomics information, MySQL may lack of dealing with "big data" functionalities.
It is a freely available S/W and easy to manage budget, but there are possibilities to spend cost for additional technical support. |


### Final Decision
For event service possible to follow both: ACID principals and CAP theorem, that's why Event Service can store data in both, MySQL and NoSQL;
MySQL DB deployed to on-premis does not meet reqironments;

AWS RDS Aurora MySQL DB meets almost all requirenments exclude the big size and time for request execution. In case of the service it is possible to have more than 1000000 rows and to be able to save performance on the appropriate lvl - Additional payments required for AWS to improve RDS plan;

ElasticSearch NoSQL storage meets all requirenments and can be used as an service Datastorage engine. 

## Sources
- Amazon Web Services Cloud Provider;
- AWS RDS Aurora MySQL DB.

## Tickets

1) Jira tickets

## References

[MySQL DB deployed to on-premis](https://www.oracle.com/mysql/)

[AWS RDS Aurora MySQL DB](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.AuroraMySQL.html)
[MySQL Integer types](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)
[MySQL performance calculation](https://dev.mysql.com/doc/refman/8.0/en/estimating-performance.html)

[ElasticSearch NoSQL storage](https://www.elastic.co/elasticsearch/features)
[Performance Calculations](https://www.elastic.co/blog/benchmarking-and-sizing-your-elasticsearch-cluster-for-logs-and-metrics)