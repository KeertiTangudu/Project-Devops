# DevSecOps with AIOps and AWS

## Purpose and scope

These are learning notes about DevOps, DevSecOps, and AIOps. They explain
delivery and operations concepts; they do not document an implemented AWS
architecture, production deployment, automated remediation system, or measured
outcome in this repository.

The AWS portion of this topic is a future learning area. Any future AWS
implementation should identify the services used, required permissions, cost
considerations, validation evidence, and rollback procedure in its own project
documentation.

## Software development lifecycle

Software commonly moves through these connected phases before and after a
release:

1. **Requirements** — identify the user need and constraints.
2. **Planning and analysis** — decide the delivery approach, risks, timeline,
   and expected cost.
3. **Development** — implement the change.
4. **Testing** — verify that the change behaves as expected.
5. **Deployment or handover** — release the change and provide the context
   needed to support it.
6. **Maintenance** — correct defects, manage dependencies, and improve the
   product over time.

### Waterfall and Agile

In a traditional waterfall approach, work is completed in large, sequential
phases. Feedback from testing or users can arrive late, making incorrect
assumptions and defects more expensive to address.

Agile delivery reduces that feedback delay by organizing work into small,
time-boxed iterations, often called sprints. Teams can demonstrate a working
increment, gather feedback, and adapt the next iteration. Daily coordination
and sprint reviews are examples of practices that help shorten feedback loops.

Agile alone does not necessarily integrate the operational team into delivery.
DevOps addresses that gap by treating development, delivery, and operations as
shared responsibilities.

## DevOps

DevOps is a collaborative engineering practice that connects development and
operations through shared ownership, automation, and rapid feedback. For a
small code change, a delivery pipeline can build the software, run relevant
tests and security checks, package an artifact, and deploy it to an appropriate
environment according to defined controls.

A typical continuous delivery flow is:

1. **Build** — compile or package the change into a reproducible artifact.
2. **Scan** — evaluate source code, dependencies, images, or configuration as
   appropriate to the change.
3. **Test** — run automated checks that cover expected and invalid input.
4. **Deploy** — promote the validated artifact using an approved deployment
   process.
5. **Feedback** — make failures and operational signals visible to the people
   responsible for the change.

Automation does not remove the need for engineering review. Release controls,
security requirements, environment boundaries, and rollback plans should match
the risk of the workload.

## DevSecOps

DevSecOps integrates security work throughout the delivery lifecycle instead
of postponing it until the end of a release. The objective is to make secure
delivery a shared responsibility and to provide actionable feedback early.

Examples of security controls include:

- **Static application security testing (SAST)** for source-code findings.
- **Software composition analysis (SCA)** for known dependency
  vulnerabilities and license concerns.
- **Dynamic application security testing (DAST)** for a running application in
  a safe, authorized environment.
- **Infrastructure-as-code and configuration scanning** for common
  misconfigurations before deployment.
- **Secrets detection** to prevent credentials and sensitive values from
  entering source control.

Security findings require triage: a tool result is not automatically a
confirmed vulnerability, and remediation priority should consider context,
exploitability, and impact.

## AIOps

AIOps applies data analysis and machine learning techniques to operational
signals such as metrics, logs, traces, events, and alerts. It can complement
traditional monitoring, rather than replace sound instrumentation, clear
service ownership, and human review.

### Anomaly detection

Threshold alerts identify known conditions, such as sustained CPU utilization
above a defined level. Anomaly detection instead compares a signal with an
expected baseline, which may vary by time of day or day of week. This can help
identify unusual behavior that does not cross a static threshold.

### Predictive alerting

Trend analysis can estimate whether a resource is likely to reach a defined
limit. When the model and input data are reliable, advance warning can give
operators time to investigate capacity, reduce load, or prepare a controlled
scaling action. Predictions should be monitored for false positives and false
negatives before they influence critical decisions.

### Automated remediation

For well-understood and low-risk failure modes, a system may execute a
pre-approved remediation, such as restarting an unhealthy process, scaling a
service within tested limits, or rolling back a failed deployment. Automation
should be bounded by least-privilege permissions, safeguards against repeated
actions, audit logs, alerting, and an operator override. It is not appropriate
to automate an action solely because an anomaly was detected.

## AWS learning considerations

An AWS-based learning project in this area should keep application delivery,
security scanning, observability, and remediation controls separate and
explicit. Before implementation, document at least:

- the services and regions used, ownership boundaries, and data flows;
- IAM roles and least-privilege permissions;
- encryption, secrets management, and log-data handling;
- alert conditions, anomaly or prediction inputs, and expected operator
  response;
- safeguards, approval boundaries, and rollback behavior for automated
  actions; and
- expected costs, budget alerts, and teardown steps.

No AWS services are configured by this note.

## References

- [AWS: What is DevOps?](https://aws.amazon.com/devops/what-is-devops/)
- [AWS: What is DevSecOps?](https://aws.amazon.com/what-is/devsecops/)
- [AWS: What is AIOps?](https://aws.amazon.com/what-is/aiops/)
- [NIST Secure Software Development Framework (SSDF)](https://csrc.nist.gov/Projects/ssdf)
