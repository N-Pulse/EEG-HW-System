# Design Solution Definition: EEG/Power Board

> **Project:** N-pulse – Wearable EEG System for Prosthetic Control
> **Document ID:** `DSD-EEG-PWR-001`
> **Version:** `0.1.0` · **Status:** `Draft`
> **Project phase:** `pre-System Design Review`
> **Last modified:** `2026-10-24`
> **Repository path:** `docs/DSD-POWER-BOARD.md`


HOW TO USE THIS TEMPLATE
- Replace every `<placeholder>`. Delete guidance comments before review.
- One DSD per subsystem or module
- The DSD must stand alone: a reviewer should be able to judge the design
  from this document, the linked requirements, and the linked evidence.
- Normative language: "shall" = requirement, "should" = recommendation,
  "may" = optional.

---

## Document control

### Roles and sign-off

| Role | Name | Affiliation / Handle | Date | Signature / Review link |
|---|---|---|---|---|
| Author(s) | Célia Lundmark| N-pulse — Power Board Designer | @cihnamxn | | |
| Reviewer (technical) | Maëlys Clerget | N-pulse — EEG Team Leader | @maelysclerget | | |
| Concurrence (SE Reviewer) | Luca Seelbach | N-pulse — System Engineer Electronics | | |
| Concurrence (Quality management reviewer) | Armelle Chritin | N-pulse — Quality Management | | |
| Approval (Project Manager) | Elodie Dubois | N-pulse — Project Manager | | |

> Note : Link approval to any relevant PR (Pull Request)
> Affiliation/Handle : Role/ Username on Github

### Modification log

| Version | Date | Author(s) | Main changes | PR / Commit |
|---|---|---|---|---|
| 1.0.0 | 2026-10-24 | | First version | |

---

## Table of contents

