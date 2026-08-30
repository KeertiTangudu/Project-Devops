# Linux and SSH Assignment Guide

## Purpose and safety boundary

This is a step-by-step **practice guide** for the supplied Linux and DevOps
Basics assignments. Commands are written for an Amazon Linux 2023 EC2 lab and
must be run only against a disposable instance and AWS account that the learner
is authorized to use. The guide does not claim that any AWS resource has been
created or that any command has been executed.

Do not commit private keys, passwords, public IP addresses, account IDs, or SSH
configuration containing real hosts. Replace angle-bracket values with lab
values at execution time. Commands marked `sudo` change the instance and should
be reviewed before running.

## Prerequisites

- An AWS account with permission to create and delete EC2 instances, key pairs,
  and security groups.
- A local terminal with OpenSSH, `curl`, and `wget` installed.
- The current public IPv4 address of the client that will connect to the
  instance.
- An Amazon Linux 2023 instance and its default `ec2-user` account.

> **Cost and cleanup:** An EC2 instance can incur charges while it exists.
> Complete the cleanup steps in the first assignment when the lab is finished.

## 1. Linux and SSH setup

### 1. Launch the instance and create a key pair

1. In the AWS Console, select the **Asia Pacific (Hyderabad)** Region
   (`ap-south-2`).
2. Open **EC2** → **Instances** → **Launch instances**.
3. Name the instance with a non-sensitive lab name, select **Amazon Linux
   2023**, choose an instance type suitable for the lab, and create or select a
   key pair.
