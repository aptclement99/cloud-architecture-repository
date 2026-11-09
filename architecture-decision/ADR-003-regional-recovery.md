ADR-003: Regional Recovery Strategy

Status: Proposed
Date: November 2026
Project: Global E-Commerce Modernization

1. Context

The Global E-Commerce platform serves customers across 47 countries. A regional AWS outage could disrupt customer access, order processing, and other critical business operations.

The architecture requires a disaster recovery strategy that balances availability, recovery speed, operational complexity, and infrastructure cost.

The proposed deployment uses:

* Primary Region: US East (N. Virginia) — us-east-1
* Recovery Region: Europe (Ireland) — eu-west-1

The final regional selection must be validated against data residency, regulatory requirements, service availability, network latency, and cost.

2. Decision

Adopt a primary-region architecture with a separately provisioned recovery region as the initial disaster recovery strategy.

The primary region will handle normal production workloads. The recovery region will maintain the resources and replicated data necessary to restore critical services during a major regional failure.

Active-active multi-region deployment will not be adopted initially because its additional complexity and cost are not justified without demonstrated business requirements.

3. Recovery Strategy

The proposed recovery approach includes:

* Replicate critical application data to the recovery region.
* Maintain infrastructure definitions through Infrastructure as Code.
* Store deployment artifacts in a way that makes them accessible during regional recovery.
* Establish documented failover and failback procedures.
* Monitor replication health and recovery-region readiness.
* Conduct periodic disaster recovery exercises.

Critical application dependencies, DNS changes, security configurations, secrets, and third-party integrations must also be included in recovery planning.

4. Recovery Objectives

The architecture must define and validate:

* Recovery Time Objective (RTO): Maximum acceptable time to restore critical services.
* Recovery Point Objective (RPO): Maximum acceptable amount of data loss measured in time.

Specific RTO and RPO targets will be agreed with business stakeholders based on the criticality of each workload.

These objectives must be demonstrated through recovery testing rather than assumed from the architecture diagram.

5. Alternatives Considered

Option A: Single Region Only

Rejected because a major regional failure could make the entire platform unavailable.

Option B: Active-Active Multi-Region

Not selected initially because it introduces additional complexity around data consistency, traffic management, deployment coordination, and operational costs.

It may be reconsidered if business requirements demand continuous service across regional failures.

Option C: Primary Region with Recovery Region

Selected because it provides a structured disaster recovery path while maintaining a more manageable operational model.

6. Consequences and Trade-offs

Benefits

* Reduced dependence on a single AWS region.
* Clearer disaster recovery procedures.
* Lower initial complexity than active-active architecture.
* Better alignment between recovery investment and business requirements.

Trade-offs

* Additional infrastructure and data replication costs.
* Potential replication lag and data loss within the agreed RPO.
* Recovery procedures require regular testing.
* Failover and failback may involve temporary service disruption.
* Some services may require manual intervention during recovery.

7. Validation Requirements

Before production adoption, the team must:

1. Validate data replication and consistency.
2. Test application startup in the recovery region.
3. Simulate primary-region failure.
4. Measure actual RTO and RPO.
5. Test DNS and traffic-routing changes.
6. Validate security, secrets, and third-party dependencies.
7. Document failback procedures.
8. Review recovery costs against business impact.

8. Final Position

A primary-region architecture supported by a separately provisioned recovery region is the preferred initial strategy for the Global E-Commerce platform.

The design prioritizes business continuity, manageable complexity, and cost awareness.

The strategy will be reviewed as the platform grows and as business availability requirements evolve.
