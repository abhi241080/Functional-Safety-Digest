4. Safe and Reliable Embedded System Design
4.1. Common Techniques
The figure below illustrates common techniques used to achieve functional safety, which falls under the scope of reliability design. Many existing reliability design patterns can be directly used in the design of safety-critical systems. Each design method has different advantages and disadvantages, requiring trade-offs based on the specific application.
<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/009cdd0c-f7de-4500-9f90-321d4f28e024" />
4.2. Hardware Patterns
   <img width="1518" height="501" alt="image" src="https://github.com/user-attachments/assets/373064a8-5c77-4d49-9c41-dac44aa0e73a" />

4.2.1. Homogeneous Duplex Pattern
Summary: This is a hardware pattern that improves system safety and reliability by providing a duplicate of the same module (Modular redundancy) to handle random faults. The Homogeneous Duplex Pattern consists of two identical modules (channels): an Active module and a Standby module. This pattern can be applied at any level of system design, from the complete system (channel) down to a single component. The idea behind this pattern is based on the assumption that two identical components are unlikely to experience a random fault simultaneously.

Problem Solved: How to handle random faults and how to enable the system to continue operating when one of its components fails, thereby improving system safety and reliability.

Disadvantages and Side Effects: The main drawback of this pattern is that it is not suitable for handling systematic faults, as both channels are identical and share the same potential systematic failures. In the event of a systematic fault, the Switch circuit, designed to select between the two channels, will switch to one of the two faulty channels, leading to a complete system failure. When a fault is detected in the active channel, the switch circuit transfers to the standby channel, which may involve the loss of computation steps and potentially the loss of input data, especially if there is no recovery time to redo the calculation.

4.2.2. Heterogeneous Duplex Pattern
<img width="1518" height="499" alt="image" src="https://github.com/user-attachments/assets/e95b6300-c484-48a7-ba6c-d858eecdd76f" />

Summary: This is a hardware pattern used to handle random and systematic faults by providing a heterogeneous duplicate for the required module, thereby improving system safety and reliability. The Heterogeneous Duplex Pattern consists of two independent and different modules (channels): an Active module and a Standby module. Due to high development costs, this is one of the most expensive patterns. It can be applied at any level of system design, from the complete system (channel) down to a single component. The two modules should be designed and implemented independently of each other.

Problem Solved: How to handle systematic faults as well as random faults, and how to enable the system to continue operating when one of its components fails, thereby improving system safety and reliability.

Disadvantages and Side Effects: The main drawback is the very high duplication and development costs of this pattern. When a fault is detected in the active channel, the Switch circuit transfers to the standby channel, which may involve the loss of computation steps and potentially the loss of input data, especially if there is no recovery time to redo the calculation.

4.2.3. Triple Modular Redundancy Pattern
Aliases: 2-oo-3 Redundancy Pattern, Homogeneous Triplex Pattern
<img width="1518" height="499" alt="image" src="https://github.com/user-attachments/assets/fb743eda-4869-4305-ae8e-9dc517b6acf4" />

Summary: This pattern is a variant of homogeneous hot redundancy, consisting of three identical modules operating in parallel to detect random faults to enhance the reliability and safety of systems with no fail-safe-state. The modules operate in parallel to produce three results, which are compared using a voting system to produce a common result, as long as two or more channels have the same result. This structure allows the system to operate and provide functionality in the presence of a random fault without losing input data.

Problem Solved: How to handle random faults and single point failures to improve system safety and reliability without losing input data in case of a fault.

Disadvantages and Side Effects: The main drawback of this pattern is that it is not suitable for handling systematic faults. In this case, the three channels are identical and share the same potential systematic failures, and the system will continue to work, producing invalid data. To handle single-point failures in input sensors, three independent sensors can be used. This option may lead to problems, especially with different sensors having different response speeds.

4.2.4. M-Out-Of-N Pattern
<img width="1518" height="482" alt="image" src="https://github.com/user-attachments/assets/db334228-b598-4bbf-89a3-eabc80b2316b" />

Summary: The M-oo-N pattern includes homogeneous hot redundancy. It consists of N identical modules (channels) working in parallel to mask random faults and enhance system safety and reliability. M-oo-N redundancy requires at least M components in the N parallel modules to be successful for the system to be considered successful. An M-out-of-N voting algorithm is used in the Voter component to allow system operation and provide the required functionality in the presence of random faults without losing input data.

