# Automotive Safety Integrity Level (ASIL)
Automotive Safety Integrity Level (ASIL) expresses the criticality associated with system functions.
It defines the safety requirements that system design and development must satisfy, ensuring sufficient safety for users (drivers, passengers, road users, etc.) even in the event of failure.

# ASIL Fundamentals
ASIL is not calculated for physical system components but for functions.
The ASIL associated with a function is inherited by the software and hardware elements that realize that function.
A single hardware or software component may implement multiple functions with different ASILs (e.g., a microcontroller).
The ASIL associated with hardware or software components is inherited from the highest ASIL function running on it.

![asil decomposition](https://github.com/user-attachments/assets/2d4ceade-0b25-43bf-9817-d1c8fa171d84)


# ASIL Reduction (Lowering ASIL)
In some cases, the ASIL can be reduced through the ASIL Decomposition technique.
This concept already exists in IEC 61508.
This can be advantageous.
For example, in terms of production cost—development is usually cheaper for a lower ASIL (labor, time, tools).
However, strict fundamental concepts and rules must be observed.
# ASIL Decomposition Fundamentals
An element implemented to address a given safety goal with a given ASIL can be decomposed into two independent elements that may have a lower ASIL.
Each must address the same safety goal.
Each must achieve the same safe state.
ASIL Decomposition can be used in the following phases:

Functional Safety Concept
System Design
Hardware Design
Software Design
ASIL Decomposition is a qualitative concept, addressing systemic issues (architecture) more than random errors (hardware reliability).

It can make the architecture more robust.
Similar to the IEC 61508 fault-tolerant architecture concept.

# Redundancy?
Is ASIL decomposition a way to introduce redundancy?
Yes and no.
Keep in mind that (in general) automotive systems actually have very little redundancy.
Only one air pump, only one battery, ...
Cost! This is widely accepted.
ASIL decomposition introduces functional redundancy.
Two independent architectural elements work toward the same (redundant) safety goal.
These independent architectural elements are almost always diverse.
Heterogeneous redundancy achieved through architectural design elements.
This is not the homogeneous hardware redundancy we typically consider in IEC 61508.

<img width="1003" height="930" alt="Gemini_Generated_Image_8uny6z8uny6z8uny" src="https://github.com/user-attachments/assets/49dd0047-a1ef-421b-8b33-ff1ea8e59963" />
# Case Study
Problem Description
Consider a function F that issues an activation command to actuator M ("Motor") upon a combined input from sensors S1, S2, ... Sn.
Assume the safe state of F is "M deactivated."
Assume Hazard and Risk Analysis has determined the ASIL of function F to be ASIL D.

<img width="1024" height="899" alt="Gemini_Generated_Image_n91znzn91znzn91z" src="https://github.com/user-attachments/assets/5765981d-e8dd-4d97-833b-caed32080ef9" />
Assume we have identified the following safety goal: "Avoid unintended M activation."
Therefore, "unintended" means "due to an erroneous combination of sensors S1, S2, ... Sn."
ASIL Allocation
Further assume sensors S1, S2, ... Sn measure different values.
That is, the sensors are independent and non-redundant.
Furthermore, in this scenario, we assume that each of these sensors, if faulty, could by itself lead to the violation of the safety goal.
Standard ASIL theory suggests that each sensor must therefore also inherit the ASIL D assigned to function F.

Initial Analysis 
At this point, we begin to analyze our architecture, inferring which elements in the actual architecture have the capability to violate the safety goal.
This may leverage specific knowledge of the technologies involved.
In this example, we theoretically know that controlling a brushless three-phase DC motor requires timely and precisely defined signals for the three phases.
Therefore, some components (e.g., the driver and its associated command channel), in the event of a failure, cannot produce the precise signals required for unintended activation of Motor.

Therefore, they cannot violate the safety goal by themselves.

[Note]: ASIL only analyzes failure scenarios. The safety goal is that the motor must stop immediately after a failure occurs. If the motor or motor driver is itself faulty, the motor will not run, which is inherently safe. This is easy to understand: if a car cannot start, it is safe. If, after starting, an emergency occurs, and we need the motor to stop, but it cannot stop due to a controller failure, that is unsafe.

<img width="895" height="1024" alt="Gemini_Generated_Image_ca05chca05chca05" src="https://github.com/user-attachments/assets/85fe7be4-93fd-49c2-9022-15dd057489a0" />

Lowering ASIL (ASIL)
Through this analysis, we are justified in reducing the ASIL of the driver, motor, and command channel to QM (Quality Management).
Note that this is entirely dependent on the technology; if the motor were based on continuous technology, it might not be possible to reduce the ASIL to QM.

Utilizing HARA Analysis
We now seek ways to improve the safety architecture by leveraging the results of the Hazard and Risk (HARA) analysis.
In its current form, the architecture only considers "erroneous sensor input," regardless of the operating scenario.

But what if the H&R analysis distinguishes between operating scenarios, such as vehicle speed? (This is typical).
Assume the H&R analysis yields the result that unintended M activation is only dangerous at speeds greater than a certain threshold.
(As another example, consider unintended airbag deployment—its effect depends on the vehicle speed).
Other typical operating scenario examples might be "driver's side door open" or "engine temperature greater than a certain threshold."
The results of the H&R analysis provide information we can utilize to introduce safety mechanisms into our architecture.

Introduction of a Safety Mechanism
We now introduce a safety mechanism: "Function M must not be activated when vehicle speed is greater than a specified threshold."
This effectively introduces an "AND" gate to reduce the possibility of unintended M activation.
Unintended M activation only occurs if: Function F fails AND v > threshold.

<img width="1024" height="1024" alt="Gemini_Generated_Image_eto3xfeto3xfeto3" src="https://github.com/user-attachments/assets/68e97824-8442-4a72-818b-8c9990a6e520" />


Safety Mechanism ASIL?
Note that we have now actually changed the architecture.
We have introduced a new sensor V.
We have introduced new software.
But have we changed the ASIL assignment?
The answer is "No."
Merely adding a safety mechanism does not change the ASIL assignment itself.

SW ASIL Decomposition?
We find that the ASIL D of the system software is too high, but we do not want to introduce hardware redundancy in the control logic. Therefore, we decide to apply ASIL decomposition at the software level.

Is the Software Independent?
Answer: The proposed software-level ASIL decomposition is only acceptable if the independence criteria are met.
This includes checking not only the software but also the hardware.
Furthermore: What about hardware metrics? Do they become ASIL QM or ASIL D? Or some combination based on percentages?
Answer: The hardware metrics are unaffected, so they remain ASIL D!
There are several issues:
How to share software resources with the underlying operating system?
Shared firmware?

How to share hardware resources like memory, ALU, etc.?
HW-Level Decomposition
Our analysis of software-level decomposition indicated too many problems, so we decide to perform hardware-level decomposition.

The Safety Element
What exactly is the Safety Element on the hardware side?
This is not necessarily a complete microprocessor.
It might be a Programmable Gate Array, essentially just a state machine, programmed only once, with no operating system—they cost only one-tenth of a full microprocessor and are very reliable, with their own clock and power supply, and are easy to manage.
No embedded logic—thus no software.
This has implications for the ISO 26262 safety process.
You no longer need Part 6, only Part 5 (Note: ISO 26262 Part 5 covers hardware, Part 6 covers software).
This is why it is simply called a Safety Element.
It depends on the safety function to be performed.
Lesson Learned: Hardware-level ASIL decomposition requires in-depth knowledge of available hardware characteristics, allowing for a correct balance of independence, functionality, and cost.
Alternative Decompositions?
The hardware-level decomposition we just introduced may be advantageous when we have many non-critical functions that can be limited to the microcontroller, and a limited number of safety-critical functions sharing the same safe state (driver deactivation).

What other possibilities are there for decomposing the original ASIL (D) across two elements?
Two possibilities:

ASIL B (assigned to μP) + ASIL B (assigned to Safety Element)
ASIL C (assigned to μP) + ASIL A (assigned to Safety Element)


First Decomposition (ASIL B + ASIL B)
The first alternative decomposition appears as having two essentially equivalent processors with redundant functionality.

But it is already an expensive solution on the hardware side (processors are much more expensive than, for instance, simple sensors).
Furthermore, the software must be developed using "diversity" techniques, which are also considered very costly.

Second Decomposition (ASIL C + ASIL A)
The final alternative represents an asymmetric layout again.

The software assigned to the main microprocessor implements the overall function of the controller.
The safety element is simple, inexpensive, and reliable.
Why not the reverse? Why not Processor ASIL A and Safety Element ASIL C—would that not typically be the intuitive choice?
Because in some cases, the safety element may be a legacy element from a previous design and is being used in this specific project.
It may be too simple to handle more complex safety functions.

Second Design

# Top Ten ASIL Misconceptions
10. ASIL deals only with hardware development.
ASIL has implications for hardware, software, and supporting processes.

9. A hardware element can be designed as ASIL X for any system.
A hardware element can be designed to satisfy the ASIL X safety requirements in a given system.

8. ASIL decomposition is a form of hardware redundancy.
Yes and no: ASIL decomposition implies functional redundancy, but also diversity, independence, and freedom from interference.

7. ASIL decomposition is used to lower HW metrics targets.
No! After ASIL decomposition, the same targets for the initial safety goal (before decomposition) apply to the decomposed HW/SW elements.

6. ASIL decomposition is mainly about random faults.
Correct for IEC 61508, but not for ISO 26262. It is more about addressing systemic issues (e.g., architecture).

5. The ISO 26262 standard requires ASIL decomposition.
It is actually not a mandatory step. It can be viewed as an opportunity to assign uniform functions with different safety criticality during SW partitioning to HW elements.

4. Software-level ASIL decomposition is simpler and cheaper than hardware-level decomposition.
In reality, software-level decomposition is often more difficult and more expensive than hardware-level decomposition due to strict requirements for diversity and independence.

3. ASIL decomposition is the only way to lower the ASIL of an element.
In fact, the ASIL of an element can sometimes be directly reduced through informed and careful analysis of the involved technology and architecture. Many people still do not realize this (e.g., classifying the motor in the example as QM).

2. ASIL decomposition is always feasible.
In reality, using multiple functions with many different ASILs (as in modern microcontrollers) may make achieving ASIL decomposition virtually impossible in some cases.

1. ASIL decomposition is always desirable.
In reality, there is always a cost-benefit trade-off, and after careful analysis, ASIL decomposition often proves to be unnecessary.

# Conclusion
ASIL decomposition can reduce the required ASIL of system elements (hardware and software).
It necessitates architectural decisions.
They may affect hardware, software, or both.
Sometimes these decisions are extremely difficult to evaluate.
Sometimes the problem exhibits independence, and too much effort is required to justify the cost.
There are many factors to consider!
