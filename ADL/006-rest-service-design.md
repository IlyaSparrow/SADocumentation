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
4) Use Amazon Web Services Cloud Provider deployment distributed between following regions: eu-west, eu-east;
5) Cloud provider should be consistent.

## Constraints

N/A

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

### SQL or NoSQL

### MySQL DB deployed to on-premis

### AWS RDS Aurora MySQL DB

### ElasticSearch NoSQL storage


## Sources
- Amazon Web Services Cloud Provider;
- AWS RDS Aurora MySQL DB.

## Tickets

1) Jira tickets

## References
N/A
