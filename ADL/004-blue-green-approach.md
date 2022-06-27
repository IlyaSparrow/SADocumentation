# Blue-Green approach

Date: 2020-06-24

Author: Ilya Vorobyeu

## Status
Accepted

## Context

- Blue Green CI/CD approach.

## Decision

- Implement Blue Green approach for event service;
- Blue Green approach will allow to deploy without downtimes;
- In tearm of big number of requests to event service we are expecting hight performance in term of big system load;
- Blue Green approach will allow to exclude downtimes;
- Service should be synchronized with more than one pod instance, it will allow to exclude performance issues and deligate load from one pod to another;
- Primary and secondary cluster should be configured in Cloud. Database Replications must be preconfigured between regions. Secondary cluster database must have replication from the Primary cluster;

## Consequences

Implementation of the additional datastorage needed. Replication issues, based on the used cloud services.