Problem Solved: How to handle random faults to improve system safety and reliability without losing input data.

Disadvantages and Side Effects: The main drawback of this pattern is that it is not suitable for handling systematic faults, as the channels are identical and share the same potential systematic failures. Therefore, the M-oo-N pattern cannot detect this type of fault, meaning the system continues to produce invalid data. If multiple sensors are used to handle single-point failures in input sensors, this solution may lead to problems, especially with different sensors having different response speeds.

4.2.5. Monitor-Actuator Pattern
<img width="1518" height="537" alt="image" src="https://github.com/user-attachments/assets/4645cea9-0a02-49c5-9e6d-dde29ebbbe03" />

Summary: The Monitor-Actuator pattern is a special type of heterogeneous redundancy suitable for safety-critical systems with low availability requirements and a fail-safe state, which is a known condition where the system is always safe. The MA pattern consists of two different channels (modules): a main channel, called the actuation channel, which performs the primary actions, such as controlling some actuators, and a monitoring channel, which provides supervision of the actuation channel to detect and identify possible faults, and then force the actuation channel into the fail-safe state. The monitoring channel is isolated from the actuation channel so that if it contains any fault, the actuation channel continues to operate correctly.

Problem Solved: How to enhance the safety of an embedded system when a single-point fault occurs, for systems that include a fail-safe state and have low availability requirements at a reasonable cost.

Disadvantages and Side Effects: The main drawback of this pattern is that it is not suitable for applications with high availability requirements.

4.2.6. Watchdog Pattern
<img width="1518" height="386" alt="image" src="https://github.com/user-attachments/assets/268519e6-448e-452b-874c-346a26073d29" />

Summary: The Watchdog pattern is a very lightweight and inexpensive pattern with minimal coverage, used to check the internal computational execution of the executing channel. It is widely used in embedded systems to ensure that time-dependent computational processing proceeds correctly according to a predetermined sequence [37]. This pattern includes a component called a watchdog, which receives periodic messages from the monitored channel. If events occur too late or out of sequence, the watchdog issues a shutdown signal or a corrective action to avoid losing control of the system. The Watchdog pattern is rarely used alone in safety-critical systems and is often combined with other patterns to improve system safety.

Problem Solved: How to add additional safety to the system by providing a simple and low-cost way to observe, detect, and handle faults during execution. How to ensure that the internal computational processing of the executing channel proceeds correctly as expected. How to identify time-based faults. How to improve deadlock detection using a "Keyed Watchdog."

Disadvantages and Side Effects: The main drawback of this pattern is the minimal coverage of the watchdog; therefore, it is rarely used alone in safety-critical systems. It can be combined with other safety patterns.

4.3. Software Patterns
<img width="1518" height="362" alt="image" src="https://github.com/user-attachments/assets/813084d0-82bd-4bdc-b84c-de24f7540865" />

4.3.1. N-Version Programming PatternSummary: N-Version Programming (NVP) is a well-known fault-tolerant software approach based on software diversity and fault masking. It is defined as N $\ge 2$ functionally equivalent software modules, called "versions," independently generated from the same initial specification. This pattern involves N programs running in parallel, performing the same task on the same input to produce N outputs. A Voter is used in this pattern to produce the correct output; it accepts the N results as input and uses them to determine the correct output based on a specific voting scheme.Problem Solved: How to overcome software faults that may exist after software development to improve software reliability and safety.Disadvantages and Side Effects: The main drawbacks of the NVP pattern are the complexity of developing independent N versions, the high development cost, and the high dependence on the initial specification, which may propagate related faults to all versions. The problem of related faults in all versions is critical for the safety and reliability of this pattern.

4.3.2. Recovery Block Pattern
<img width="1518" height="609" alt="image" src="https://github.com/user-attachments/assets/15bed67b-56e4-45e3-aff0-9c9b7f00875f" />

Summary: The Recovery Block (RB) is a well-known fault-tolerant software approach. It is based on fault detection with an acceptance test and backward error recovery to avoid system failure. Similar to N-Version Programming, the Recovery Block pattern includes N different, independent, functionally equivalent software modules, called "versions," from the same initial specification. These modules are divided into a primary version and N-1 alternate versions, where the execution of any version is followed by an acceptance test. The primary version (Block) executes first, followed by the acceptance test. A test failure will lead to the execution of an alternate standby version (Recovery Block), followed by an acceptance test. The last step is repeated until one of the alternates passes its acceptance test, or all alternates execute without passing the test, and a complete system failure is reported.

