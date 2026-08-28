# DevOps and SRE Engineering Focus

## Purpose and evidence boundary

This document records my intended Senior DevOps Engineering and Site
Reliability Engineering (SRE) learning focus. It describes the principles,
concepts, and future areas of work that I plan to apply to hands-on projects
in this repository.

It is not evidence that I have implemented, operated, or used a particular
platform, cloud service, monitoring tool, alerting workflow, reliability
target, or automated remediation mechanism. Future projects will document
their own scope, implementation, validation, and limitations.

## Engineering lifecycle

I view delivery as a continuous lifecycle rather than a sequence that ends at
release. My intended lifecycle is:

1. **Requirements Gathering** — understand functional needs, stakeholders,
   constraints, and non-functional expectations such as reliability, security,
   cost, and performance.
2. **Planning & Analysis** — assess architecture choices, dependencies,
   capacity, operational risks, security controls, cost trade-offs, and
   rollback options before implementation.
3. **Development & Automation** — create repeatable, version-controlled
   application and infrastructure changes with clear configuration and
   ownership.
4. **Testing** — validate functional behavior along with security, reliability,
   performance, and deployment assumptions using appropriate automated and
   manual checks.
5. **Handover** — provide the documentation, operational context,
   observability, and recovery information needed for a service to be
   supportable.
6. **Maintenance** — monitor outcomes, manage changes and dependencies,
   address operational findings, and use feedback to improve the next cycle.

## Continuous DevOps practice

I see DevOps as a continuous engineering practice that connects development,
building, security scanning, testing, operational handover, monitoring, and
improvement. Each stage should produce useful feedback for the next one rather
than operate in isolation.

For future projects, I intend to treat automation, testing, and security
scanning as planned quality controls. The exact tools and configurations will
be selected, implemented, and validated within the scope of each project;
their inclusion here is not a claim of current implementation.

## Core engineering priorities

### Cost optimization

I aim to evaluate cost as a design concern throughout the lifecycle. This
includes selecting resources deliberately, understanding demand and capacity,
measuring cost drivers, avoiding unnecessary consumption, and documenting
trade-offs between cost, resilience, security, and performance.

### Security

I aim to build security into design and delivery through secure defaults,
least-privilege access, protected secrets, controlled changes, and meaningful
validation. Future work should consider dependency and image scanning where
applicable, while ensuring sensitive values are never committed to this
repository.

### High availability

I aim to design for expected failures by considering redundancy, health
signals, dependency failures, recovery procedures, and relevant failure
domains. Availability decisions should be based on documented requirements and
tested assumptions, not on unsupported claims of production resilience.

### Scalability

I aim to consider expected load, resource limits, bottlenecks, capacity
signals, and scaling approaches early in design. A scaling decision should be
observable and appropriate to the workload rather than assumed to be
universally beneficial.

## Reliability engineering

I am building my understanding of reliability engineering through service
level indicators (SLIs), service level objectives (SLOs), and service level
agreements (SLAs):

- An **SLI** is a quantitative measurement of a service behavior that matters
  to its users, such as successful request rate or latency.
- An **SLO** is a target level for an SLI over a defined period.
- An **SLA** is an agreement that communicates a service commitment and may
  include consequences when that commitment is not met.

I intend to use these concepts to make reliability expectations measurable and
to guide engineering trade-offs. Error budgets are also a useful planning
concept: they can help balance reliability work and delivery pace when an SLO
has been defined. This repository does not currently establish service-level
targets, error budgets, or contractual commitments.

## Monitoring and observability learning focus

I am developing an observability-focused mindset that includes metrics, logs,
traces, events, dashboards, alert routing, and actionable operational
documentation. The objective is to understand service behavior, identify
meaningful symptoms, support investigation, and enable an appropriate
response.

My cloud monitoring learning focus includes AWS CloudWatch. I also plan to
evaluate PagerDuty, Datadog, and New Relic as examples of incident-management
and observability platforms in future hands-on projects. These named services
are learning and evaluation topics only; this document does not claim that I
have configured, administered, or operated them.

## Future AI-assisted operations focus

I am interested in learning how AI-assisted operations can support predictive
alerting, anomaly detection, alert correlation, alert-noise reduction, and
carefully controlled automated remediation. These approaches should complement
sound observability and operational practices rather than replace engineering
judgment.

Any future automated remediation design should have explicit safeguards,
including least-privilege permissions, defined scope, auditable execution,
validation in lower-risk environments, clear rollback paths, and human review
or approval for high-impact actions. I will treat automation as a hypothesis to
test and document, not as an assumed substitute for incident response or
service ownership.

## Expectations for future repository work

When I add a project, lab, or scenario, I intend to document the work with a
clear purpose and scope, implementation details, prerequisites, validation
results, operational considerations, limitations, and cleanup or rollback steps
where relevant. Only those project-specific records should be treated as
evidence of demonstrated implementation.

## Official references

- [AWS CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
- [PagerDuty documentation](https://support.pagerduty.com/main/docs/what-is-pagerduty)
- [Datadog documentation](https://docs.datadoghq.com/getting_started/)
- [New Relic documentation](https://docs.newrelic.com/docs/new-relic-solutions/)
- [Google SRE Book: Service Level Objectives](https://sre.google/sre-book/service-level-objectives/)