1. [Introduction](#1-introduction)
2. [Requirements and traceability](#2-requirements-and-traceability)
3. [Concept justification](#3-concept-justification)
4. [Traded-off concepts](#4-traded-off-concepts)
5. [Quality management and compliance](#5-quality-management-and-compliance) /!\ THIS SECTION IS NOT FINALISED /!\
6. [Licensing](#6-open-source-considerations) /!\ THIS SECTION IS NOT FINALISED /!\
7. [Open issues and future work](#7-open-issues-and-future-work)
8. [References](#8-references)
9. [Appendices](#9-appendices)

---

## 1. Introduction

### 1.1 Purpose and scope

<!-- Why does this document exist, at which review/milestone is it produced,
and what decision does it support? State what is IN and OUT of scope. 

DO NOT CHANGE ANYTHING IN THIS SECTION EXCEPT THE HIGHLIGHTED SECTIONS -->

This Design Solution Definition (DSD) is at a `Prototype` stage
validated on `<date>`. It justifies the concept selected for `EEG Power Board`.

It covers:

- the problem the subsystem must solve
- the retained concept: architecture, components, and design choices
- the concepts that were considered and not retained
- risks, quality, and compliance implications.

**Out of scope:** `EEG signal acquisition and processing (EEG Board DSD); electrode integration and mechanical design (Headset DSD).`

### 1.2 Responsibility and change authority

<!-- Who owns this document? Who may approve changes? How are changes
proposed (issue, RFC, pull request)? -->

- **Document owner:** `Célia / N-pulse Power Board Team` <!-- Who is responsible to keep this document up to date ? --> 
- **Change authority:** `EEG Team Leader and System Engineer Electronics, subject to project review and approval procedures` <!-- What is the role/responsibility of the person -->


### 1.3 Abbreviated terms and definitions

| Term | Definition |
|---|---|
| DSD | Design Solution Definition | 
| EEG | Electroencephalography | 
| IC | Integrated Circuit |
| NTC | Negative Temperature Coefficient |
| BMS | Battery Management System |
| LDO | Low Dropout Regulator |
| TVS | Transient Voltage Suppressor | 


## 2. Requirements and traceability

<!-- Do not duplicate the full requirements document. Link to it and list only
the requirements this design directly addresses. -->

| Requirement ID | Verification method | Status |
|---|---|---|
| EEG-001 | T | System integration testing pending |
| EEG-005 | T, A | Design validated — hardware testing pending |
| EEG-006 | T | Electrical safety testing pending |
| EEG-007 | T | EMC compliance testing pending |
| EEG-010 | T, M | Skin temperature testing pending |
| EEG-011 | T, M | Patient leakage-current testing pending|
| SAF-008 | T, M | Skin temperature safety testing pending |
| SAF-009 | T, M | Patient leakage-current testing pending |

> <span style='color: red;'>EXAMPLE</span>
>| Requirement ID | Verification method | Status |
>|---|---|---|

 **Verification methods:** (D)emonstration, (T)est, (M)easurement, (I)nspection, (A)nalysis
 

---

## 3. Concept justification

`<Explain HOW the design addresses the technical problem it's trying to solve. Every major element should trace back to a need or requirement>`

<span style="color: red;">The following chapters and sub-chapters are an example. Tailor them to be the most relevant to your own system.</span>


### 3.1 Solution overview

`<Shortly, how does the retained concept address the problem ? State the core idea in one paragraph.>`

The EEG power board supplies clean and stable power to an EEG headset through a 2S Li-ion rechargeable battery pack. It also protects the user against overheating from the battery. The battery can be recharged via a USB-C port. The board supplies stable power by passing the battery voltage through two different voltage converters;  a buck and an LDO. The buck accomodates the fluctuating voltage supply from the battery and the LDO removes the ripple noise from the buck, ensuring a constant and clean 5.7V ouput from the board. The overheating protection is guaranteed by a hysteresis comparator that switches off the load when the battery reaches a certain temperature, and turns it back on when it is cool enough.

<!-- Mermaid renders natively on GitHub. Alternatively link an image from
`docs/figures/`. Include interfaces: data, mechanical, electrical,
biological/sample flow, and user interaction. -->

```mermaid
flowchart LR
    A[Battery Input] <--> B[Charging IC]
    C[USB-C port] --> B
    A --> D[Temperature Management]
    B --> E[Buck (7.4->6.1V)]
    E --> F[LDO (6.1-5.7V)]
    F --> G[Load + Switch]
    D --> G
```

*Figure 3.1: High-level architecture of EEG Power Board.*


### 3.2 Main design choices

<!-- One subsection per significant choice. Use this pattern:
Decision · Rationale · Requirements addressed · Consequences. -->

#### 3.2.1 Battery 

- **Decision:** Keeppower 2S1P 18650 3400mAh 7.2/7.4V Li-Ion with integrated BMS protection and three connections (+, −, Temp).
- **Rationale:** The 2S configuration provides a nominal 7.4 V supply compatible with the selected power regulation architecture. Its 3400 mAh capacity was selected to support the required 4-hour EEG system autonomy, subject to power consumption and runtime validation. Its integrated BMS provides overcharge, overdischarge, and short-circuit protection and NTC thermistor enables battery temperature monitoring without an additional external sensor. Its compact dimensions ((H)37.0*(W)19.0*(L)*67.5mm) and light weight (100 g) make it a candidate for integration into the wearable system. This pack has numerous safety certifications, ensuring that it is safe to be worn. 
- **Requirements addressed:** EEG-001 (battery autonomy), EEG-005 (battery safety).
- **Consequences / limitations:** As this battery's BMS does not handle thermal protection, a dedicated NTC-based protection circuit must be implemented on the Power Board. The battery is supplied with three wire leads, requiring an appropriate connector and secure cable routing. Battery autonomy and user comfort must be verified during system integration.


#### 3.2.2 Charger and port

- **Decision:** TI BQ25886 charger and USB-C port
- **Rationale:** The BQ25886 is a standalone 2-cell Li-ion battery charger. Its characteristics are compatible with our battery's requirements. It is configurable as we can choose the charging current, charging voltage, and input current limit. This charger monitors battery temperature during charging through an NTC thermistor and provides indications of the charging state. This IC will also enters a low-power mode when not charging which reduces power consumption. The USB-C port was chosen because it is compact and widely used compared to the previous USB-A port. 
- **Requirements addressed:** EEG-005 (battery safety).
- **Consequences / limitations:** As this charger operates without an MCU, the charging-status information available to the system is limited. Also, its NTC-based battery temperature monitoring is used during charging, which means we need a separate thermal protection circuit to monitor the battery temperature during discharge. 

#### 3.2.3 Voltage conversion

- **Decision:** Two-stage voltage regulation using a TI TPS63070 buck-boost converter followed by a TI TPS7A8101 low-dropout regulator (LDO). The buck-boost converter regulates the variable battery voltage (approximately 5–8.4 V) to 6.1 V, while the LDO provides a nominal 5.7 V output (5-8.4 V->6.1->5.7V). A 500 Ω trimmer is included in the LDO feedback network to allow output-voltage adjustment.
- **Rationale:** This buck-boost was chosen because it can maintain a regulated output despite the variations of the battery voltage (5V up to 8.4V). It also has an efficiency of up to 95% which minimizes power loss. This LDO is placed downstream to attenuate the ripple noise of the buck-boost which is a switching regulator. This helps prevent disturbances from being carried on to the decoding board. The intermediate value of 6.1V was chosen to provide sufficient headroom for the LDO, which has a typical dropout voltage of 170mV at 1A. The potentiometer allows the output to be adjusted from 5.49V to 5.87V. A power switch connected to the buck-boost converter's enable pin allows the regulated power supply to be turned off.
- **Requirements addressed:** EEG-001 (battery autonomy), EEG-007 (electromagnetic compatibility — contributes to this system-level requirement).
- **Consequences / limitations:** This architecture increases component count, PCB area, and power dissipation compared with a single-regulator architecture. The LDO introduces additional efficiency losses and requires sufficient voltage headroom to maintain regulation. Output-voltage accuracy, regulator efficiency, and residual ripple must be verified experimentally once the PCB is available. 

#### 3.2.4 Thermal protection

- **Decision:** A hysteresis comparator that compares the voltage derived from an NTC thermistor, which is placed between the two battery cells, with a reference voltage.  When the temperature recorded by the NTC thermistor exceeds 58°C, the comparator's output goes low, disabling the load switch placed at the LDO's output, cutting off the supply to the decoding board. When this happens, a buzzer sounds for a couple of seconds, signalling to the user that the system has overheated, and a yellow LED remains on while the system is disabled. When the temperature has gone back down to 50°C, the comparator's output goes back high and the supply to the decoding board is restored. The circuit uses an MCP6541 comparator, MCP1703AT LDO, AP2281 load switch, MFS201N-9-Z switch, LED and buzzer.
- **Rationale:** The BQ25886 charging IC monitors the battery temperature through the NTC thermistor during charging, but not during battery-powered operation. A separate thermal protection circuit is therefore required to monitor the battery temperature during operation and disconnect the decoding board in case of overheating. The hysteresis prevents repeated switching when the temperature fluctuates around the shutdown threshold. The buzzer and LED provide feedback to the user when overheating occurs. Because the power board will not be directly visible to the user during operation, the buzzer was implemented to give an auditory clue about the system's state. The LED allows the user to know when the board will be operational again after overheating (ON: the battery is cooling and the supply to the decoding board is off, OFF: the cooling down is finished, the headset is once again operational). An LDO is placed upstream of the comparator to bring down the battery voltage to 5V which is in the supply voltage range of this IC (1.6-5.5V).
- **Requirements addressed:** EEG-005 (battery safety), EEG-010 (skin-contact temperature), SAF-008 (prevention of skin burns).
- **Consequences/ limitations:** This solution requires additional components and PCB space. The thermal protection depends on the correct operation of the NTC thermistor, comparator, and load switch. To know when the headset is operational again after overheating, the user should take off the headset and only put it back on once the yellow LED is off. There is also currently no mechanism to prevent the system from operating if the NTC switch is not in operating mode, it is up to the user to make sure that the switch is in this state. The shutdown and recovery temperatures, as well as the buzzer and LED behavior, must be verified experimentally once the PCB is available.


#### 3.2.5 Connectorss

- **Decision:** XT30F for the output to the EEG decoding board, 2.00mm Pitch Pico-Lock MOLEX for the battery input.
- **Rationale:** Connectors are used instead of directly soldering the cables to the board to allow for easier debugging and part replacement. The Pico-Lock connector is a polarised and locking connector, which means that it can't be plugged backwards and it locks in place making the connection more resistant to tugs and vibrations. This connector can support the maximum current supplied by the battery. The XT30 connector is also polarised and has a low contact resistance, minimising the voltage drop. This connector is not locking but its friction works well enough especially for prototyping. The female connector is used on the Power Board to reduce the risk of accidental short circuits by limiting access to exposed contacts.
- **Requirements addressed:** EEG-005 (battery safety)
- **Consequences / limitations:** These connectors take considerable space on the PCB, and the battery cables must be crimped.

#### 3.2.6 Operating modes

<!-- For instruments: operating modes. For protocols and pipelines: workflow
stages. Include entry and exit conditions and what is active in each. -->

| Mode / Stage | Purpose | Entry condition | Exit condition | Active components |
|---|---|---|---|---|
| OFF | No power to the system | Power switch open | Power switch closed | Battery's BMS, charging IC if USB-C is connected |
| Nominal operation | Supplying power to the EEG system | Power switch closed, battery connected, NTC switch on operating mode, temperature below the threshold (connection to the decoding board) | overheating, battery disconnected or discharged, power switch open | Battery, buck-boost, LDO, comparator|
| Charging | Recharging the battery from the USB port |supply on the USB port, NTC switch on charging mode, power switch open (manually set by user) | USB port unplugged USB port, NTC switch on operating mode, power switch closed| USB-C port, charging IC, battery |
| Safe / fault state (overheated) | Allowing the battery to cool down by cutting off the supply to the EEG decoding board | Battery temperature exceeds 58°C during operation | Battery temperature drops below 50°C, automatically reactivating the load switch | Battery, BMS, buck-boost, LDO, NTC, comparator, load switch (OFF), LED, buzzer (temporarily) |

### 3.3 Functionally linked systems

This board supplies the power required for the EEG system to operate. This system is comprised of the decoding board and the headset housing the electrodes. The Power Board is connected to the decoding board via two cables (+,-). 


### 3.4 Performance budgets and design margins

Mass/Power/Compute/Delay budgets. Either include them here directly or link the relevant excel sheet

Component list : 
---

## 4. Traded-off concepts

<!-- Show your work. This section protects the project from re-litigating
decisions and records why alternatives were rejected. -->

`<This is the most important part of this document. Please fill it in with great attention. >`

`<The goal of this section is to go over every other solution you found relevant but that you chose not to pursue.>`

### 4.1 Trade-off method

The trade-off method used was based on the following criteria : 
- Meets performance requirements
- Safety and biosafety
- Cost
- Reproducibility and accessibility
- Complexity and space 
- Maintainability and community support


`<Describe criteria, weights, and scoring scale. This is a standardized way to explicit your priorities in the design.>`

<span style='color: red;'> The table given below is an example. Adapt it with the above mentionned criterias</span>

`<example for an ADC chip for a proof of concept board>`


| Criterion | Weight (1 - 5) | Rationale for weight |
|---|---|---|
| Meets performance requirements | 5 | The Power Board is part of a first proof of concept. As such, stable and low-noise power delivery is a priority. |
| Safety and biosafety | 5 | This system can overheat and present risks to the patient so safety is very important. |
| Cost | 3 | This is a proof of concept. The components for tis board are ordered in small batches and as such prices are higher. Additionally, budget is a constraint for this board.  |
| Reproducibility and accessibility | 4 | The design should be easy to reproduce using accessible components and standard assembly methods. |
| Complexity and space  | 3 | This board should be as small as possible while also minimising its complexity. |
| Maintainability and community support | 1 | As this is an early prototype, long-term maintenance and community support are less important than performance and safety. However, the design should still allow debugging and component replacement. |

Score scale : 
> [1] : Does not meet any requirements
> [2] : Meets some requirements 
> [3] : Meets all minimum requirements 'Shall' requirements
> [4] : Meets all minimum requirements and 'should'
> [5] : Meets all requirements, 'Shall, should & may'

### 4.2 Traded-off design choices

#### 4.2.1 Temperature regulation with MCU

- **Description:** We considered using the integrated NTC from the battery and connecting it to a MCU that would be in charge of monitoring the temperature during operation. The design is evaluated without a hardware backup.
- **Pros**: Using software makes the thermal protection more flexible and easy to modify. The design can also be structured as a state machine with distinct states, each having their transition conditions and behaviour. 
- **Cons**: The MCU must remain powered to monitor the temperature and control the protection, which consumes energy during the cooling phase and prolongs it. Reusing the decoding board’s MCU would require additional connections between the two boards, and a way to keep the MCU powered while the EEG load is disconnected. Adding a dedicated MCU on the Power Board would take more space and make the design far more complicated. The over-temperature protection also relies on software running correctly. A firmware bug, a hung or crashed program, a brown-out during start-up, or a misconfigured threshold could leave the battery unprotected, and nothing in this option would catch it.

| Criterion | Weight (1-5) | score (1-5) |
|---|---|---|
| Meets performance requirements | 5 | 4 |
| Safety and biosafety | 5 | 4 |
| Cost | 3 | 3 |
| Reproducibility and accessibility | 4 | 4 |
| Complexity and space | 3 | 2 |
| Maintainability and community support | 1 | 4 |
| **Total Score** | | **75 / 105 (71.4%)** | 

#### 4.2.2 IC that measures the temperature

- **Description:** A temperature-sensing IC with an integrated comparator measures the temperature at its own surface and handles the temperature comparison and switching internally. It would be mounted in one of two positions: placed against the inside of the casing on the side closest to the user, with the battery behind it, or placed directly against the battery. The trip temperature is set by an external resistor on the SET pin, following the formula given in the datasheet. The hysteresis band is selected by tying a second pin to VDD or GND.
- **Pros**: The solution requires very few components: one IC and two resistors. The comparison and hysteresis are handled internally by the IC, reducing the number of external components and making the circuit simpler to implement.
- **Cons**: Placing the IC directly on the battery may expose the neighbouring PCB components to elevated temperatures, depending on their placement and thermal ratings. Placing the IC against the casing means we measure the casing temperature rather than the battery’s. The casing temperature is only an indirect indicator of the battery temperature, so the trip threshold would have to be determined from thermal modelling and experimental validation. The trip point also depends on how well the IC is thermally coupled to the casing or battery, so the actual trip temperature at which protection is triggered can vary with the mounting conditions. Finally, a single sensor only measures one side of the casing. Another side could be hotter, which puts the patient at risk of burns on contact.
 
| Criterion | Weight (1-5) | score (1-5) |
|---|---|---|
| Meets performance requirements | 5 | 3 |
| Safety and biosafety | 5 | 2 |
| Cost | 3 | 4 |
| Reproducibility and accessibility | 4 | 2 |
| Complexity and space | 3 | 5 |
| Maintainability and community support | 1 | 3 |
| **Total Score** | | **63 / 105 (60.0%)** |

#### 4.2.3 Temperature regulation with pre-isntalled NTC and comparator score

- **Description:** The selected solution uses the NTC thermistor already integrated into the battery pack, connected to an external comparator with hysteresis. The comparator monitors the battery temperature and disables the load switch when it reaches 58°C, then re-enables it when the temperature drops below 50°C.
- **Pros**: The NTC is integrated inside the battery pack, so it measures the temperature close to the cells without requiring an additional sensor, reducing uncertainties related to external sensor placement. Its reliability is backed by the battery’s certifications. The protection is entirely analogue, so it does not depend on firmware execution, and it operates independently of the decoding board, which allows the load switch to be placed at the LDO’s output without additional wiring between the boards.
- **Cons**: The slide switch must be set by the user. If it is left in the 'Charging position',the comparator may no longer receive a valid temperature signal, potentially leaving the thermal protection inactive. Changing the trip or recovery temperature requires modifying resistor values, whereas the MCU option would only require a firmware change. The circuit requires the comparator, load switch, LDO, switch, passives, LED and buzzer, which increases the component count and PCB area. The MCP6541 is limited to a 5.5 V operating supply and no suitable lower-voltage rail exists on this board, so a dedicated 5 V LDO is required, which increases cost and area.

| Criterion | Weight (1-5) | score (1-5) |
|---|---|---|
| Meets performance requirements | 5 | 5 |
| Safety and biosafety | 5 | 3 |
| Cost | 3 | 3 |
| Reproducibility and accessibility | 4 | 4 |
| Complexity and space | 3 | 3 |
| Maintainability and community support | 1 | 4 |
| **Total Score** | | **78 / 105 (74.3%)** |

#### 4.2.4 USB-A port

- **Description:** A USB-A interface was considered to provide a 5 V input to the BQ25886's VBUS pin for battery charging, with D+/D− connected to the charger for source detection.
- **Pros**: The interface is simple: no CC pins or pull-down resistors are needed. USB-A ports and chargers are also found on most computers and legacy adapters.
- **Cons**: The USB-A receptacle is much wider and deeper than a USB-C one, which works against the compact form factor needed for an EEG headset. The connector is not reversible, making it less convenient and potentially more susceptible to damage if inserted incorrectly. It is also being phased out, as new devices and chargers increasingly come with USB-C only.

| Criterion | Weight (1-5) | Score (1-5) |
|---|---:|---:|
| Meets performance requirements | 5 | 3 |
| Safety and biosafety | 5 | 4 |
| Cost | 3 | 4 |
| Reproducibility and accessibility | 4 | 4 |
| Complexity and space | 3 | 2 |
| Maintainability and community support | 1 | 3 |
| **Total Score** | | **72 / 105 (68.6%)** |


#### 4.2.5 USB-C port

- **Description:** A USB-C port provides a 5 V input to the BQ25886 for battery charging.
- **Pros**: The selected USB-C receptacle is much smaller and thinner than a typical USB-A receptacle, which helps keep the board compact. The cable can be plugged in either orientation, which is more convenient and reduces the risk of incorrect insertion and thus port damaging. It is also a common charging interface on current devices, so users are likely to already own a compatible charger and cable.
- **Cons**: The 16-pin receptacle has fine-pitch pads and requires the two CC resistors which makes it more complex. This solution is also slightly more expensive.


| Criterion | Weight (1-5) | Score (1-5) |
|---|---:|---:|
| Meets performance requirements | 5 | 5 |
| Safety and biosafety | 5 | 4 |
| Cost | 3 | 3 |
| Reproducibility and accessibility | 4 | 3 |
| Complexity and space | 3 | 4 |
| Maintainability and community support | 1 | 5 |
| **Total Score** | | **83 / 105 (79.0%)** |


#### 4.2.6 Buck for voltage regulation

- **Description:** A single step-down (buck) converter would regulate the battery voltage directly to the 5.7 V rail that supplies the decoding board, with no second regulation stage.
- **Pros**: With only one stage, the overall efficiency can be higher. This design also requires fewer components and less board area, making it less costly.
- **Cons**: A buck can only step down, so the usable battery range is reduced once the battery voltage becomes too low to maintain a regulated 5.7 V output. Without an additional LDO stage, less attenuation of the converter's switching ripple is provided, so more noise could reach the decoding board, which is particularly important in our system.

| Criterion | Weight (1-5) | Score (1-5) |
|---|---:|---:|
| Meets performance requirements | 5 | 2 |
| Safety and biosafety | 5 | 4 |
| Cost | 3 | 4 |
| Reproducibility and accessibility | 4 | 4 |
| Complexity and space | 3 | 4 |
| Maintainability and community support | 1 | 4 |
| **Total Score** | | **74 / 105 (70.5%)** |

#### 4.2.7 Buck-boost and LDO for voltage regulation score

- **Description:** A two-stage voltage regulation system using a buck-boost converter followed by an LDO. The buck-boost regulates the varying battery voltage (5–8.4 V) to 6.1 V, while the LDO reduces it to 5.7 V and attenuates switching ripple before supplying the EEG decoding board.
- **Pros**: The buck-boost converter can step up or down, so the output remains regulated across the intended battery operating range, allowing more of the available battery capacity to be used. The LDO attenuates the buck-boost’s switching ripple, so less disturbance reaches the decoding board. The trimmer also allows the final voltage to be tuned after manufacturing, which is useful on a proof of concept.
- **Cons**: The overall efficiency can be lower because of the two regulation stages. The design requires two regulators, an inductor, a trimmer and additional capacitors and resistors, so it takes more board area and costs more.

| Criterion | Weight (1-5) | Score (1-5) |
|---|---:|---:|
| Meets performance requirements | 5 | 5 |
| Safety and biosafety | 5 | 4 |
| Cost | 3 | 3 |
| Reproducibility and accessibility | 4 | 4 |
| Complexity and space | 3 | 2 |
| Maintainability and community support | 1 | 4 |
| **Total Score** | | **80 / 105 (72.4%)** |

### 4.3 Trade-off summary

| Decision | Option | Score (/105) | Retained | Key reason |
|---|---|---|---|---|
| Thermal protection | NTC + hardware comparator | 78 | **Yes** | Uses the battery's integrated NTC, requires no firmware, and has low power comsumption. Weakness: manual switch. |
| Thermal protection | MCU | 75 | No | Flexible, but must stay powered, adds wiring or complexity, and depends on firmware. |
| Thermal protection | Temperature-sensing IC | 63 | No | Few components, but temperature measurement depends on sensor placement and may not adequately represent battery temperature. |
| Charging port | USB-C | 83 | **Yes** | Compact, reversible and widely used. |
| Charging port | USB-A | 72 | No | Larger, not reversible, and less common on newer devices. |
| Voltage regulation | Buck-boost + LDO | 80 | **Yes** | across the intended battery operating range with additional switching-ripple attenuation. |
| Voltage regulation | Buck only | 74 | No | Reduced usable battery-voltage range and no additional LDO stage for ripple attenuation. |

## 5. Quality management and compliance

**WORK IN PROGRESS**
<!--
<!-- Adapt to your regulatory context. If the project is RUO-only, say so and
explain which parts apply by choice (good practice) versus by obligation. 

### 5.1 Regulatory classification and pathway

| Item | Statement |
|---|---|
| Intended use classification | `<RUO / IVD / medical device / bioprocess / environmental / other>` |
| Target jurisdictions | `<EU / US / CH / UK / other>` |
| Applicable regulation | `<e.g. EU IVDR 2017/746, EU MDR 2017/745, FDA 21 CFR 820 (QMSR), 21 CFR Part 11>` |
| Device / product class | `<class and rationale>` |
| Regulatory strategy | `<summary, notified body / agency contact status>` |

### 5.2 Quality management system

- **QMS framework:** `<ISO 13485 / ISO 9001 / GLP / GMP / GCP / lightweight in-house QMS>`
- **QMS scope and document hierarchy:** `<link>`
- **Design controls followed:** `<design inputs → outputs → review → verification → validation → transfer → changes>`
- **This document's place in the design history file (DHF):** `<link>`

### 5.3 Applicable standards and guidelines

| Standard / Guideline | Title | Applicability | Compliance status |
|---|---|---|---|
| ISO 13485 | Medical devices: Quality management systems | | Planned / Partial / Compliant / N/A |
| ISO 14971 | Application of risk management to medical devices | | |
| IEC 62304 | Medical device software life cycle | | |
| IEC 61010-1 | Safety of electrical equipment for measurement, control, laboratory use | | |
| IEC 62366-1 | Usability engineering | | |
| ISO 20387 | Biobanking: general requirements | | |
| `<other>` | | | |


### 5.4 Ethics, access, and biosecurity compliance

| Topic | Applicability | Status / reference |
|---|---|---|
| Ethics committee / IRB approval | | |
| Informed consent | | |
| Nagoya Protocol / access and benefit sharing | | |
| Biosafety committee approval | | |
| Import / export / dual-use controls | | |
| Animal research regulations | | |

-->

## 6. Licensing

**WORK IN PROGRESS**

<!--
| Topic | Decision |
|---|---|
| Hardware license | `<CERN-OHL-S / CERN-OHL-W / TAPR / other>` |
| Software license | `<Apache-2.0 / GPL-3.0 / MIT / other>` |
| Documentation and data license | `<CC-BY-4.0 / CC0 / other>` |
| Open Source Hardware certification | `<OSHWA status>` |
| Licence compatibility of all dependencies | `<checked on YYYY-MM-DD, link to SBOM>` |
| Patents / IP / prior art | `<statement, defensive publication?>` |
| Contribution and governance model | `<link to CONTRIBUTING.md / GOVERNANCE.md>` |
| Liability and warranty disclaimer | `<statement, especially for any clinical use>` |
| Sourcing for builders | `<BOM with alternates, availability of parts worldwide>` |
| Quality of community-contributed changes | `<review gates, required tests, who can merge>` |

-->

## 7. Open issues and future work

| ID | Issue / TBD / TBC | Owner | Due | Impact if unresolved |
|---|---|---|---|---|
| OI-01 | | | | |

---

## 8. References

1. `<Applicable documents (binding): requirements spec, standards, regulations>`
2. `<Reference documents (informative): papers, datasheets, prior DSDs>`

---

## 9. Appendices

### Appendix A: Bill of materials

`<link to BOM file>`

<!-- FILL With relevant items>
