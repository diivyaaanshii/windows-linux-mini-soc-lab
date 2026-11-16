# Mini SOC Lab — Security Investigation Guide

## 1. Windows Failed Login Investigation

**Event ID:** 4625

**Objective:** Identify unsuccessful authentication attempts.

Investigation steps:
1. Search Windows Security events in Splunk.
2. Filter for Event ID 4625.
3. Extract the target username, source IP, and logon type.
4. Review event timestamps and identify repeated failures.
5. Use the multiple failed-login detection query to identify accounts with at least three failures within five minutes.

**Observation:** During controlled testing, multiple failed login attempts were generated and observed in Splunk.

## 2. Windows Successful Login Investigation

**Event ID:** 4624

**Objective:** Review successful authentication activity.

Investigation steps:
1. Search for Event ID 4624.
2. Extract the target username.
3. Review login counts and timestamps.
4. Examine logon types to understand the authentication context.

## 3. Linux Privileged Command Monitoring

**Log source:** `/var/log/auth.log`

**Objective:** Monitor sudo activity and privileged command execution.

Investigation steps:
1. Search Linux authentication events in Splunk.
2. Filter events containing `COMMAND=`.
3. Extract the executed command.
4. Review command frequency and timestamps.
5. Identify unusual activity for further investigation.

**Controlled test:** Executed `sudo whoami` and verified the corresponding `/usr/bin/whoami` command event in Splunk.

## 4. SOC Dashboard

The dashboard provides an overview of:
- Successful Windows logins
- Failed Windows logins
- Failed logins by username
- Failed login activity over time
- Linux sudo activity over time
- Frequently executed privileged commands

## 5. Investigation Limitations

- This is a local learning environment, not a production SOC.
- A high number of failed logins is an investigation signal, not proof of compromise.
- Linux failed-authentication detection was not confirmed during this lab.
- The sudo monitoring threshold is illustrative and requires environment-specific tuning.

## 6. Key Takeaway

This lab demonstrates practical experience in security log collection, event investigation, SPL-based detection, Linux privileged activity monitoring, and security dashboard development.
