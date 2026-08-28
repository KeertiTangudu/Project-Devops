# AWS and Linux Foundations

## Purpose and evidence boundary

This document organizes handwritten study notes covering foundational AWS,
Linux, networking, authentication, and SSH concepts. It records concepts under
study; it is not evidence that I have deployed AWS resources, administered
Linux systems, configured security groups, or operated SSH in a production
environment.

The notes have been structured for clarity. Where a handwritten statement was
incomplete, ambiguous, or technically imprecise, the original learning intent
is retained and a clearly identified clarification is provided. No real keys,
hostnames, usernames, IP addresses, or credentials are included.

## AWS global infrastructure

### Regions and Availability Zones

The study notes introduce AWS Regions and Availability Zones (AZs), using US
East and US West examples. An AWS Region is a physical geographic area, and an
Availability Zone is an isolated location within a Region. A Region contains
multiple AZs. [AWS Regions and Availability Zones](https://docs.aws.amazon.com/global-infrastructure/latest/regions/aws-regions-availability-zones.html)
provide the current AWS terminology and regional infrastructure details.

| Study-note example | Organized interpretation |
| --- | --- |
| `us-east-1a` | An Availability Zone identifier in the `us-east-1` Region. |
| `us-west-1b` | An Availability Zone identifier in the `us-west-1` Region. |

**Clarification:** The examples above are AZ identifiers, while `us-east-1`
and `us-west-1` are Region identifiers. The notes' statement that a Region has
many Availability Zones is retained as the concept that each Region has
multiple AZs. This document does not state a fixed AZ count because it varies
by Region and can change.

## Operating systems and Linux foundations

### Operating systems, hardware, and the kernel

The notes connect an IP-enabled system with an operating system and identify
RAM, hard disks, and processors as hardware resources. They also describe the
operating system as a bridge between the user and hardware.

**Clarification:** An IP-enabled device normally has software or firmware that
supports networking, but not every such device necessarily has a general-purpose
operating system. In a Linux system, the kernel is the core component that
mediates access to hardware for software running on the system. The
[Linux kernel documentation](https://www.kernel.org/doc/html/latest/) is the
upstream reference for kernel concepts.

### Linux and distributions

The notes identify Linux as open source, developed using the C language and
Unix principles, and used for long-running servers. They also identify Linux
as a kernel associated with multiple operating-system distributions.

Examples recorded in the notes are:

- Oracle Linux
- Red Hat Enterprise Linux (RHEL)
- Debian

The notes describe RHEL as mostly used in enterprise organizations.

**Clarifications:**

- Linux is a kernel; a Linux distribution combines the kernel with user-space
  tools, libraries, package management, and other software.
- RHEL is commonly used in enterprise environments. This is not evidence of
  use in a particular organization or environment.
- The handwritten equivalence `CentOS = AWS Linux = Fedora` is not retained as
  a factual statement. CentOS, Amazon Linux, and Fedora are distinct
  distributions or projects, although some have historical or ecosystem
  relationships.

### Characteristics recorded in the notes

The notes list Linux as more secure, open source, suitable for long-running
servers, and associated with low power consumption, low bandwidth, and less
resource consumption.

**Clarifications:** Security depends on configuration, permissions, patching,
and operating practices; Linux is not automatically secure in every
deployment. A Linux installation's resource use depends on its distribution,
installed services, configuration, and workload. The phrase “low bandwidth” is
ambiguous in the source notes: Linux itself does not inherently require low
network bandwidth. It may refer to remote server administration or to a
lightweight installation, but that intended meaning needs confirmation.

## Authentication, network access, and protocols

### Authentication

The notes list several authentication methods or factors:

- username and password
- tokens
- public-key and private-key authentication
- biometric examples such as retina and fingerprint

For key-based authentication, the public and private keys form a key pair. The
public key may be authorized on a target system; the private key remains secret
with the user or client. Private keys must never be committed to this
repository.

### Firewalls and security groups

The notes associate firewalls with inbound and outbound authorized access, and
associate a firewall or security group with allowing SSH on port `22` from a
specific IP address.

**Clarification:** A firewall applies rules that allow or deny inbound and/or
outbound network traffic; it does not itself authenticate a user. A security
group is an AWS network access-control construct with similar traffic-filtering
intent, but it is not a universal synonym for every type of firewall. See the
[Amazon VPC security groups documentation](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html).

### Protocols, IP addresses, and ports

The notes connect a protocol, IP address, port number, and authentication as
connection concepts. They name HTTP (Hypertext Transfer Protocol) and HTTPS,
and list FTP, SSH, SFTP, UDP, TCP, and SMTP as protocol examples.

**Clarifications:** FTP, SSH, SFTP, SMTP, TCP, and UDP are network protocols,
but they are not examples of HTTP. Port numbers from `0` through `65535` are
used with TCP and UDP transport communication; not every network protocol maps
to a port in the same way. The range is a port-number range, not a property of
a laptop.

An HTTPS URL can explicitly show its port, for example:

```text
https://example.invalid:443
```

This safe placeholder preserves the source-note concept without documenting a
real external endpoint.

## SSH and terminal access

The notes define SSH as Secure Shell, a protocol used to connect to a Linux
server through terminal access. SSH commonly uses port `22` by default. The
[OpenSSH `ssh` manual](https://man.openbsd.org/ssh) documents the client and
connection options.

### Key generation and connection patterns

The following source-note command patterns are retained with placeholders:

```bash
ssh-keygen -f <filename>
ssh -i <private-key-file> <username>@<public-ip-address>
```

`ssh-keygen -f <filename>` specifies a filename or path for generated key
material. The [OpenSSH `ssh-keygen` manual](https://man.openbsd.org/ssh-keygen)
documents its options. The SSH connection pattern specifies an identity file,
a username, and a public IP address. It is an example pattern only; it does not
represent a configured connection or a deployed server.

**Safety note:** Keep private-key files confidential. Do not upload, commit,
or share them.

## Linux commands and user privileges

The notes record these Linux commands:

| Command | Study-note concept |
| --- | --- |
| `pwd` | Print the present working directory. |
| `cd` | Change directory. |
| `uname -a` | Display system information. |
| `sudo` | Run a permitted command with elevated privileges. |
| `su -` | Start a login shell as another user, commonly root. |

The notes distinguish normal users from administrator users.

**Clarification:** Administrator access should be limited to authorized users
and only used when necessary. `sudo` and `su -` have different behavior and
both require appropriate authorization. Consult the local system documentation
before using privileged commands on a system.

## Items retained for confirmation

The following source-note wording remains incomplete or ambiguous and should
be confirmed before it is expanded further:

1. The intended meaning of “low bandwidth” as a Linux characteristic.
2. Whether “embedded Linux” was intended as an example of an operating system
   or as a general description of the operating system-to-hardware boundary.
3. Whether any fixed number of Availability Zones was intended by the US East
   and US West examples.
4. Whether the notes intended an additional distinction between normal users,
   administrator users, and the root account beyond the listed `sudo` and
   `su -` commands.

## Official references

- [AWS Regions and Availability Zones](https://docs.aws.amazon.com/global-infrastructure/latest/regions/aws-regions-availability-zones.html)
- [Amazon VPC security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [Linux kernel documentation](https://www.kernel.org/doc/html/latest/)
- [OpenSSH `ssh` manual](https://man.openbsd.org/ssh)
- [OpenSSH `ssh-keygen` manual](https://man.openbsd.org/ssh-keygen)