4. Create a security group that permits inbound TCP port `22` temporarily from
   the client public IP only. If the assignment starts with `0.0.0.0/0`, change
   it immediately in [step 3](#3-restrict-ssh-to-the-client-ip).
5. Launch the instance and wait until its status checks pass.

If a key is generated **locally** with `ssh-keygen`, it creates two files:

```text
lab-ec2-key       # private key: keep secret; use it to prove identity
lab-ec2-key.pub   # public key: safe to install on the server
```

The private key must remain on the local client and should normally have mode
`600`. The public key is copied into the server user's
`~/.ssh/authorized_keys`. When creating an EC2 key pair in the console, AWS
downloads the private key file (often `.pem`) once and retains the public-key
material for instance launch; it does not necessarily download two local files.

### 2. Connect and confirm the session

On the local computer, restrict the private-key permissions and connect:

```bash
chmod 600 <path-to-private-key.pem>
ssh -i <path-to-private-key.pem> ec2-user@<instance-public-dns-or-ip>
```

After connecting, confirm identity, host, and location:

```bash
hostname
pwd
whoami
```

Expected characteristics are an instance hostname, a home directory such as
`/home/ec2-user`, and the username `ec2-user`. Values vary by lab and should
not be copied into repository documentation if they identify a real host.

### 3. Restrict SSH to the client IP

1. In **EC2** → **Security Groups**, select the security group attached to the
   instance.
2. Choose **Edit inbound rules**.
3. For the SSH/TCP/22 rule, replace the `Anywhere-IPv4` source (`0.0.0.0/0`)
   with the single client address in CIDR form: `<client-public-ip>/32`.
4. Save the rule, keep the existing SSH session open, and verify a new SSH
   connection succeeds from the permitted network.

A `/32` IPv4 CIDR contains exactly one address. If the client network changes,
update the rule before disconnecting. Avoid opening SSH to the internet merely
to restore access.

### 4. Identify the distribution and version

On the instance, run:

```bash
cat /etc/os-release
uname -r
```

`/etc/os-release` reports the distribution identity and version; `uname -r`
reports the running kernel release. For this lab, the distribution output should
identify Amazon Linux 2023, while the kernel value can differ after updates.

### 5. Remove the lab resources

1. Copy any non-sensitive notes needed for the assignment off the instance.
2. In EC2, select the instance and choose **Instance state** → **Terminate
   instance**; confirm termination.
3. After the instance is terminated, delete the lab security group. A security
   group cannot be deleted while attached to an instance or referenced by
   another rule.
4. Delete the EC2 key-pair record from **Key Pairs** and securely remove its
   downloaded private key from the local computer if it is no longer needed.

Terminating an instance is destructive. Do not terminate an instance unless its
identity and data-retention requirements have been verified.

## 2. Linux file operations

Run these in a disposable working directory, not in a system directory:

```bash
# 1. Create the nested directories in one command.
mkdir -p ~/devops/projects/batch-90s

# 2. Write three lines, append two, and verify them.
printf '%s\n' 'line one' 'line two' 'line three' > ~/devops/projects/batch-90s/notes.txt
printf '%s\n' 'line four' 'line five' >> ~/devops/projects/batch-90s/notes.txt
cat ~/devops/projects/batch-90s/notes.txt

# 3. Copy under the requested new name and verify both files.
cp ~/devops/projects/batch-90s/notes.txt ~/devops/projects/backup.txt
find ~/devops/projects -maxdepth 2 -type f -printf '%p\n'

# 4. Move the backup file to /tmp and verify it.
mv ~/devops/projects/backup.txt /tmp/backup.txt
stat /tmp/backup.txt

# 5. Remove only the lab tree, then verify it is gone.
rm -rf ~/devops
[ ! -e ~/devops ] && echo 'devops lab directory removed'

# 6. List /var/log entries newest first.
ls -lt /var/log/
```

Files under `/tmp` are temporary: their lifetime is managed by the operating
system and local cleanup policy, so they must not be relied upon after reboot
or after a configured cleanup interval. `ls -lt /var/log/` typically shows that
logs have different modification times. A DevOps engineer checks this directory
to investigate errors, service starts and stops, authentication events, and
resource or application symptoms. Log availability and names vary by service
and distribution.

## 3. Linux text processing

Create the input file without manually aligning columns; whitespace-aware tools
handle the extra spaces:

```bash
cat > servers.txt <<'EOF_SERVERS'
web-server-01 192.168.1.10 running
web-server-02 192.168.1.11 stopped
db-server-01  192.168.1.20 running
db-server-02  192.168.1.21 running
cache-server  192.168.1.30 stopped
EOF_SERVERS
```

| Task | Command |
| --- | --- |
| 1. Show running servers | `awk '$3 == "running"' servers.txt` |
| 2. Show non-running servers | `awk '$3 != "running"' servers.txt` |
| 3. Count running servers | `awk '$3 == "running" { count++ } END { print count + 0 }' servers.txt` |
| 4. First three, then last two | `head -n 3 servers.txt; tail -n 2 servers.txt` |
| 5. IP addresses only | `awk '{ print $2 }' servers.txt` |
| 6. Server names only | `awk '{ print $1 }' servers.txt` |
| 7. Names of stopped servers | `awk '$3 == "stopped" { print $1 }' servers.txt` |

In `awk`, fields are whitespace-separated by default, so `$1`, `$2`, and `$3`
refer to the name, IP address, and status respectively.

## 4. `wget`, `curl`, and pipes

Set the public assignment URL once for the current shell:

```bash
url='https://raw.githubusercontent.com/daws-90s/notes/refs/heads/main/session-02.txt'
```

Then complete each task:

```bash
# 1. Download and name the file.
wget -O session-notes.txt "$url"

# 2. Fetch without saving and count lines.
curl --fail --silent --show-error "$url" | wc -l

# 3. Fetch and print lines containing Linux.
curl --fail --silent --show-error "$url" | grep 'Linux'

# 4. Print the final slash-separated field from the URL.
printf '%s\n' "$url" | awk -F '/' '{ print $NF }'

# 5. Use two pipes: fetch, filter case-insensitively for ssh, and count matches.
curl --fail --silent --show-error "$url" | grep -i 'ssh' | wc -l
```

In `awk`, `-F '/'` sets `/` as the field separator and `$NF` means the last
field (`NF` is the total number of fields). The URL's last field is
`session-02.txt`. `curl --fail` returns a nonzero status for HTTP errors, and
`--show-error` keeps error diagnostics visible while successful output remains
suitable for pipes.

## 5. `/etc/passwd` deep dive

`/etc/passwd` is world-readable account metadata; it does not hold modern
password hashes. Run:

```bash
# 1. Number of account entries.
wc -l < /etc/passwd

# 2. Usernames only.
cut -d: -f1 /etc/passwd

# 3. Accounts with UID greater than 999.
awk -F: '$3 > 999 { print $1 ":" $3 }' /etc/passwd

# 4. Locate ec2-user and show its home directory.
getent passwd ec2-user | awk -F: '{ print $6 }'

# 5. Unique assigned login shells.
cut -d: -f7 /etc/passwd | sort -u
```

The exact account count, UID threshold interpretation, home directory, and
shell list depend on the instance image and any accounts added during the lab.
`getent` is used for the `ec2-user` lookup because it follows configured name
service sources rather than assuming only local files.

## 6. Vim editor

1. Start the editor: `vim practice.txt`.
2. Press `i` to enter Insert mode, type ten lines, then press `Esc` and enter
   `:wq` to write and quit.
3. Reopen with `vim practice.txt`. Enter `:set number`, then `7G` to jump to
   line 7. Enter `3Gdd` to jump to and delete line 3. Save and exit with
   `:wq`.
4. Search with `/word` then press `Enter`; use `n` for the next match and `N`
   for the previous match. Clear highlighting with `:nohlsearch`.
5. Replace every occurrence in the file with
   `:%s/old-word/new-word/g`. Confirm the replacement before saving if the text
   is important.
6. Go to the final line with `G`, press `o`, add three lines, then press `Esc`
   and `u` once to undo the last change. Save with `:w`.
7. Copy the current line with `yy`, then paste it five times in one command
   with `5p`.

## 7. Linux user management

These exercises require `sudo` and change authentication and authorization on
the instance. Perform them only on the disposable lab instance and keep an
existing administrator session open while testing SSH changes.

```bash
# 1. Create alice; the usual user-private group is also named alice.
sudo useradd --create-home alice
id alice

# 2. Set a password (the prompt does not echo it).
sudo passwd alice

# 3. Create the team and add alice as a supplementary member.
sudo groupadd devops-team
sudo usermod --append --groups devops-team alice
id alice

# 4. Create bob and add both accounts to the group.
sudo useradd --create-home bob
sudo usermod --append --groups devops-team alice
sudo usermod --append --groups devops-team bob
getent group devops-team

# 5. Grant and verify temporary sudo access.
sudo usermod --append --groups wheel alice
su - alice -c 'sudo -n id'

# 6. Remove it and verify sudo is denied.
sudo gpasswd --delete alice wheel
su - alice -c 'sudo -n id' || echo 'Expected: alice is not authorized for sudo'
```

On Amazon Linux, membership in `wheel` normally grants sudo privileges through
`/etc/sudoers`; confirm the local policy with `sudo visudo` rather than assuming
this on another distribution. To test password SSH for `alice`, change
`PasswordAuthentication yes` in the applicable `sshd_config` or included
configuration using `sudoedit`, validate with `sudo sshd -t`, reload the SSH
service, test a **new** connection, then restore the secure key-only setting.
Do not make this change on a shared or production system.

For the final least-privilege requirement, create a dedicated sudoers drop-in
with `visudo`, not a regular editor:

```bash
sudo visudo -f /etc/sudoers.d/devops-team-user-management
```

Add this single rule, using the actual resolved paths shown by `command -v` on
the instance if they differ:

```sudoers
%devops-team ALL=(root) /usr/sbin/useradd, /usr/sbin/usermod
```

Then validate it and test as a team member:

```bash
sudo visudo -cf /etc/sudoers.d/devops-team-user-management
su - alice -c 'sudo -l'
```

This rule permits only the listed commands as root; it does not automatically
permit `groupadd`, `passwd`, a shell, or arbitrary sudo commands. Sudoers rules
are security-sensitive: remove the `wheel` membership before testing the
restriction, otherwise broad `wheel` access defeats the exercise.

## 8. File permissions and ownership

```bash
# 1. Create a file and inspect its default permissions (affected by umask).
printf '%s\n' 'lab-only content' > secret.txt
stat -c '%A %a %U:%G %n' secret.txt

# 2. Permit only the owner to read and write, then test as another user.
chmod 600 secret.txt
sudo -u bob cat secret.txt

# 3. Owner-only executable script: rwx------ is numeric 700.
printf '%s\n' '#!/usr/bin/env bash' > deploy.sh
chmod 700 deploy.sh
stat -c '%A %a %n' deploy.sh

# 4. Directory with owner rwx, group r-x, and no other access.
mkdir shared
chmod 750 shared

# 5. Change the file owner and group.
sudo chown alice:devops-team secret.txt

# 6. Recursively set ownership for three project files.
mkdir project
touch project/file-{1,2,3}.txt
sudo chown -R alice:devops-team project

# 7. Inspect shadow metadata without displaying password-hash contents.
sudo stat -c '%A %a %U:%G %n' /etc/shadow
```

A new regular file is commonly readable by its owner, group, and others subject
to the process `umask`; inspect rather than assume the exact default. After
`chmod 600`, another unprivileged user receives `Permission denied`. Numeric
`700` is owner `4+2+1` (read, write, execute) with no group or other bits.
`750` is owner `rwx`, group `r-x`, and no access for others. `/etc/shadow` is
restricted because it contains password-verifier data and related account-aging
metadata; revealing it would increase credential-attack risk.

## 9. Key-based SSH for a new user

### 1. Create the server account without a usable password

On the lab server as an administrator:

```bash
sudo useradd --create-home --shell /bin/bash deploy
sudo passwd --lock deploy
sudo passwd --status deploy
```

`--lock` prevents password authentication for the account. The account can
still use an authorized public key if SSH is configured to allow it.

### 2. Generate a local key pair

On the **local client**, not on the server:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/deploy-lab-ed25519 -C 'deploy-lab'
chmod 600 ~/.ssh/deploy-lab-ed25519
```

Choose a passphrase when prompted. The private file stays local;
`~/.ssh/deploy-lab-ed25519.pub` is the public key to authorize on the server.

### 3. Install the public key with secure ownership and modes

From the local client, use the existing `ec2-user` administrator connection to
install only the public key:

```bash
cat ~/.ssh/deploy-lab-ed25519.pub | \
  ssh -i <path-to-private-key.pem> ec2-user@<instance-public-dns-or-ip> \
  'sudo install -d -m 700 -o deploy -g deploy /home/deploy/.ssh && \
   sudo tee /home/deploy/.ssh/authorized_keys >/dev/null && \
   sudo chown deploy:deploy /home/deploy/.ssh/authorized_keys && \
   sudo chmod 600 /home/deploy/.ssh/authorized_keys'
```

Verify ownership and permissions on the server:

```bash
sudo stat -c '%A %a %U:%G %n' /home/deploy /home/deploy/.ssh /home/deploy/.ssh/authorized_keys
```

The home directory must not be writable by unrelated users, and `~/.ssh` and
`authorized_keys` should be owned by `deploy` with modes `700` and `600`.

### 4. Connect using the new private key

On the local client:

```bash
ssh -i ~/.ssh/deploy-lab-ed25519 deploy@<instance-public-dns-or-ip>
whoami
```

The verification output should be `deploy`.

### 5. Try without the key

Use a command that does not offer the lab identity file:

```bash
ssh -o IdentitiesOnly=yes -o IdentityFile=none deploy@<instance-public-dns-or-ip>
```

It should fail with `Permission denied (publickey)` when password authentication
is disabled or the account password is locked. It fails because the client did
not provide a private key corresponding to an authorized public key. Agent-held
or default key files can otherwise make a plain `ssh deploy@host` succeed, so
this explicit test is more reliable.

### 6. Disable password authentication and verify

On the server, edit the relevant SSH configuration with `sudoedit` and set:

```text
PasswordAuthentication no
KbdInteractiveAuthentication no
```

Before reloading SSH, validate the configuration:

```bash
sudo sshd -t
sudo systemctl reload sshd
```

Keep the existing session open, then attempt a new password-only connection
from the client (using `IdentitiesOnly=yes` and `IdentityFile=none` as above).
It should be rejected. If the test is unsuccessful, use the existing session to
restore the previous configuration and run `sudo sshd -t` before another reload.

Disabling password authentication reduces exposure to password guessing,
password reuse, and password interception risks. It is not sufficient on its
own: production systems also need controlled network access, strong key
protection, account lifecycle management, updates, logging, and monitoring.

## Validation checklist

Use these checks after completing the lab; expected values are environment
specific:

```bash
# Confirm the expected Amazon Linux distribution and current SSH configuration.
cat /etc/os-release
sudo sshd -T | grep -E '^(passwordauthentication|kbdinteractiveauthentication) '

# Confirm user/group memberships and sudoers syntax.
id alice
id bob
getent group devops-team
sudo visudo -c

# Confirm key authorization file permissions without exposing key contents.
sudo stat -c '%A %a %U:%G %n' /home/deploy/.ssh /home/deploy/.ssh/authorized_keys

# Confirm the text-processing exercise produces the expected running count.
awk '$3 == "running" { count++ } END { print count + 0 }' servers.txt
```

## Official references

- [Amazon EC2 key pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)
- [Connect to a Linux instance using SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-linux-inst-ssh.html)
- [Amazon VPC security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [Amazon Linux 2023 User Guide](https://docs.aws.amazon.com/linux/al2023/ug/what-is-amazon-linux.html)
- [OpenSSH `sshd_config` manual](https://man.openbsd.org/sshd_config)
- [sudoers manual](https://www.sudo.ws/docs/man/sudoers.man/)
