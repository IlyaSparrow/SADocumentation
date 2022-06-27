# Test containers API

Date: 2020-06-24

Author: Ilya Vorobyeu

## Status
Accepted
## Context

- Implementation of test containers in projects stracture;
- Integrate projects with test containers docker image;
- Configure local env docker configurations for integration tests;
- Configure docker test container into CI/CD.

## Decision

- Introduct development team to use docker test containers, it will allow to make application more flexible for different platforms;
- Test containers will abstract code part from database part of the project, like embeded databases or some local server implementations. This will help to save developers time and abstract from OS;
- Test containers should be used for local application run and integration tests;
- Team should be able to install DockerDesktop on there local machines, this might require to have a licencies;
- CI/CD processes should be updated with implementation of Test Conteiners;

## Consequences

Docker compose file needs to be prepared. May require additional resources in CI/CD workflow to be able to create additional image for test container instance;

