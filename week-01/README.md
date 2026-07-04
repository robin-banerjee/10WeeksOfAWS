# Week 1 — Cloud Foundations + IAM (Users, Groups & Policies)

> Sessions S1–S2 · Jul 4–5, 2026 · SAA-C03 Domain 1 — Design Secure Architectures (30%)
> Deep dive: study-repo modules `01-AWS-Fundamentals`, `02-IAM`

Welcome to the challenge. This week is your foundation. You'll set up an AWS account safely, get a feel for how the cloud is structured, and learn to control who can do what with IAM. Everything you build over the next nine weeks sits on top of this.

## Overview

By the end of the week you should be able to:

- Explain the AWS Global Infrastructure (Regions, Availability Zones, edge locations) and the Shared Responsibility Model.
- Set up an AWS account securely — lock down the root user, turn on MFA, and add a billing alarm so a surprise bill can't happen.
- Create IAM users, groups, and policies, and apply least privilege using JSON policies and permission boundaries.

## Theory & Concepts

**S1 — Cloud Foundations**
- **Global Infrastructure.** Regions are geographic areas; each contains isolated Availability Zones; edge locations serve content close to users. Almost every question about resilience or latency starts here.
- **Shared Responsibility Model.** AWS secures the cloud (hardware, global infra); you secure what's *in* the cloud (data, IAM, configuration). The exam often asks "who is responsible for X?"
- **Account and root user.** The root user can do everything, so you should use it almost never. Leaving it unprotected is the single most common real-world mistake.

**S2 — IAM Users, Groups & Policies**
- **Users vs groups.** Attach policies to groups and put users in the groups. It scales cleanly and it's the recommended practice.
- **IAM policies (JSON).** Built from `Effect`, `Action`, `Resource`, and `Condition`. Reading a policy and predicting "allow or deny?" is a guaranteed exam skill.
- **Least privilege.** Grant only the permissions actually needed. This is the core idea of Domain 1.
- **Permission boundaries.** A maximum-permission ceiling for a user or role. People confuse these with SCPs (which come in Week 2), so learn the difference now.

## Hands-on Labs

Do these in your own AWS account, use the Free Tier, and run the cleanup step when you're done.

**Lab 1 — Secure your account (root hardening + billing alarm)**

The goal is an account you can safely build in for ten weeks.

1. Create or log into your AWS account.
2. Enable MFA on the root user (IAM → Security credentials → MFA).
3. In Billing → Billing preferences, turn on Free Tier and billing alerts.
4. In CloudWatch → Alarms, create a billing alarm (for example, alert if estimated charges go over $5) and subscribe your email through SNS. This is also your carry-forward cost thread — see below.
5. Stop using root. Create an IAM admin user for everyday work and sign in as that from now on.

Deliverable: screenshots of root MFA enabled and the billing alarm in an OK/ALARM state.

**Lab 2 — IAM users, groups, and least-privilege policies**

The goal is to see policy evaluation with your own eyes.

1. Create a group `Developers` and attach the managed policy `AmazonS3ReadOnlyAccess`.
2. Create an IAM user `dev-01`, add it to `Developers`, and generate console/CLI access.
3. As `dev-01`, confirm you can list S3 buckets but cannot delete one (you should get Access Denied).
4. Write a small custom JSON policy that allows `s3:GetObject` on one specific bucket ARN only, attach it, and test.
5. Optional: attach a permission boundary to `dev-01` and watch how it caps the effective permissions.

Deliverable: the custom policy JSON in your repo, plus a screenshot of the denied and allowed actions.

**Cleanup**
- Delete the test users, groups, and custom policies (keep your admin user and the billing alarm).
- Nothing paid is created this week, but leave the billing alarm on for all ten weeks.

## Carry-Forward

- **Cost — billing alarm.** You set it in Lab 1. That's the start of your Domain 4 (cost) thread. You'll watch cost implications each week, and in Week 10 you'll add Budgets, Cost Explorer, and Compute Optimizer.
- **Governance — CloudTrail.** Turn on a CloudTrail trail now so every API call in your account is logged. You'll lean on these logs for auditing and troubleshooting throughout, and revisit governance and observability properly in Week 10.

## Exam Domain Mapping

Domain 1 — Design Secure Architectures (30% of the exam). IAM is the single most-tested topic, so it's worth over-learning early.

<details><summary>Q1. Where should you attach IAM policies for easiest management at scale?</summary>

To IAM groups, then place users in the groups. Attaching directly to individual users doesn't scale.
</details>

<details><summary>Q2. A permission boundary grants <code>s3:*</code> but the user's policy grants only <code>s3:GetObject</code>. What can the user do?</summary>

Only `s3:GetObject`. A permission boundary sets the maximum possible permissions; the effective permission is the intersection of the boundary and the identity policy.
</details>

<details><summary>Q3. Under the Shared Responsibility Model, who patches the guest OS on an EC2 instance?</summary>

You do. AWS is responsible for the hypervisor and host; the customer manages the guest OS, patches, and applications.
</details>

## Learn in Public

Share a short post this week. Adapt this for LinkedIn, X, or GitHub:

```text
Week 1 of #10WeeksOfAWS done.

This week I set up my AWS foundation:
- Secured the account: root MFA and a billing alarm so no surprise bills
- Created IAM users, groups, and least-privilege JSON policies
- Got my head around the Shared Responsibility Model and the Global Infrastructure

The thing that finally clicked: [write one thing you actually understood this week].

Next up: IAM roles, STS, and Organizations.

#10WeeksOfAWS #CloudAdhar #TrainWithShubham
@TrainWithShubham @cloudadhar
```

Attach a screenshot of your billing alarm or IAM setup, and tag us so we can reshare.

---
<div align="center">

[Home](../README.md) · [Week 2 →](../week-02/)

</div>
