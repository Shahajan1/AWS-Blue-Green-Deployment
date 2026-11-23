# Project Overview

## Objective

Build two independently identifiable web application environments on AWS and use an Application Load Balancer to direct traffic between them.

- **Blue:** Version 1, hosted on `Blue-Server`.
- **Green:** Version 2, hosted on `Green-Server`.

The rollout sequence is 100% Blue, then weighted 80/20 Blue/Green, then 100% Green.

## Why Blue-Green?

Blue-Green deployment maintains two application environments so a new version can be prepared and validated separately from the current version. Traffic can then be directed to the new environment. Keeping the old environment available during validation can support a rollback decision.

This lab adds a weighted intermediate stage. That stage is described as canary-style traffic shifting.

## Scope

This is a manually configured AWS console lab. It does not include automated CI/CD, infrastructure as code, automated rollback, HTTPS, or production-grade private-subnet architecture.