Problem Solved: How to overcome software faults that may exist after software development to improve software reliability and safety.

Disadvantages and Side Effects: The main drawbacks of RB are the high dependence on the quality of the acceptance test and the impact of sequential execution time, which may include service interruptions during recovery. In addition, it shares common drawbacks with the NVP pattern, such as the complexity of developing independent N versions, the high development cost, and the high dependence on the initial specification, which may propagate software errors to all versions.

4.3.3. Acceptance Voting Pattern
<img width="1518" height="399" alt="image" src="https://github.com/user-attachments/assets/1fab92b1-1a65-4f3d-8095-42d3635014ac" />

Summary: The Acceptance Voting (AV) pattern is a hybrid pattern that combines the NVP pattern with the acceptance test used by the RB pattern. Similar to NVP, this pattern is based on the independent generation of N $\ge 2$ functionally equivalent software modules, called "versions," from the same initial specification [13]. This pattern involves N programs running in parallel, performing the same task on the same input to produce N outputs. The output of each version passes an acceptance test to check its correctness. The outputs that pass the acceptance test are used as input to a dynamic Voter, which executes based on a voting scheme to produce the correct output.Problem Solved: How to overcome software faults that may exist after software development to improve software reliability and safety.Disadvantages and Side Effects: Similar to the original N-Version Programming method, the drawbacks of the AV pattern include the complexity of developing N different software versions, in addition to the high dependence on the initial specification that may propagate faults to all versions. Regarding safety, the problem of related faults in all versions is less critical in this pattern, as the acceptance test represents an additional measure for detecting these faults.

4.3.4. N-Self Checking Programming Pattern
<img width="1518" height="563" alt="image" src="https://github.com/user-attachments/assets/bc2f76da-9865-498c-abc1-4caa89af4a77" />

Summary: N-Self Checking Programming (NSCP) is one of the most expensive fault-tolerant software approaches. It is based on software design diversity and self-checking provided by adding redundancy to the program so that it can check its own dynamic behavior during execution. This pattern includes the independent generation of N $\ge 4$ functionally equivalent software modules, called "versions," from the same initial specification. These versions are arranged in groups called components, where each component consists of two versions and a comparison algorithm to compare the correctness of the results of the two versions. During execution, one component works as the active component to provide the required service, while the others are hot standbys. To provide fault tolerance for a single fault, at least 4 versions must be executed on 4 hardware units, which represents the highest cost compared to other methods.Problem Solved: How to overcome software faults that may exist after software development to improve software reliability and safety.Disadvantages and Side Effects: High dependence on the initial specification, which may propagate faults to all versions. Uses a large number of different versions and hardware modules compared to other patterns that tolerate the same number of faults. Complexity of developing N independent and functionally equivalent versions.

4.3.5. Recovery Block with Backup Voting Pattern
<img width="1518" height="571" alt="image" src="https://github.com/user-attachments/assets/98def840-355b-48dd-9b0c-db5d0ce6457a" />

Summary: The Recovery Block with Backup Voting (RBBV) is a hybrid pattern that combines the concepts of classic RB with NVP to improve the reliability of classic RB in situations where it is difficult to construct an effective acceptance test. This software pattern includes N different, independent, functionally equivalent software modules, called "versions," from the same initial specification. It addresses the problem of the false negative case, which involves the acceptance test incorrectly considering a correct output as an error output. The primary version executes first, followed by the acceptance test. When the first version fails the acceptance test, a copy of the result is stored as a backup in a cache, and the next alternate version is called to execute the required function. This is repeated until one of the alternates passes its acceptance test. If all alternates execute without passing the acceptance test, the stored values are used as input to a voting method as a last attempt to provide a valid result.

Problem Solved: How to overcome software faults that may exist after software development to improve software reliability, and how to address the false negative cases problem in a weak acceptance test.

Disadvantages and Side Effects: Similar to classic RB, the main drawbacks of this pattern are the complexity of developing independent N versions, the high development cost, and the high dependence on the initial specification, which may propagate faults to all versions.

