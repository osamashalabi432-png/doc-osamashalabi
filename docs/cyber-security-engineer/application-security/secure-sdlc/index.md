# Secure SDLC — Overview

The Secure Software Development Lifecycle (SSDLC) is the practice of building security into every phase of software development, rather than bolting it on at the end. It matters because of when security issues traditionally get found: in a standard SDLC, security testing is introduced very late in the lifecycle, so bugs, flaws, and other vulnerabilities are identified late — which makes them far more expensive and time-consuming to fix than if they had been caught during design or early development.

Two practices sit at the core of doing this well: **Risk Assessment** (deciding what level of risk is acceptable and where to focus effort) and **Threat Modelling** (structurally identifying what could go wrong before code is written).

!!! note "Scope of this page"
    This page covers Risk Assessment and Threat Modelling — the two SSDLC practices with recorded notes so far. Secure Configuration and Security Assessments are recognized SSDLC activities but have no captured content yet.

## Risk Assessment

A risk assessment is how you decide, deliberately, what level of security risk a piece of software is allowed to carry — instead of leaving it to guesswork or fixing everything indiscriminately. It matters because security effort is always a tradeoff against cost and time, and a risk assessment is what turns that tradeoff into a documented, defensible decision that stakeholders actually agree on.

### Performing a risk assessment

1. **Start from the assumption the software will be attacked**, and consider what would motivate a threat actor to target it.
2. **List the factors that drive that risk** — the value of the data the program handles, the security posture of any third-party resources the code depends on, who the clients purchasing the software are, and how widely the software is distributed (a single install, a small workgroup, or a worldwide release). Based on these factors, write down what level of risk is acceptable.
3. **Weigh the cost of a breach against the cost of fixing it.** For example, a data loss incident could cost a company millions — especially with regulatory fines such as GDPR — while eliminating all potential security bugs in the code might cost tens of thousands of dollars. The company and other stakeholders have to decide together whether that spend is worth it, and communicate the tradeoff clearly so everyone understands the risk and its implications. Reputational damage from an attack often costs more in the long run than the fix would have.
4. **Evaluate the risk**, including the worst-case scenario if an attacker succeeds. Simulating a ransomware attack is one way to make that worst case concrete for executives and senior engineers.
5. **Determine the value of what could be stolen** — user identities, credentials that grant control over endpoints on the network, and other data or assets, some of which carry more risk than others. Also factor in how difficult the attack would be to pull off (its complexity).
6. **Weigh impact by what's exposed.** For example, gaining access to an internal tool used for colleague feedback or retrospectives is lower impact than gaining access to a production monitoring and alerting system.
7. **Treat high risk as unacceptable and mitigate it.** A vulnerability that can be exploited with prewritten attack scripts, or spread via botnets, needs to be dealt with — the number of users affected is a key factor here.
8. **Consider blast radius and accessibility.** Some attacks affect only one or two users, while a denial-of-service attack can affect thousands, and worms can spread across thousands of machines. Also consider whether the target is reachable across a network or only locally, whether authentication is required, and whether the target is a production environment (higher impact) versus a sandbox used for labs and tutorials (lower impact).

### Types of risk assessment

There are several types of risk assessment, suited to different scenarios.

**Qualitative Risk Assessment** is the most common type found in companies. It classifies risk into thresholds — "Low", "Medium", "High" — by systematically examining what can cause harm and what controls should be put in place, with "High" carrying the most urgency. Even though it doesn't use numbers, a typical qualitative formula is:

```text
Risk = Severity x Likelihood
```

Severity is the impact of the consequence, and Likelihood is the probability of it happening — it's up to the risk assessor to judge both.

**Quantitative Risk Assessment** measures risk with numerical values instead of Low/Medium/High bands, using tools or a custom set of calculations based on the company's own processes to derive Severity and Likelihood.

!!! tip "Example"
    Suppose services are assigned business-criticality levels. You might decide that a bug affecting a business-critical service (an authentication service, payment infrastructure, etc.) is worth 5 points. Building a scoring scheme endemic to the company's own services — rather than a generic one — produces much better prioritization results than a one-size-fits-all model.

## Threat Modeling

Threat modelling is a structured process of identifying potential security threats and prioritizing techniques to mitigate them, so that the data or assets classified as valuable or high-risk during the risk assessment (confidential data, for example) are actually protected. It's best integrated into the **design phase** of the SDLC, before any code is written — when performed early, potential issues can be found and solved while they're still cheap to fix, rather than after the cost of fixing them has already climbed.

There are various methods for performing threat modelling, and they don't all share the same purpose — some focus on risk, some on privacy concerns, and some are more customer-focused. Methods can be combined to build a better picture of potential threats; which one (or combination) fits best depends on the project or business.

!!! note "Common threat modelling methodologies"
    **STRIDE**, **DREAD**, and **PASTA** are among the common threat modelling methodologies. The source notes only name these frameworks as the common ones in use — no further detail on how each is applied has been captured yet.
