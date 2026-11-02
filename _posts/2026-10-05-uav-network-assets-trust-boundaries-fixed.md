---
layout: post
title: "UAV Network Assets and Trust Boundaries"
date: 2026-10-05 09:00:00 +0000
categories: [6g-uav-cybersecurity, threat-models]
tags: [UAV, 6G, 5G, cybersecurity, trust-boundaries, replay-attacks, swarm]
excerpt: "Lesson 1 of the 6G UAV Cybersecurity series: map the assets and trust boundaries in a networked UAV swarm, then analyse how replayed telemetry can mislead separation monitoring."
---

*6G UAV Cybersecurity · Lesson 1 · Threat models · Approximately 20 minutes*

**Learning objective.** Identify where external information becomes trusted enough to influence a drone's navigation, and describe an adversary that exploits that crossing.

**Prerequisite.** Basic familiarity with UAV navigation and telemetry. No previous lessons are required.

## 1. Mission scenario: three search-and-rescue drones

Imagine three drones conducting search and rescue. Each flies autonomously, shares its position with the others, and receives search assignments from a ground control station (GCS). A cellular network carries these messages. An **edge server**, a computer hosting applications near network users, relays swarm telemetry. Each drone makes its own avoidance decisions onboard.

Before selecting a security mechanism, identify the mission dependencies:

- Search assignments must originate from an authorized operator.
- Peer positions must be sufficiently accurate and recent.
- Communications outages must trigger an appropriate fallback.
- External messages must influence flight only through permitted interfaces.

These dependencies provide the starting point for a threat model: **what is trusted, why is it trusted, and what happens when that trust is misplaced?**

## 2. Assets, attack surfaces, and trust boundaries

An **asset** is something whose compromise matters, such as command authority, a signing key, trustworthy telemetry, or access to a mission service.

An **attack surface** is an exposed interface through which an adversary can interact, such as a telemetry API, wireless receiver, software update service, or command interface.

A **trust boundary** is a crossing where information or commands enter a different security context and require an explicit acceptance decision.

Receiving a peer-position message, for example, creates a boundary between another system's report and the receiving drone's navigation logic. The receiver must decide which uses of that report are justified.

### Cellular-network terminology

- **User equipment (UE):** the drone's cellular modem.
- **Radio access network (RAN):** the radio connectivity provided through base stations.
- **Core network:** functions such as subscriber authentication and connectivity management.
- **Mission application:** the software exchanging assignments, telemetry, and commands over the connectivity infrastructure.

### Standards checkpoint

As of **5 October 2026**, 6G remained under development. The ITU's [IMT-2030 framework](https://www.itu.int/en/ITU-R/study-groups/rsg5/rwp5d/IMT-2030/Pages/default.aspx), approved in November 2023, includes security and resilience; its 2026 requirements and evaluation reports were described as drafts. The swarm architecture here is illustrative, not a claim about a deployed 6G standard.