4.4. Composite Patterns
<img width="1518" height="283" alt="image" src="https://github.com/user-attachments/assets/9c8b441f-7093-484b-a295-15c2a3789283" />

4.4.1. Protected Single Channel Pattern
Summary: The Protected Single Channel pattern is a lightweight pattern used to enhance safety and reliability by adding checks and monitoring at different points in the channel. It is typically used to handle transient faults.

Problem Solved: How to handle transient faults to provide a certain level of safety and reliability for embedded systems in an inexpensive way.

Disadvantages and Side Effects: The main drawback of this pattern is that it is not suitable for handling persistent faults, as it cannot continue safe operation and can sometimes lead to the loss of the entire system.

4.4.2. 3-Level Safety Monitoring Pattern (E-Gas)
<img width="1518" height="495" alt="image" src="https://github.com/user-attachments/assets/bcb8cd8a-126e-47ec-9227-8efecb5814a1" />

Summary: The 3-Level Safety Monitoring Pattern is considered a combination of the Monitor-Actuator pattern and the Watchdog pattern, suitable for applications that require continuous safety monitoring and include a fail-safe state without high hardware redundancy. It consists of a single hardware channel including 3 levels: Execution (Functional), Monitoring, and Control levels. The Functional Level executes sub-programs to perform the intended function, while the Monitoring Level supervises the first level, and the Control Level controls the Monitoring Level and the entire hardware channel. Additionally, a Watchdog, which communicates with the Control Level through periodic messages, is used to reset the system to its fail-safe state in case of failure.

Problem Solved: How to enhance the safety of an embedded system at a low, reasonable cost if a fault occurs in a system with a fail-safe state. Furthermore, how to continue providing the required level of safety and ensure the system does not cause harm or damage when there is any deviation between the actuator output and the command setpoint.

Disadvantages and Side Effects: The main drawback of this pattern is that it includes a single hardware channel; therefore, it cannot be used to tolerate hardware faults in applications with high reliability and availability requirements.

4.4.3. E-Gas Explained Again
Here is a better document explaining the E-Gas system: Design patterns are proven, mature architectures. One of the most common software patterns in the automotive industry is called E-Gas. The E-Gas pattern defines three software levels.

The first level is the Functional Level, which contains the system's intended function. For the lane assist example, the intended function would be to steer the wheel towards the center and adjust the steering based on camera data. The second and third levels of the E-Gas pattern are used for monitoring.
<img width="918" height="498" alt="image" src="https://github.com/user-attachments/assets/69d5f5f3-250b-4eeb-888b-d0bc5c64bda2" />

The Second Level refers to functional monitoring that monitors the first level for any safety objective violations. For example, the second-level software would monitor the time and torque requests of the lane assist system.
<img width="883" height="462" alt="image" src="https://github.com/user-attachments/assets/157210ba-47fb-4a04-bff5-dbe3f7269b25" />

The Third Level is used for processor monitoring of key hardware components. For example, the third-level software initiates monitoring of RAM, ROM, and control flow.

In the lane assist example, the Level 1 software sends torque requests to the steering wheel motor.

If the Level 2 or Level 3 software detects an error, the Level 1 software is disabled, and the Level 2 and Level 3 software puts the system into a safe state. In this case, we determine the safe state to send zero torque output.
<img width="901" height="459" alt="image" src="https://github.com/user-attachments/assets/2490c39a-710a-4d09-9dd6-3dca7934ed6d" />

4.4.3.1. Where Did the Name E-gas Come From?
The E-gas design pattern originally came from a drive-by-wire accelerator system. Initially, the accelerator pedal in a car had a direct mechanical connection to the throttle valve on the engine. The throttle valve regulates the amount of air entering the engine. In modern cars, the accelerator pedal is an electronic sensor. When you press the accelerator pedal, software interprets how much you want to accelerate. The software then opens or closes the throttle valve. The E-gas software pattern was developed to monitor faults in the drive-by-wire accelerator system. In the event of a fault in the gasoline engine system, the Level 2 or Level 3 monitoring function can reduce the throttle.

4.4.3.2. Software Partitioning and Safety Monitoring
Safety monitoring and software partitioning are software mechanisms typically addressed using design patterns. For safety monitoring, there are specific patterns, such as the explained E-GAS concept. In the case of software partitioning, one pattern is to use hardware features called MPU (Memory Protection Unit) and dual data storage.

