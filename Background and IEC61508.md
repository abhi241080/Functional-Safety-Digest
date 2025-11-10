1. Functional Safety Overview
1.1. Definition
"Absence of unreasonable risk due to hazards caused by malfunctioning behavior of electrical/electronic systems" – ISO 26262-1:2011; 1.51 Safety = Absence of unreasonable risk Absence of unreasonable risk due to hazards caused by the failure of electronic/electrical (E/E) systems.

Functional safety arose due to the increasing use of E/E systems in control systems. Currently, functional safety is applied across many industries, including avionics, automotive, railway, and medical devices. The most general standard is IEC 61508, which originated in the industrial market. It currently exists as a standard in IEC/ISO basic safety publications, covering "general functional safety" for several industries. ISO 26262 is specifically applicable to the electrical and electronic systems of automotive passenger vehicles. The ISO 26262 standard is a derivation of the IEC 61508 standard.

Note that functional safety and ISO 26262 do not consider all risks in E/E systems. They only consider the reduction of risk from the malfunctioning behavior of E/E systems.
1.2. Vehicle Safety vs. Automotive Functional Safety
Vehicles comprise many different systems, including hydraulic, mechanical, electrical, electronic, and chemical systems. There are various automotive standards that cover topics related to safety and best design practices. Examples can be found on the Society of Automotive Engineers (SAE) website. Governments worldwide even have their own safety standards, such as the U.S. government's Federal Motor Vehicle Safety Standards and the United Nations World Forum for Harmonization of Vehicle Regulations.

Functional safety, which focuses only on the failure of electrical and electronic systems, is just one part of overall automotive safety.

1.3. Functional Safety vs. Nominal Performance
Functional safety also does not address nominal performance. For example, the nominal performance of an automatic braking system might be the vehicle coming to a complete stop within 5 seconds when traveling at 60 mph. Functional safety does not determine whether the vehicle brakes within 5 seconds.

Instead, functional safety should focus on failures, such as the braking function activating when it should not, or the system braking so forcefully that it injures the driver. Nominal performance issues can still lead to safety concerns, but nominal performance is not part of the functional safety standard.

1.4. What Functional Safety Is
At the most basic level, functional safety comprises three elements:

Identification of potential problems that could cause physical harm to people, which are referred to as hazards.

Assessment of the risk caused by these hazards.

Utilizing system engineering knowledge to reduce the risk to an acceptable level.

1.5. What Degree of Risk is Reasonable?
History of the Seatbelt: Many credit Karl Benz with inventing the automobile around 1885. Seatbelts did not appear until the 1920s when doctors began installing them in their own cars. Automobile companies did not offer seatbelts as optional equipment until the 1950s, and they became standard equipment only at the end of the 1950s. In 1968, the United States passed a law requiring seatbelts in all vehicles. Australia became the first country to mandate seatbelt use in 1970. In 1984, New York was the first U.S. state to pass similar legislation. Thus, it took nearly a century for countries to mandate seatbelt use.

If a seatbelt system were purely mechanical, it would not be considered part of functional safety. If it contains electronic components, it falls under functional safety.

In summary, functional safety focuses on keeping risk below the current societal tolerance threshold.

1.6. Complete Risk Elimination
Why attempt only to reduce risk to a reasonable level instead of eliminating all risk? Eliminating all risk would paralyze engineering, product development, and testing. Unfortunately, ensuring that people are never injured in a car accident is likely impossible. On the other hand, failing to eliminate sufficient risk could lead to product failure, recalls, litigation, and even loss of life. Ultimately, every company must decide for itself how much risk to eliminate from its products.

1.7. What Does "Functional" Mean?
The term "functional" comes from a branch of system engineering called requirements engineering. System engineering divides requirements into:

Functional Requirements: What the system should do; in other words, the system's function. The form is "System X shall do Y". For example, "The turn signal system shall turn on the indicators to inform the driver the system is active".

Non-Functional Requirements: How the system should perform; for example, how reliable the system is. The form is "System X shall be Y". For example, "The turn signal system shall be available when the vehicle ignition is in the 'on' position".

Functional safety focuses on what happens when the system does something it should not—known as a malfunction. When adding new engineering requirements to vehicle design to ensure system safety, the word "shall" frequently appears in the functional safety module. This is because engineering requirements almost always contain the word "shall". If you see the word "shall," you are likely looking at an engineering requirement.
1.8. ASIL
Severity, the probability of exposure to the hazardous event, and controllability provide a systematic method for risk assessment. These three factors are combined into a single metric called the Automotive Safety Integrity Level (ASIL).Note that these three factors are classified into levels.ASIL measures the magnitude of risk in each hazardous situation. The lowest risk level is ASIL A, and the highest risk level is ASIL D. A quick grading method is 6, 7, 8, 9 $\rightarrow$ A, B, C, D.