A standardized reference is [ETSI TS 133 256 V19.2.0](https://www.etsi.org/deliver/etsi_ts/133200_133299/133256/19.02.00_60/ts_133256v190200p.pdf) (February 2026), covering **5G UAS security**. Clause 5.1 assigns UAS traffic protection to the UAS provider independently of cellular-network protection. Consequently, application-level message acceptance is an explicit design responsibility.

[NIST SP 800-207, *Zero Trust Architecture*](https://csrc.nist.gov/pubs/sp/800/207/final) (August 2020) states that network location or ownership alone should not confer implicit trust. In this UAV scenario, delivery over an approved network is not sufficient evidence that telemetry is suitable for navigation.

### Map the trust boundaries

| Crossing | Asset at risk | Candidate acceptance rule |
|:--|:--|:--|
| Drone modem → operator network | Network access and subscriber identity | Authenticate and authorize network access |
| Ground station → mission application | Command authority | Verify origin, permitted action, and freshness |
| Sending drone → telemetry relay | Telemetry provenance | Preserve verifiable sender identity and message metadata |
| Relay → receiving drone | Trusted peer-state information | Check origin, age, sequence, and plausibility |
| Mission application → flight controller | Permitted aircraft behavior | Enforce command limits and operational constraints |

*These are proposed rules for the example, not assertions that every deployment implements them.*

A telemetry relay may be authorized to **deliver** a message without being authorized to make its contents **authoritative** for a flight decision. That distinction remains important even when the relay and receiving drone belong to the same organization.

## 3. Worked example: replayed peer telemetry

Consider a deliberately simplified **one-dimensional corridor**. Assume positions are exact, clocks are synchronized, and the receiving drone uses the latest accepted peer position without extrapolation.

At time $t=10$ seconds, Drone A reports:

```text
sender=A, sequence=41, sample_time=10.0, position_x=0.0
```

Over the next two seconds, A moves at $1.5\ \mathrm{m/s}$, reaching $x=3$ metres. Drone B is at $x=4$ metres.

An adversary controls the application telemetry relay. The adversary suppresses newer reports from A and resubmits the old application message at $t=12$ seconds.

The old message **may still carry a valid application signature**, because its original signed contents have not changed. The failure occurs if B treats recent **arrival** as evidence of recent **measurement**, without checking the sample time or repeated sequence number.

### Calculate the message age

$$
\Delta t=t_{\mathrm{receive}}-t_{\mathrm{sample}}=12-10=2\ \mathrm{s}.
$$

### Compare perceived and actual separation

Using the stale report, B estimates:

$$
\hat d=|4-0|=4\ \mathrm{m}.
$$

The true separation is:

$$
d=|4-3|=1\ \mathrm{m}.
$$

If the illustrative minimum permitted separation is $2$ metres, the stale-data separation check remains silent even though the true separation violates the threshold.

**Interpretation:** the calculation demonstrates a *false negative in a simplified separation check*. It does **not** establish that a collision occurs; that would require a closed-loop study of avoidance actions, independent sensors, and fallback logic.

## 4. State the adversary model explicitly

| Element | Description |
|:--|:--|
| **Asset** | Recent peer-state information used for separation monitoring |
| **Adversary access** | Control of the application telemetry relay |
| **Knowledge** | Message format, routing, and a previously valid report |
| **Capabilities** | Store, suppress, delay, and resubmit application messages |
| **Entry mechanism** | Submit an old report through the relay's normal delivery interface |
| **Boundary exploited** | Delivered telemetry becomes accepted navigation state |
| **Intended operational consequence** | Delay avoidance action and increase the chance of a separation violation |
| **Defender observations** | Sample time, arrival time, sequence number, report gaps, and independent peer observations |

The drone's signing key and flight controller remain trusted in this model. The attacker operates on **application messages** through the existing relay connection; replay of protected cellular radio frames is not required.

The distinction between levels of failure matters:

- **Component compromise:** the adversary controls the relay.
- **Application acceptance failure:** the receiver accepts stale telemetry as current peer state.
- **Observed consequence in this example:** a false negative in the simplified separation calculation.


## 5. Key takeaway

**Authenticated delivery does not imply fresh, plausible, or operationally safe information.** In a networked UAV swarm, the boundary between receiving telemetry and accepting it as navigation state needs explicit checks. An adversary who controls an application relay can exploit that boundary without compromising the drone's signing key or flight controller.


## References

1. ITU-R, [IMT-2030 framework](https://www.itu.int/en/ITU-R/study-groups/rsg5/rwp5d/IMT-2030/Pages/default.aspx).
2. ETSI, [TS 133 256 V19.2.0, 5G UAS security](https://www.etsi.org/deliver/etsi_ts/133200_133299/133256/19.02.00_60/ts_133256v190200p.pdf), February 2026.
3. NIST, [SP 800-207: Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final), August 2020.
