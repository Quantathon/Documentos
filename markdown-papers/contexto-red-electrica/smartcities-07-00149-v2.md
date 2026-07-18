# smartcities-07-00149-v2

## Article

## A Novel IoT-Based Controlled Islanding Strategy for Enhanced

## Power System Stability and Resilience

**Aliaa A. Okasha 1,2,* , Diaa-Eldin A. Mansour 1,3 , Ahmed B. Zaky 4,5 , Junya Suehiro 6 and Tamer F. Megahed 1,7** 1 Department of Electrical Power Engineering, Faculty of Engineering, Egypt-Japan University of Science and Technology (E-JUST), Alexandria 21934, Egypt; diaa.mansour@ejust.edu.eg or mansour@f-eng.tanta.edu.eg (D.-E.A.M.); tamer.megahed@ejust.edu.eg (T.F.M.) 2 Department of Electrical Power and Machines Engineering, Faculty of Engineering, Aswan University, Aswan 81542, Egypt 3 Department of Electrical Power and Machines Engineering, Faculty of Engineering, Tanta University, Tanta 31733, Egypt 4 Computer Science and Information Technology Programs (CSIT), Egypt-Japan University of Science and Technology (E-JUST), Alexandria 21934, Egypt; ahmed.zaky@ejust.edu.eg 5 Faculty of Engineering at Shoubra, Benha University, Benha 11629, Egypt 6 Faculty of Information Science and Electrical Engineering, Kyushu University, 744 Motooka, Nishi-ku, Fukuoka 819-0395, Japan 7 Electrical Engineering Department, Faculty of Engineering, Mansoura University, Mansoura 35516, Egypt ***** Correspondence: aliaa.arafa@aswu.edu.eg **Highlights: What are the main findings?**
- A novel IoT-based intentional controlled islanding strategy was developed as a defensive action

against blackout events, enhancing grid resilience by detecting post-disturbance changes in the network.
- An adaptive coherency index was developed to assess the coherency between generator pairs

using real-time PMU measurements, followed by a clustering technique to identify islands and associated generators. **Citation:** Okasha, A.A.; Mansour,
- A Mixed-Integer Linear Programming (MILP) model was proposed to identify optimal trans-

D.-E.A.; Zaky, A.B.; Suehiro, J.; mission lines for disconnection with minimal power disruption, aiming to create stable islands. Megahed, T.F. A Novel IoT-Based
- Optimal generation rescheduling and/or load shedding were implemented to improve voltage

Controlled Islanding Strategy for and frequency stability within subsystems. Enhanced Power System Stability and Resilience. *Smart Cities* **2024**, *7*, **What is the implication of the main finding?** 3871–3894. https://doi.org/10.3390/
- The integration of IoT technology in the islanding strategy significantly increases the effective-

smartcities7060149 ness of grid management during disturbances, potentially reducing the likelihood of widespread Academic Editor: Pierluigi Siano blackouts.
- The adaptive coherency index enhances real-time monitoring capabilities, allowing for better

Received: 8 July 2024 Revised: 11 November 2024 decision-making in dynamic grid conditions. Accepted: 19 November 2024
- The MILP model provides a systematic approach to controlled islanding, which can be applied

Published: 10 December 2024 to real-world scenarios, ensuring stable operations with minimal service interruption.
- Improved voltage and frequency stability through strategic rescheduling and load shedding

contributes to the overall reliability and performance of the power grid. **Copyright:** © 2024 by the authors. Licensee MDPI, Basel, Switzerland. **Abstract:** Intentional controlled islanding (ICI) is a crucial strategy to avert power system collapse This article is an open access article and blackouts caused by severe disturbances. This paper introduces an innovative IoT-based ICI distributed under the terms and strategy that identifies the optimal location for system segmentation during emergencies. Initially, conditions of the Creative Commons the algorithm transmits essential data from phasor measurement units (PMUs) to the IoT cloud. Attribution (CC BY) license (https:// Subsequently, it calculates the coherency index among all pairs of generators. Leveraging IoT creativecommons.org/licenses/by/ technology increases system accessibility, enabling the real-time detection of changes in network 4.0/). *Smart Cities* **2024**, *7*, 3871–3894. https://doi.org/10.3390/smartcities7060149 https://www.mdpi.com/journal/smartcities

*Smart Cities* **2024**, *7* 3872 topology post-disturbance and allowing the coherency index to adapt accordingly. A novel algorithm is then employed to group coherent generators based on relative coherency index values, eliminating the need to transfer data points elsewhere. The “where to island” subproblem is formulated as a mixed integer linear programming (MILP) model that aims to boost system transient stability by minimizing power flow interruptions in disconnected lines. The model incorporates constraints on generators’ coherency, island connectivity, and node exclusivity. The subsequent layer determines the optimal generation/load actions for each island to prevent system collapse post-separation. Signals from the IoT cloud are relayed to the circuit breakers at the terminals of the optimal cut-set to establish stable isolated islands. Additionally, controllable loads and generation controllers receive signals from the cloud to execute load and/or generation adjustments. The proposed system’s performance is assessed on the IEEE 39-bus system through time-domain simulations on DIgSILENT PowerFactory connected to the ThingSpeak cloud platform. The simulation results demonstrate the effectiveness of the proposed ICI strategy in boosting power system stability. **Keywords:** controlled islanding; IoT-based strategy; coherency index; islanding boundaries; mixed integer linear programming; power flow interruption; system collapse prevention **1. Introduction* 1.1. Motivation* Electrical power grids are vulnerable to a wide range of disturbances caused by human activities, equipment malfunctions, or natural disasters [1]. Modern power systems are equipped with advanced protection and control systems to address these challenges. However, factors such as network expansion, renewable energy integration, and increased power consumption have reduced the power system’s stability margin [2,3]. Rotor angle stability is a critical aspect of power system operation that indicates the robustness of generators against synchronization loss [1]. It can be defined as the ability of generators to restore synchronization and return to a stable operating state following the disturbance. When a power system experiences a disturbance, such as a fault on a transmission line or sudden loss of generation, the rotor angles of the synchronous generators change rapidly, leading to electromechanical oscillations. These oscillations can be local or inter-area, posing risks of unintentional islanding and blackouts with severe societal and economic consequences [4,5]. Several blackouts have been reported in many countries, such as those seen in Bangladesh, Australia, Turkey, India, the US, Canada, and Italy over the past two decades [6–9]. Effective control systems are essential to dampen oscillations within an acceptable timeframe to ensure stable system operation [6]. Emergency control approaches can be classified into device-based techniques (e.g., power system stabilizers, flexible AC transmission systems, HVDC), operational-based techniques (e.g., load shedding, generation adjustments), and configurational-based techniques [10,11]. Implementing these strategies is crucial for maintaining system stability and preventing widespread blackouts. *1.2. Solving Power Grid Instabilities* One of the effective means used to maintain system stability and prevent widespread blackouts is intentional controlled islanding (ICI). It involves dividing the network into isolated islands, each containing generators with strong coupling, known as coherent generators [1,12]. Successful ICI involves addressing three key sub-problems:
1. When, determining the necessity of applying ICI to prevent widespread instability

and system collapse.
1. Where, identifying optimal locations for network separation, including the appropri-

ate number of islands and the right transmission lines to be disconnected.

*Smart Cities* **2024**, *7* 3873
1. What, implementing optimal measures to maintain stability within islands after

separation. Recently, there has been a significant shift toward smart grids in response to the growing challenges faced by traditional power systems. Smart grids feature real-time monitoring and autonomous control, facilitated by integrating the Internet of Things (IoT) into power networks. The IoT enables seamless real-time monitoring and control of all system components, offering vast potential for the future of smart grids. The data extracted from real-time monitoring will enable proper system analysis, performance evaluation, decision-making, and predictive maintenance. In addition, these data can be used to train artificial intelligent models with minimal human involvement. By incorporating these smart grid features, ICI can be enhanced in terms of reliability, efficiency, and coordination. *1.3. Literature Review* The “where to island?” stage in the ICI process involves two main steps: coherency assessment, and determination of optimal splitting points in the system. Coherency assess- ment is a crucial task for a successful ICI scheme. In a multi-machine system, coherent generators exhibit similar dynamic trends when undergoing major disturbances. Therefore, keeping coherent units in the same island after separation is necessary to maintain the rotor angle stability in each subsystem [12]. Generally, coherency identification strategies can be classified into: model-based and measurement-based techniques. The former category involves traditional coherency assessment approaches. The core of model-based methods is developing reduced-order models that describe the dynamic behavior of generators in the system. The most common approaches in this category are slow coherency-based strategies, as they have been employed in several studies [13,14]. Other methods, such as Epsilon decomposition, relation factor, and singular points, can also be applied to identify coherent generators [15]. Model-based methods for coherency analysis often encounter several challenges. Firstly, obtaining an accurate model for a large interconnected power system is a complex procedure. Moreover, most of these techniques are based on linearized models. Since linearization is performed around a stable operating point, any deviation from this point could affect the validity of these models [12]. Also, some generator details would be neglected in linearization, which can affect the model’s accuracy. Therefore, measurement-based approaches present superior alternatives for identifying coherent gen- erators under various scenarios. These approaches rely on data-driven methods that utilize measurements acquired from PMUs installed at optimal locations within the electrical power network. In this category, some researchers have employed signal processing tech- niques, such as Hilbert–Huang transform, to evaluate the similarity between generators’ responses [16]. However, these methods can be computationally extensive, and the com- plexity of the signals can greatly influence their performance. Therefore, other researchers suggested applying optimization techniques to solve the coherency problem, such as parti- cle swarm optimization (PSO), followed by the K-means clustering technique [17]. Similarly, researchers in [18] combined modified PSO with a fuzzy c-means classifier. The major drawback of these approaches is their computational cost, which makes them unsuitable for online applications. Machine learning (ML) algorithms could also be implemented to detect coherent generators. Some authors have used support vector machine algorithms [19,20], while others have applied decision trees [21]. The scholars in [22] combined artificial neural networks and K-means algorithms for coherency analysis. Moreover, the authors in [23] proposed an online classifier based on dynamic neural networks. Despite the efficiency of ML models, they are trained on specific datasets. Therefore, experiencing considerably different cases due to variations in the system structure or its operating conditions may lead to inaccurate results. Statistical techniques represent another alternative for coherency analysis. The Pearson correlation coefficient has been extensively used in the literature to determine the degree of closeness between system generators [24–30]. Although it is a straightforward method

*Smart Cities* **2024**, *7* 3874 to implement, its accuracy is strongly influenced by the window size and the number of samples in the dataset. Also, it assumes a linear relationship between variables, so it may fail in handling nonlinear relationships. The authors in [20] employed an efficient index to measure the strength of the generators’ coupling. This similarity index considers the inertia of generators, power network parameters, as well as the generators’ voltages and rotor angles, which are commonly used in other coherency indices. In this work, the authors developed this index to evaluate generators’ coherency while considering changes in network topology that affect the resulting similarity indices. Integrating IoT technology into the electrical power network for monitoring and control enables the system to detect changes in the network structure, thereby significantly enhancing the accuracy of coherency analysis. The second task in the “where to island?” subproblem, following the coherency assessment stage, involves identifying the optimal locations for network separation. Several methods have been applied in previous studies to find the optimal splitting points that achieve stable operation of the isolated areas. These approaches can be categorized into graph-based methods, ML-based approaches, and optimization algorithms. In this stage, minimum load-generation imbalance, minimum load shedding, or minimum power flow disruption (PFD) are mostly adopted as objective functions. Numerous studies have applied graph-based methods to solve the “where to island” task. For example, authors in [31] proposed a three-stage ICI algorithm based on the depth-first search method. This strategy identified the optimal boundary lines subjected to minimum PFD. Spectral clustering has also been widely used to determine proper island boundaries, as illustrated in [32], where a constrained hierarchical spectral clustering strategy is proposed, considering active PFD as its objective function. A spectral clustering algorithm combined with the k-medoids algorithm was presented in [33]. To enhance the clustering performance, the authors in [34] implemented a multi-layer spectral clustering strategy instead of a single-layer strategy to consider frequency similarity, and active and reactive PFD simultaneously. A similar strategy was proposed in [24], where each criterion (frequency similarity, active PFD, and reactive PFD) was evaluated separately before comparing the results with those obtained from a combined criteria strategy. In other approaches [35–37], ordered binary decision diagrams (OBDDs) were used to formulate efficient splitting strategies. These techniques simplified the network graph according to either system topology or control areas, allowing only multi-regional transmission lines to be disconnected once synchronization is lost in the system. However, it is important to note that overly simplified graphs may result in significant load-generation imbalances within each island. Optimization-based techniques could also be applied to solve the network partitioning issue. An algorithm named tabu search was applied in [38] to determine the optimal borders of islands. The proposed solution abides by connectivity and stability constraints. The ICI problem could also be addressed through the application of metaheuristic algorithms, such as the modified shuffled frog leap technique in [39] and particle swarm optimization in [40]. The objective function of the above-mentioned studies is to minimize load-generation imbalance in all islands, followed by load flow analysis to evaluate the feasibility of the solution. However, attaining the global optimal solution using such strategies is not assured, and computational costs can be another burden associated with this category. In contrast, deterministic optimization techniques can overcome the drawbacks of the stochastic optimization algorithms in ICI. A controlled islanding strategy based on linear programming was suggested in [41]. The authors in [42] proposed a strategy that used a second-order cone program constrained by the frequency stability of islands. Also, mixed integer linear programming (MILP) has been used for this task, as seen in [14,27,43–45]. Load shedding minimization was the objective function of the MILP models developed in [14]. The authors compared the performance of the proposed models with AC and DC load flow under different numbers of islands. The DC load flow was based on simplified models, which significantly reduced the execution time while compromising the results’

*Smart Cities* **2024**, *7* 3875 accuracy. On the contrary, the AC load flow gave precise results. However, computational complexity and convergence issues are the main drawbacks of these algorithms. Thus, linearized AC load flow can provide a compromise between accuracy and computational efficiency for real-time applications. In [45], a two-stage splitting strategy formulated as an MILP model satisfied the linearized AC load flow equations, aiming to minimize the difference between generation and demand in each island while preserving the subsystems’ stability. Therefore, an energy function is employed in the next stage to assess the transient stability of the islands. Despite the efficiency of this method, the iterative nature of this strategy may increase the computational time, threatening the system’s integrity under severe disturbances. In the present study, the authors aim to present a solution based on the MILP constrained to the linearized AC load flow, coherency, and connectivity constraints. For the proposed model, a minimum PFD is selected as an objective function. The main feature of this objective function is preserving the power flow patterns in transmission lines, thereby enhancing the transient stability of the separated islands. Additionally, it effectively mitigates the risk of transmission line overload and facilitates the smooth grid restoration process. The last stage in ICI is determining the optimal measures required in each subsystem, as finding the optimal cut-set alone may not ensure generation-load balance in the created islands. When the network is split into multiple regions, certain regions may have an excess of generation, while others may experience surplus loading levels. Therefore, additional control strategies, such as load shedding and/or generation adjustment, must be implemented in each area to ensure the stable operation of the islands. *1.4. Contributions* This paper presents a novel IoT-based ICI strategy as a defensive action against black- out events. Firstly, real-time data of the power network are gathered from the distributed sensors and PMUs, which are then transmitted to the IoT cloud for system status investiga- tion. Secondly, a non-model-based similarity index for coherency assessment is developed. This index considers any topological changes that occur due to disturbances. Then, a two-layer optimization model is proposed to handle the “where to island?” and “what post-islanding measures are required?” tasks in ICI. The first layer is formulated as an MILP model. Minimizing PFD in transmission lines is the objective function of this model. Multiple constraints are considered in this work, i.e., coherency, connectivity, and oper- ational constraints. Further generation adjustments and/or load shedding are required post-islanding. Finally, the IoT cloud sends signals to the circuit breakers to establish the optimal islanding scheme. Furthermore, it sends commands to the control system to execute the required load or generation reduction in islands. The significant contributions of this study are:
- Integration of IoT technology for online coherency assessment technique;
- Developing an adaptive similarity index for coherency assessment;
- Proposing an MILP model for the “where to island” stage in ICI;
- Optimal generation rescheduling and/or load shedding to attain stable and secure

operation for each subsystem after islanding. *1.5. Paper Structure* Finally, this paper is constructed as follows: Section 2 discusses the general IoT architecture, which forms the backbone of data collection and transmission within the system. Then, it highlights the integration of the IoT in the proposed controlled islanding strategy. Section 3 discusses the coherency analysis technique that is vital to ensure the integrity and stability of the subsystems after separation. It also covers the clustering technique presented in this study. This paper further highlights in Section 4 the utilization of an MILP model to accurately determine island boundaries, considering factors such as coherency and exclusivity constraints. It emphasizes the significance of adhering to connectivity constraints to maintain operational functionality within the isolated microgrid.

*Smart Cities* **2024**, *7* 3876 Additionally, the section outlines post-islanding procedures essential for restoring normal system operation once islands are identified. The system testing results and conclusion are presented in Sections 6 and 7, respectively. **2. IoT Integration for ICI** The IoT system comprises three distinct layers: physical, communication, and cloud layers. The physical layer encompasses sensors, actuators, and other intelligent devices within the power network. The interconnection between these devices and the subsequent layer is facilitated through gateways. The communication layer is responsible for bidirec- tional data transfer between the physical and cloud layers. The final layer is dedicated to data monitoring, analysis, and storage, thereby facilitating the generation of informed and judicious decisions [46,47]. *2.1. IoT Value Chain in ICI* In this work, the IEEE 39-bus system model represents the physical layer of the IoT system. The connection between the power system model and the cloud is established through a Python script seamlessly integrated with the DIgSILENT PowerFactory platform. The process is initiated with the generators’ data gathered from PMUs transferred to the IoT cloud. Then, the actual system data are processed to identify the optimal islanding scheme. Finally, control signals are sent to the power system model to perform intentional islanding with the required procedures for each subsystem. The proposed IoT-based controlled islanding scheme is illustrated in Figure 1. **Figure 1.** The proposed IoT-based controlled islanding strategy.

*Smart Cities* **2024**, *7* 3877 *2.2. Cybersecurity of the System* The electrical power system is critical infrastructure, and integrating IoT technology into this sector raises concerns about potential cyberattacks. Therefore, it is crucial to include a dedicated cybersecurity layer in the IoT value chain of smart grids [48]. This cybersecurity layer focuses on achieving five main goals: authentication, authorization, availability, confidentiality, and integrity [48,49].
- Authentication ensures that every user and device accessing the system is verified,

preventing unauthorized access;
- Authorization further limits data and action access based on specific roles, ensuring

only authorized entities can execute certain controls;
- Availability guarantees that monitoring and control functionalities of the smart grid

remain accessible under all conditions, including during potential cyber incidents, which is essential for maintaining uninterrupted power system operations;
- Confidentiality protects sensitive data such as grid performance metrics through

encryption, ensuring that private information remains secure from unauthorized access. Integrity mechanisms counteract any data tampering attempts, preserving the accuracy and reliability of system data. To further enhance data security, incorporating a blockchain layer into the IoT system provides additional traceability and resilience against data forgery, supporting audit trails and making it difficult for attackers to manipulate information [50]. Furthermore, each layer within the IoT value chain requires unique cybersecurity strategies due to the varied technologies involved. At the device level, cybersecurity can be strengthened by enhancing personnel expertise and implementing secure manufacturing practices that reduce vulner- ability to hardware-based attacks [51]. In the communication layer, intrusion detection systems, such as AI-based models, can be deployed to monitor and identify suspicious activity with high accuracy [52]. These systems are also valuable in the cloud layer, where they can detect anomalies and secure data processing and storage [53]. **3. Coherency Analysis** When an interconnected power system undergoes severe events, synchronous gen- erators have a strong tendency to form coherent groups that oscillate against each other. The formation of these groups is event-based, so the power system cannot adopt a spe- cific pattern for generators coherency that suits all incidents. Thus, having an online measurement-based strategy to recognize these groups is paramount for a successful intentional islanding scheme. *3.1. An Adaptive Similarity Index* Similar rotor acceleration reflects strong coherency between generator pairs. Thus, in this work, a coherency index based on the dynamic coupling criterion is applied [54]. This index is derived from the swing equation of a synchronous machine, incorporating all factors that influence rotor acceleration. The similarity index (SI) between generators m and n is calculated using the following equation: ( 1 + 1 ) *SImn* = *Em*.*En*.*Bmn*.cos(*δm −δn*) (1) *Hm Hn* where *H* refers to generator inertia constant, *E* and *δ* are the internal voltage and rotor angle of the generator, respectively, and *B*mn stands for the susceptance between generators m and n in the admittance matrix. PMUs are mainly designed for electrical measurements such as voltage, current, and frequency. Therefore, mechanical measurements such as rotor angle could be estimated based on the equivalent circuit of each machine. Equation (2)

*Smart Cities* **2024**, *7* 3878 calculates the generator rotor angle with respect to the reference bus in the system as follows [55]: ( *XqIa*cos(*θ*) ) *δ* = *φ* + tan*−*1 (2) *Va* + *XqIa*sin(*θ*) where *φ*, *Xq* , *Ia* , and *θ* are the terminal voltage angle with respect to the slack bus, quadrature–axis reactance, terminal current, and phase angle between terminal voltage and current, respectively. *3.2. Clustering Algorithm* In this stage, a clustering technique is required to recognize coherent generating groups based on SI values. Coherent groups and consequently the number of islands vary with the disturbance. Thus, applying algorithms such as k-means, k-medoids, and fuzzy c-means is inappropriate since the number of islands needs to be determined beforehand. This study proposed a straightforward algorithm that can cluster generators without defining either the number of islands or a threshold value beforehand. Unlike algorithms such as DBSCAN or the support vector machine that deal with Euclidean distance datasets, the algorithm in this study directly works on the SI pairwise values without transferring the data to any specific space. The flowchart in Figure 2 describes how the algorithm can identify coherent generators based on the SI matrix. **Figure 2.** Flow chart of the clustering algorithm.

*Smart Cities* **2024**, *7* 3879 **4. Proposed MILP Model for ICI** In ICI, the power system network is decomposed into N self-sustained areas, achieved through isolating a specific set of lines referred to as the cut set. The ICI procedure starts with the “when to Island?” subproblem that recognizes the necessity of ICI implementation. This paper primarily addresses the last two stages of the ICI process. Therefore, the first task is assumed to be solved using the efficient technique proposed in [56]. *4.1. Graph Theory* In this work, the power system is represented as a non-directional, weighted graph G(E, V, W). This graph comprises E edges (transmission lines) and V vertices (buses), while W denotes the weight of each edge. The weights assigned to transmission lines are determined based on the power flow through each line (*Pl*). It is essential to highlight that power losses in transmission lines are considered by calculating the average active power flow measured at both ends of the line as follows: *Wmn* = *Pl* = (*|P mn|−|P nm|*)/2 (3) *4.2. MILP Model to Determine Islands Boundaries* The computational time in ICI is extremely critical, as late decisions may fail to prevent system collapse following severe conditions. This stage focuses on reducing unallocated nodes, aiming to enhance the MILP model’s computational efficiency and, consequently, the overall performance of the ICI strategy. In each island, it is imperative that all synchronous generators are interconnected. Consequently, the initial phase aims at determining a pathway among these units within each subsystem. In this study, Dijkstra’s algorithm is employed to identify the transmission lines constituting the shortest path among coherent groups of generators. The buses along these paths are designated primary buses within each island. Therefore, they are excluded from the array of optimization variables. Following the identification of primary buses, some other buses, referred to as trapped buses, exhibit connections exclusively with buses belonging to the same island. Thus, trapped buses are assigned to the same island as their adjacent nodes to avoid isolated nodes after system separation. Trapped buses are also eliminated from the optimization problem. Consequently, the MILP algorithm considers only the remaining unallocated buses (U), which significantly enhances the model’s execution time. 4.2.1. Coherency and Exclusivity Constraints In this study, the MILP model is subjected to different constraints: coherency constraint, exclusivity constraint, and connectivity constraints. Regarding the coherency constraint, the model continues the analysis by building upon the islands identified in prior stages, comprising coherent, primary, and trapped buses. Then, the exclusivity constraint ensures that each node is uniquely allocated to a single island. This is attained by a U-by-N optimization variable, denoted as Exist, that equals 1 if this node belongs to this island and zero otherwise. Implementing the constraint outlined in Equation (4), wherein the summation of each row equals one, prevents the existence of isolated nodes or nodes belonging to multiple islands. *N* ∑ *Existi*,*j* = 1 *∀i ∈U* (4) *j*=1 4.2.2. Connectivity Constraint Forming connected subgraphs in ICI is crucial for the stable and reliable operation of islands. A connected graph means that every pair of nodes has at least one path connecting them. In other words, there are no isolated nodes after segmentation. Connectivity con- straints could guarantee that all nodes in each subsystem are connected. The connectivity of each graph is ensured by checking that each node has at least one of its neighboring

*Smart Cities* **2024**, *7* 3880 nodes belonging to the same island. A V *×* N optimization expression *C* is introduced to indicate the inclusion of a node within this subgraph. The determination of *C* can be achieved through the utilization of Equations (5)–(7). Equation (8) represents the connectiv- ity constraint satisfied by the MILP model. *Cm*,*i* = 1 *m ∈island i* (5) *Cm*,*i* = 0 *m* /*∈island i* (6) *Cm*,*i* = *Existm*,*i m ∈U* (7) *r*

### ∑

*i*=1 *C*(*i*, *j*) *>*= *C*(*x*, *j*), *∀*x *∈*V, j *∈*N, r *∈*neighbor nodes of x (8) The proposed model aims to minimize PFD in lines during separation. To detect the cut-set that satisfies this objective, an optimization variable α is employed. For a line with m and n terminals, the binary variable α equals one if m and n belong to different islands and zero otherwise. This variable could be obtained by Equation (9); however, it incorporates a quadratic term that is incompatible with the MILP solver. Therefore, this equation is linearized using an auxiliary optimization variable *Z*, as illustrated in Equations (10)–(12). Subsequently, this variable is utilized for the computation of *α*. The objective function is then represented by (14): *N*

### αe* = 1 *−*∑

*i*=1 *C*(*K*1, *i*) *× C*(*K*2, *i*)*e ∈E*, *K*1, *K*2 *are indises o f edge e* (9) *Ze*,*i >*= *C*(*K*1, *i*) + *C*(*K*2, *i*) *−*1 (10) *Ze*,*i <*= *C*(*K*1, *i*) (11) *Ze*,*i <*= *C*(*K*2, *i*) (12)

### αe* = ∑

*N i*=1 *Ze*,*i* (13) *Objective f unction* : min*αi × Pl*,*i∀i ∈edges* (14) **5. Post-Islanding Procedures** After disconnecting the optimal transmission lines to create the islands, these islands may suffer from load-generation mismatch. Therefore, voltage and frequency instabil- ity occur with a significant likelihood of system collapse. If the subsystem has surplus generating power, then it becomes necessary to curtail the output power of generators to achieve power balance. In this case, optimal economic dispatch is deployed to reschedule the produced power. The objective of this method is to minimize the overall generation costs of the system. The cost function in a quadratic form is shown in (15). The values of *σ*, *β*, and *γ* for the generating units within the IEEE 39-bus system are available in [57].

### Ctot* = ∑

### i*=1 *Ci* = ∑

### Gk

*Gk i*=1 *σi* + *βiPi* + *γiP*2 *i* = 1, .., *Gk* (15) *i Objective f unction* : min*Ctot* (16) Load flow analysis is an essential part of the economic dispatch process as it ensures that the derived solution adheres to the operational constraints of the system. Several research studies have applied DC load flow using approximate equations. This formulation enhances the computational time efficiency compared with the AC load flow. However, AC power flow algorithms give more precise results under all circumstances. Therefore, in this work, an optimal load flow analysis is conducted using the Newton–Raphson technique. The IoT integration provides the advantage of capturing all bus data prior to islanding, which are provided to the load flow algorithm as an initial condition. Consequently, the

*Smart Cities* **2024**, *7* 3881 number of iterations needed to attain the optimal solution is reduced. The load flow technique is governed by the following constraints: *Pi* = *Pg i −Pd i i ∈Nk* (17) *Qi* = *Qg i −Qd i* + *Qsh i ∈Nk* (18) *i Vmin ≤Vi ≤Vmax i ∈Nk* (19) *Qn*,*min ≤Qn ≤Qn*,*max n ∈Gk* (20) *Pn*,*min ≤Pn ≤Pn*,*max n ∈Gk* (21) *−PL*,*max ≤PL ≤PL*,*max L ∈Ek* (22) *−QL*,*max ≤QL ≤QL*,*max L ∈Ek* (23) where *Nk*, *Ek*, and *Gk* refer to the number of nodes, edges, and generating units in island K, respectively. *Pj* and *Qj* represent the active and reactive power flow in lines, and it is imperative to ensure that these values remain below the maximum capacity of each line. Under certain conditions, the power demand exceeds the generated power on the isolated island. In this context, two scenarios are possible: either the maximum capacity of the generators is sufficient to meet the connected loads, or it falls short of reaching this level. In the former scenario, the generating power in the system is increased, so economic dispatch is executed to determine the optimal output power from each generating unit. Conversely, the latter requires load shedding to restore the balance between generation and demand in each subsystem. The first step in load shedding is determining the power deficit within each island. Immediately after separation the power deficit can be calculated by Equations (24) and (25). Priority for load shedding is given to critical buses that have the lowest voltage magnitudes. ∆*P* = 2 *Heq × d f* (24) *fn dt* ∑*Nk j*=1 *Hj × Sj Heq* = (25) ∑*Nk j*=1 *Sj* where *Hj* and *Sj* are the inertia constant and apparent power of generator j, respectively. *fn* is the system nominal frequency. **6. Results and Discussion** The viability of the proposed ICI scheme was evaluated through simulating different scenarios on the IEEE 39-bus system. This system comprises 39 buses, 10 synchronous generating units, and 46 transmission lines. In the first case study, the system experienced an outage of line 3-4 at t = 3 s, followed by another outage of line 16-17 after a duration of 7 s. Under these disturbances, system generators experience growing oscillations until cascading out of step occurs; it started with the loss of synchronization of generator 1 at t = 28.02 s, followed by out-of-step of the other units, as shown in Figure 3. Moreover, the frequency and rotor speed of generators experience growing oscillations, as illustrated in Figures 4 and 5, respectively. It is noteworthy that the impact of these disturbances is more severe on units 2 and 3, as they are located relatively close to both outages. Under this scenario, the terminal voltages of generators witnessed variations that became more severe with the out-of-synchronization of units, as depicted in Figure 6.

*Smart Cities* **2024**, *7* 3882 **Figure 3.** Rotor angles of generators under disturbances in the first case study. **Figure 4.** Electrical frequency of generators in the first case study. **Figure 5.** Rotor speed of generators in the first case study.

*Smart Cities* **2024**, *7* 3883 **Figure 6.** Voltage magnitude of the PV buses in the first case study. The first stage in controlled islanding is coherency assessment. Similarity indices are calculated between every generator pair based on the data gathered from PMUs, which reveals that the system has two oscillating groups, as illustrated in Table 1. Generators 1, 8, 9, and 10 form the first group, while generators 2 to 7 belong to the second one. It can be noticed that, due to the location of generator 1, it exhibits coherency with units in both generating groups. However, the highest similarity index was 0.3758, corresponding to generator 10; that is why the clustering algorithm assigned generator 1 to the same group as unit 10. **Table 1.** Values of SI of generator pairs in the first case study.

|  | G2 G3 G4 G5 G6 G7 G1 G8 G9 G10 |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| G2 G3 G4 G5 G6 G7 G1 G8 G9 G10 | - | 0.8167 | 0.1446 | 0.0684 | 0.1374 | 0.0878 | 0.2285 0 0 0 0.1500 0 0 0 0.0307 0 0 0 0.0151 0 0 0 0.0267 0 0 0 0.0193 0 0 0 |  |  |  |
|  | 0.8167 | - | 0.1826 | 0.0868 | 0.1721 | 0.1113 |  |  |  |  |
|  | 0.1446 | 0.1826 | - | 1.3069 | 0.4546 | 0.2893 |  |  |  |  |
|  | 0.0684 | 0.0868 | 1.3069 | - | 0.2159 | 0.1365 |  |  |  |  |
|  | 0.1374 | 0.1721 | 0.4546 | 0.2159 | - | 1.0690 |  |  |  |  |
|  | 0.0878 | 0.1113 | 0.2893 | 0.1365 | 1.0690 | - |  |  |  |  |
|  | 0.2285 0.1500 0.0307 0.0151 0.0267 0.0193 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 |  |  |  |  |  | - | 0.1760 | 0.0556 | 0.3758 |
|  |  |  |  |  |  |  | 0.1760 | - | 0.4654 | 1.1935 |
|  |  |  |  |  |  |  | 0.0556 | 0.4654 | - | 0.4476 |
|  |  |  |  |  |  |  | 0.3758 | 1.1935 | 0.4476 | - |

Each coherent group is highlighted with a specific color to enhance clarity. After allocating generator buses, instead of passing all the remaining nodes to the MILP algorithm, subsequent steps focusing on reducing solution dimensionality were implemented. This stage could significantly enhance the computational efficiency of the MILP algorithm. Thus, primary and trapped buses were determined and distributed between the identified islands. Then, unallocated nodes and initially established islands were provided to the MILP algorithm. Considering the specified constraints and the formulated islanding objective, the optimal cut-set consisted of two lines {3-4, 8-9, and 16-17}. Lines 3-4 and 16-17 were already isolated; therefore only line 8-9 was disconnected. This resulted in two isolated subsystems with 20.056 MW power interruption, as depicted in Figure 7.

| 2024, 7 |  |  |  |  |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |
|  |  |  |  |  |
|  |  |  |  | 3884 |

**Figure 7.** Test system after applying ICI in the first scenario. In this case, the total electrical demand on both islands was lower than the available capacity of its generating units. Consequently, generation adjustment was required to ensure the stable operation of islands. Optimal economic dispatch was implemented to determine the optimal output power from each unit to restore the generation–load balance at minimum generation costs. Table 2 describes the optimal power produced by all units for the identified islands in the first case. Intentional islanding with the essential generation adjustments were was implemented at t = 12.3 s. Figure 8 illustrates that the rotor angles of units in island 2 witnessed slight oscillations until a steady state was reached. Meanwhile, generators in island 1 experienced higher oscillations. However, all generators exhibited a tendency for stabilization. As shown in Figure 9, after the system separation, units 2 to 7 witnessed a rise in rotor speed until 1.009 pu, while units 8 to 10 experienced decreased oscillations around the value of 1 pu. The voltage magnitudes of all generator buses witnessed some variation after separation until they attained a steady-state condition, as illustrated in Figure 10. All buses in the system exhibited voltage magnitude falling within an acceptable voltage range, as the maximum and minimum values were 1.065 and 0.976 pu, respectively. Due to the correlation between frequency and rotor speed of units, the frequency followed almost the same trend as that of rotor speed of generators, as can be seen in Figure 11. The results demonstrated that, under this scenario, the proposed ICI strategy managed to avoid system collabs by dividing the system into two stable regions. In the second case study, at t = 1 s, the system underwent three-phase short circuits on lines 2-3 and 2-25. Faults occurred at 10% of the line length proximate to bus 2. Then, these faults were cleared by opening both lines at t = 1.1 s. The variations in the rotor angles of generators in response to these disturbances are illustrated in Figure 12. It can be noticed that synchronous generators formed groups swinging against each other until generators 1 and 10 lost synchronization at t = 18.3 s and t = 18.4 s, respectively, followed by out-of-synchronism of the other units. The variations in the generators’ rotor speed, frequency, and terminal voltages are shown in Figures 13, 14, and 15, respectively.

*Smart Cities* **2024**, *7* 3885 **Table 2.** Optimal generated power after controlled islanding. **Island Generator Active Power in Base Case (MW) Generators’ Active Power Post-Islanding (MW)** 1 1000 1312.4 8 540 560.7 1 9 830 661.2 10 250 224.1 2 520.8 492.5 3 650 616.9 4 632 598.8 2 5 508 493.3 6 650 615.7 7 560 531.2 **Figure 8.** Generators’ rotor angle following ICI in the first case study. **Figure 9.** Generators’ rotor speed following ICI in the first case study.

*Smart Cities* **2024**, *7* 3886 **Figure 10.** Voltage magnitude of the PV buses following ICI in the first case study. **Figure 11.** Frequency of generating units after ICI in the first case study. **Figure 12.** Rotor angles of generators under the second scenario.

*Smart Cities* **2024**, *7* 3887 **Figure 13.** Rotor speed of generators under the second case study. **Figure 14.** Frequency of generators under the second case study. **Figure 15.** Terminal voltages of generators under the second case study.

|  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- |
|  | these conditions, similarity indices were calculated for every generator pair. The clustering |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |

*Smart Cities* **2024**, *7* 3888 algorithm of similarity indices divided the units into three coherent groups. Specifically, generators 1 and 10 formed the first group, the second group comprised generators 8 and 9, while the units from 2 to 7 constituted the last group. Table 3 illustrates similarity indices and consequent coherent groups under the second scenario. **Table 3.** Values of SI of generator pairs in the second case study. **G2 G3 G4 G5 G6 G7 G8 G9 G1 G10**

| - | 0.7754 | 0.1588 | 0.0767 | 0.1638 | 0.0980 |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0.7754 | - | 0.1944 | 0.0934 | 0.1947 | 0.1262 |  |  |  |  |
| 0.1588 | 0.1944 | - | 1.5463 | 0.5762 | 0.3523 |  |  |  |  |
| 0.0767 | 0.0934 | 1.5463 | - | 0.2789 | 0.1680 |  |  |  |  |
| 0.1638 | 0.1947 | 0.5762 | 0.2789 | - | 1.3638 |  |  |  |  |
| 0.0980 | 0.1262 | 0.3523 | 0.1680 | 1.3638 | - |  |  |  |  |
| 0 0 0 0 0 0 - 0.3831 0 0 0 0 0 0 0.3831 - −0.2123 −0.2284 −0.0369 −0.0167 −0.0328 −0.0302 −0.1688 −0.0334 0 0 0 0 0 0 −0.1689 −0.0265 |  |  |  |  |  | - | 0.3831 |  |  |
|  |  |  |  |  |  | 0.3831 | - |  |  |
|  |  |  |  |  |  |  |  | - | 0.3347 |
|  |  |  |  |  |  |  |  | 0.3347 | - |

Each coherent group is highlighted with a specific color to enhance clarity. This phase involves steps required to minimize solution dimensionality to alleviate the complexity of the optimization process. Then, the “where to island?” task is addressed by the MILP, which determines the cut-set that has minimum power flow. In this scenario, the optimal cut-set comprises lines {8-9, 17-27, 2-3, and 2-25}. Isolating these lines to create three independent islands resulted in the minimum power interruption with a value of 39.0726 MW, as the last two lines were already disconnected. Figure 16 depicts the three resultant islands following the implementation of ICI in the test system. **Figure 16.** Intentional controlled islanding in the second scenario.

*Smart Cities* **2024**, *7* 3889 The last stage aims to ensure secure and stable operation of islands after splitting. The generation capacity in islands 1 and 3 was higher than the total demand. Thus, these islands only required generation reduction to attain load-generation balance. In contrast, island 2 had an excess loading level that could be compensated by the generators in this region. However, load shedding was required to avoid having overloaded generators and consequently kept adequate reserve in this island. Therefore, the load-generation balance would be achieved by a generation increase in addition to load shedding at buses with the least voltages in the subsystem. Optimal economic dispatch was applied to determine the generated power from every unit to achieve minimum cost and line losses in each region, as illustrated in Table 4. **Table 4.** Required remedial actions in each island after ICI. **Island Generator Active Power in Base Case (MW) Generators’ Active Power Post-Islanding** 1 1000 880.2 1 10 250 245.3 2 520.8 543.8 3 650 676.7 4 632 658.0 2 5 508 511.4 6 650 676.4 7 560 582.6 8 540 646.0 3 9 830 699.1 The variation in rotor angles was rapid post-disturbances, as shown in Figure 17. Subsequently, ICI was applied at t = 2.5 s, and the rotor angles of all units exhibited a stabilization trend. As depicted in Figure 18, after applying ICI, units 8 and 9 experienced an increase in rotor speed while units from 2 to 7 underwent a decrease before stabilizing at 1.02 pu and 0.993 pu, respectively. Following ICI generators in island 1 witnessed slight oscillations for 2.492 s to reach a stable state at 1.001 pu. This was attributed to the high inertia of generator 1 in comparison with all other units in the system. The variations in frequency and voltage magnitude of generators are illustrated in Figures 19 and 20, respectively. It can be seen that, under these disturbances, the three subsystems managed to reach a stable state after separation. **Figure 17.** Rotor angle post-controlled islanding in the second scenario.

*Smart Cities* **2024**, *7* 3890 **Figure 18.** Rotor speed post-controlled islanding in the second scenario. **Figure 19.** Frequency of generators post-controlled islanding in the second scenario. **Figure 20.** Voltage magnitude of PV buses post-controlled islanding in the second scenario.

*Smart Cities* **2024**, *7* 3891 The ICI is specifically designed to prevent the loss of synchronism among non-coherent generators following a major disruptive event. When the system experiences unstable inter- area oscillations that are not effectively damped, it can lead to a loss of synchronism among non-coherent areas, potentially resulting in unintentional islanding and subsequent partial or total blackouts. To mitigate this risk, ICI isolates non-coherent groups of generators into stable subsystems. If all generators remain coherent following a disturbance, then the risk of out-of-step and unintentional islanding is eliminated, making controlled islanding unnecessary. In scenarios where there is only one coherent group, dividing the network into islands would not improve the system’s stability, as each island would likely exhibit similar dynamic behavior in response to disturbances. In such a situation, alternative emergency measures, such as load shedding, generation rescheduling, or other device-based controls, would be more effective for dampening oscillations and maintaining system stability. A comparison of the proposed strategy with multiple existing approaches from the literature is illustrated in Table 5. Key features, such as adaptability to system variation, an adaptive number of islands, and addressing post-islanding procedures, are examined to underscore the strengths and weaknesses of each approach. This comparison emphasizes the improvements offered by the proposed strategy compared with existing methods. **Table 5.** A comparison of the proposed strategy with other existing techniques. **Guarantee a Adaptability Adaptive No. Main Coherency Global Post-Islanding Ref to System of Method Objective Analysis Optimal Procedures Variations Islands Function Solution** ✔ **[34]**
- Offline
- Graph-based

Min PFD - Min power ✔ **[40]**
- Not specified

Optimization
- ✔

imbalance Min power ✔ **[58]**
- Measurement-

Deep learning
- -

based imbalance 2nd-order ✔ **[42]**
- Model-based

coneprogram- Min PFD - ming MILP + transient Min power ✔ **[45]**
- Model-based

- - energy imbalance function Min Load ✔ **[14]**
- Model-based
- MILP

- shedding **The** Measurement- ✔ ✔ ✔ ✔ **proposed** MILP Min PFD based **strategy 7. Conclusions** In this study, a novel IoT-based controlled islanding scheme is proposed to counter- act blackout under extreme conditions. Integrating IoT into the power system provides a complete vision of the entire system in real-time. This feature enhances the electrical power system’s intelligence, autonomy, and dependability. The proposed ICI strategy for emergency situations comprises three primary stages: coherency analysis, determination of island boundaries, and post-islanding measures. A similarity index was adopted to deter- mine coherent generator groups, followed by a proposed clustering algorithm. The second phase in ICI began with reducing solution dimensionality, achieved by identifying primary and trapped buses within the system. Subsequently, MILP was applied to define island boundaries. Instead of pursuing minimal power imbalance in islands, this study adopted minimizing power disruption as an objective function. As disconnecting heavily loaded lines can jeopardize the transient stability of the resulting subsystems. Load/generation adjustment is the last stage in the controlled islanding process. The magnitude of optimal load shedding and generation rescheduling was calculated to maintain a load–generation balance. Therefore, voltage and frequency stability were maintained in each island. All

*Smart Cities* **2024**, *7* 3892 these analyses were performed in the IoT cloud after receiving measurements from PMUs installed in the power system. Then, the cloud sent the obtained control signals to specific elements in the power system to implement the ICI scheme. The IEEE 39-bus system was adopted as a test system to evaluate system performance with the proposed strategy. The results demonstrated the effectiveness of the proposed ICI scheme. The proposed strategy managed to avoid system collapse by splitting the network into stable and reliable islands under severe disturbances. **Author Contributions:** Conceptualization, A.A.O. and D.-E.A.M.; methodology, A.A.O., D.-E.A.M. and T.F.M.; software, A.A.O. and T.F.M.; validation, A.A.O., D.-E.A.M., A.B.Z., J.S. and T.F.M.; formal analysis, A.A.O.; data curation, A.A.O., D.-E.A.M. and T.F.M.; writing—original draft preparation, A.A.O.; writing—review and editing, D.-E.A.M., A.B.Z., J.S. and T.F.M.; visualization, A.A.O.; super- vision, D.-E.A.M., A.B.Z., J.S. and T.F.M. All authors have read and agreed to the published version of the manuscript. **Funding:** This research received no external funding. **Data Availability Statement:** The original contributions presented in the study are included in the article; further inquiries can be directed to the corresponding author. **Conflicts of Interest:** The authors declare no conflicts of interest. **References**
1. Kundur, P.S.; Malik, O.P. Introduction to The Power System Stability Problem. In *Power System Stability and Control*, 2nd ed.;

McGraw-Hill Education: New York, NY, USA, 2022.
1. Peyghami, S.; Palensky, P.; Blaabjerg, F. An Overview on the Reliability of Modern Power Electronic Based Power Systems. *IEEE*

*Open J. Power Electron.* **2020**, *1*, 34–50. [CrossRef]
1. Rafique, Z.; Khalid, H.M.; Muyeen, S.M.; Kamwa, I. Bibliographic review on power system oscillations damping: An era of

conventional grids and renewable energy integration. *Int. J. Electr. Power Energy Syst.* **2022**, *136*, 107556. [CrossRef]
1. Pal, B.; Chaudhuri, B. Power system oscillations. In *Robust Control Power Systems*; Springer: Boston, MA, USA, 2005; pp. 5–12.

Available online: https://link.springer.com/chapter/10.1007/0-387-25950-3_2#chapter-info (accessed on 1 June 2024).
1. Cai, D.; Regulski, P.; Osborne, M.; Terzija, V. Wide Area Inter-Area Oscillation Monitoring Using Fast Nonlinear Estimation

Algorithm. *IEEE Trans. Smart Grid* **2013**, *4*, 1721–1731.
1. Alhelou, H.H.; Hamedani-Golshan, M.E.; Njenda, T.C.; Siano, P. A Survey on Power System Blackout and Cascading Events:

Research Motivations and Challenges. *Energies* **2019**, *12*, 682. [CrossRef]
1. Allen, E.; Andersson, G.; Berizzi, A.; Boroczky, S. Blackout Experiences and Lessons, Best Practices for System Dynamic

Performance, and the Role of New Technologies. 2007. Available online: https://www.researchgate.net/publication/272482692_ Blackout_Experiences_and_Lessons_Best_Practices_for_System_Dynamic_Performance_and_the_Role_of_New_Technologies (accessed on 1 June 2024).
1. Kamwa, I.; Liu, N. How to keep the lights on: Lessons from major blackouts over the last 35 years. *IEEE Power Energy Mag.* **2023**,

*21*, 4–11.
1. Saribulut, L.; Ok, G.; Ameen, A. A Case Study on National Electricity Blackout of Turkey. *Energies* **2023**, *16*, 4419. [CrossRef]
2. Ahangar, A.R.H.; Gharehpetian, G.B.; Baghaee, H.R. A review on intentional controlled islanding in smart power systems and

generalized framework for ICI in microgrids. *Int. J. Electr. Power Energy Syst.* **2020**, *118*, 105709. [CrossRef]
1. Salimian, M.R.; Aghamohammadi, M.R. Intelligent Out of Step Predictor for Inter Area Oscillations Using Speed-Acceleration

Criterion as a Time Matching for Controlled Islanding. *IEEE Trans. Smart Grid* **2018**, *9*, 2488–2497. [CrossRef]
1. Chamorro, H.R.; Gomez-Diaz, E.O.; Paternina, M.R.A.; Andrade, M.A.; Barocio, E.; Rueda, J.L.; Gonzalez-Longatt, F.; Sood, V.K.

Power system coherency recognition and islanding: Practical limits and future perspectives. *IET Energy Syst. Integr.* **2023**, *5*, 1–14. [CrossRef]
1. Mehrzad, A.; Darmiani, M.; Mousavi, Y.; Shafie-Khah, M.; Aghamohammadi, M. An Efficient Rapid Method for Generators

Coherency Identification in Large Power Systems. *IEEE Open Access J. Power Energy* **2022**, *9*, 151–160. [CrossRef]
1. Badr, A.A.; Safari, A.; Ravadanegh, S.N. Power system islanding considering effect of microgrid by MILP formulation. *Electr.*

*Power Syst. Res.* **2023**, *216*, 109093. [CrossRef]
1. Koochi, M.H.R.; Esmaeili, S.; Ledwich, G. Taxonomy of coherency detection and coherency-based methods for generators

grouping and power system partitioning. *IET Gener. Transm. Distrib.* **2019**, *13*, 2597–2610. [CrossRef]
1. Senroy, N. Generator Coherency Using the Hilbert–Huang Transform. *IEEE Trans. Power Syst.* **2008**, *23*, 1701–1708. [CrossRef]
2. Davodi, M.; Modares, H.; Reihani, E.; Davodi, M.; Sarikhani, A. Coherency approach by hybrid PSO, K-Means clustering method

in power system. In Proceedings of the 2008 IEEE 2nd International Power and Energy Conference, Johor Bahru, Malaysia, 1–3 December 2008; pp. 1203–1207.

*Smart Cities* **2024**, *7* 3893
1. Zhang, B.-Z.; Fan, C.-X. Study on identification coherent generators based on MPSO-FCM algorithm. In Proceedings of the 2015

4th International Conference on Computer Science and Network Technology (ICCSNT), Harbin, China, 19–20 December 2015; Volume 1, pp. 574–578.
1. Soni, B.P.; Saxena, A.; Gupta, V. Online identification of coherent generators in power system by using SVM. In Proceedings of the

2017 4th International Conference on Power, Control & Embedded Systems (ICPCES), Allahabad, India, 9–11 March 2017; pp. 1–5.
1. Babaei, M.; Muyeen, S.M.; Islam, S. Identification of Coherent Generators by Support Vector Clustering with an Embedding

Strategy. *IEEE Access* **2019**, *7*, 105420–105431. [CrossRef]
1. Koochi, M.H.R.; Esmaeili, S.; Fadaeinedjad, R. New phasor-based approach for online and fast prediction of generators grouping

using decision tree. *IET Gener. Transm. Distrib.* **2017**, *11*, 1566–1574. [CrossRef]
1. Mang-Hui, W.; Hong-Chan, C. Novel clustering method for coherency identification using an artificial neural network. *IEEE*

*Trans. Power Syst.* **1994**, *9*, 2056–2062. [CrossRef]
1. Zhang, S.; Yogarathinam, A.; Zhan, J.; Yue, M.; Lin, G. A Step Towards Machine Learning-based Coherent Generator Grouping

for Emergency Control Applications in Modern Power Grid. In Proceedings of the 2020 IEEE Power & Energy Society General Meeting (PESGM), Montreal, QC, Canada, 2–6 August 2020; pp. 1–5.
1. Znidi, F.; Davarikia, H.; Iqbal, K.; Barati, M. Multi-layer spectral clustering approach to intentional islanding in bulk power

systems. *J. Mod. Power Syst. Clean Energy* **2019**, *7*, 1044–1055. [CrossRef]
1. Aghamohammadi, M.R.; Mahdavizadeh, S.F.; Rafiee, Z. Controlled Islanding Based on the Coherency of Generators and

Minimum Electrical Distance. *IEEE Access* **2021**, *9*, 146830–146840. [CrossRef]
1. Sadeghi, M.; Akbari, H.; Mousavi, S.; Daemi, T. Coherency-Based Supervised Learning Approach for Determining Optimal

Post-Disturbance System Separation Strategies. *IEEE Access* **2023**, *11*, 5894–5907. [CrossRef]
1. Mahdavizadeh, S.F.; Aghamohammadi, M.R.; Ranjbar, S. Frequency stability-based controlled islanding scheme based on

clustering algorithm and electrical distance using real-time dynamic criteria from WAMS data. *Sustain. Energy Grids Netw.* **2022**, *30*, 100560. [CrossRef]
1. Rezaee, M.; Moghadam, M.S.; Ranjbar, S. Online estimation of power system separation as controlled islanding scheme in the

presence of inter-area oscillations. *Sustain. Energy Grids Netw.* **2020**, *21*, 100306. [CrossRef]
1. Ranjbar, S. Adaptive criteria of estimating power system separation times based on inter-area signal. *IET Gener. Transm. Distrib.*

**2023**, *17*, 573–588. [CrossRef]
1. Znidi, F.; Davarikia, H.; Iqbal, K. Modularity clustering based detection of coherent groups of generators with generator integrity

indices. In Proceedings of the 2017 IEEE Power & Energy Society General Meeting, Chicago, IL, USA, 16–20 July 2017; pp. 1–5.
1. Xu, S.; Miao, S. Three-stage method for intentional controlled islanding of power systems. *J. Mod. Power Syst. Clean Energy* **2018**,

*6*, 691–700. [CrossRef]
1. Amini, M.; Samet, H.; Seifi, A.R.; Davarpanah, M. Effective Controlled Islanding Method for Power Grids Solving a Sequence of

Optimization Problems. *CSEE J. Power Energy Syst.* **2022**, *8*, 1004–1012.
1. Esmaeilian, A.; Kezunovic, M. Prevention of Power Grid Blackouts Using Intentional Islanding Scheme. *IEEE Trans. Ind. Appl.*

**2017**, *53*, 622–629. [CrossRef]
1. Dabbaghjamanesh, M.; Wang, B.; Kavousi-Fard, A.; Mehraeen, S.; Hatziargyriou, N.D.; Trakas, D.N. A Novel Two-Stage Multi-

Layer Constrained Spectral Clustering Strategy for Intentional Islanding of Power Grids. *IEEE Trans. Power Deliv.* **2020**, *35*, 560–570. [CrossRef]
1. Kai, S.; Da-Zhong, Z.; Qiang, L. Splitting strategies for islanding operation of large-scale power systems using OBDD-based

methods. *IEEE Trans. Power Syst.* **2003**, *18*, 912–923.
1. Qianchuan, Z.; Kai, S.; Da-Zhong, Z.; Jin, M.; Qiang, L. A study of system splitting strategies for island operation of power system:

A two-phase method based on OBDDs. *IEEE Trans. Power Syst.* **2003**, *18*, 1556–1565. [CrossRef]
1. Kai, S.; Da-Zhong, Z.; Qiang, L. A simulation study of OBDD-based proper splitting strategies for power systems under

consideration of transient stability. *IEEE Trans. Power Syst.* **2005**, *20*, 389–399.
1. Tang, F.; Zhou, H.; Wu, Q.; Qin, H.; Jia, J.; Guo, K. A Tabu Search Algorithm for the Power System Islanding Problem. *Energies*

**2015**, *8*, 11315–11341. [CrossRef]
1. Oboudi, M.H.; Hooshmand, R.; Karamad, A. Feasible method for making controlled intentional islanding of microgrids based on

the modified shuffled frog leap algorithm. *Int. J. Electr. Power Energy Syst.* **2016**, *78*, 745–754. [CrossRef]
1. Oboudi, M.H.; Hooshmand, R.; Karamad, A. A feasible method for controlled intentional islanding in microgrids based on PSO

algorithm. *Swarm Evol. Comput.* **2017**, *35*, 14–25. [CrossRef]
1. Kyriacou, A.; Demetriou, P.; Panayiotou, C.; Kyriakides, E. Controlled Islanding Solution for Large-Scale Power Systems. *IEEE*

*Trans. Power Syst.* **2018**, *33*, 1591–1602. [CrossRef]
1. Li, P.; Xu, D.; Su, H.; Sun, Z. A Second-Order Cone Programming Model of Controlled Islanding Strategy Considering Frequency

Stability Constraints. *Sustainability* **2023**, *15*, 5386. [CrossRef]
1. Babaei, M.; Muyeen, S.M.; Islam, S. Transiently stable intentional controlled islanding considering post-islanding voltage and

frequency stability constraints. *Int. J. Electr. Power Energy Syst.* **2021**, *127*, 106650. [CrossRef]
1. Badr, A.A.; Safari, A.; Ravadanegh, S.N. Segmentation of interconnected power systems considering microgrids and the

uncertainty of renewable energy sources. *IET Gener. Transm. Distrib.* **2023**, *17*, 3814–3827. [CrossRef]

*Smart Cities* **2024**, *7* 3894
1. Kamali, S.; Amraee, T.; Khorsand, M. Intentional power system islanding under cascading outages using energy function method.

*IET Gener. Transm. Distrib.* **2020**, *14*, 4553–4562. [CrossRef]
1. Mansour, D.A.; Numair, M.; Zalhaf, A.S.; Ramadan, R.; Darwish, M.M.F.; Huang, Q.; Hussien, M.G.; Abdel-Rahim, O. Applications

of IoT and digital twin in electrical power systems: A comprehensive survey. *IET Gener. Transm. Distrib.* **2023**, *17*, 4457–4479. [CrossRef]
1. Shakerighadi, B.; Anvari-Moghaddam, A.; Vasquez, J.C.; Guerrero, J.M. Internet of Things for Modern Energy Systems: State-of-

the-Art, Challenges, and Open Issues. *Energies* **2018**, *11*, 1252. [CrossRef]
1. Numair, M.; Mansour, D.A.; Hussien, M.G. Infrastructure for the 4th Industrial Revolution Technologies. In *Power Systems Amid*

*the 4th Industrial Revolution*; River Publishers: Aalborg, Denmark, 2024; pp. 7–30.
1. Monteiro, L.F.R.; Rodrigues, Y.R.; de Souza, A.C.Z. Cybersecurity in Cyber–Physical Power Systems. *Energies* **2023**, *16*, 4556.

[CrossRef]
1. Alajlan, R.; Alhumam, N.; Frikha, M. Cybersecurity for Blockchain-Based IoT Systems: A Review. *Appl. Sci.* **2023**, *13*, 7432.

[CrossRef]
1. Lee, I. Internet of Things (IoT) Cybersecurity: Literature Review and IoT Cyber Risk Management. *Future Internet* **2020**, *12*, 157.

[CrossRef]
1. Li, J.; Zhao, Z.; Li, R.; Zhang, H. AI-Based Two-Stage Intrusion Detection for Software Defined IoT Networks. *IEEE Internet Things*

*J.* **2019**, *6*, 2093–2102. [CrossRef]
1. Sohal, A.S.; Sandhu, R.; Sood, S.K.; Chang, V. A cybersecurity framework to identify malicious edge device in fog computing and

cloud-of-things environments. *Comput. Secur.* **2018**, *74*, 340–354. [CrossRef]
1. DIgSilent PowerFactory. Available online: https://www.digsilent.de/en/powerfactory.html (accessed on 3 July 2024).
2. Aghamohammadi, M.R.; Tabandeh, S.M. A new approach for online coherency identification in power systems based on

correlation characteristics of generators rotor oscillations. *Int. J. Electr. Power Energy Syst.* **2016**, *83*, 470–484. [CrossRef]
1. Ranjbar, S. Online estimation of controlled islanding time intervals using dynamic state trajectories through cascading failures

from WAMS data. *Electr. Power Syst. Res.* **2023**, *214*, 108890. [CrossRef]
1. Moreno, R.; Obando-Ceron, J.; Gonzalez, G. An integrated OPF dispatching model with wind power and demand response for

day-ahead markets. *Int. J. Electr. Comput. Eng. (IJECE)* **2019**, *9*, 2794. [CrossRef]
1. Sun, Z.; Spyridis, Y.; Lagkas, T.; Sesis, A.; Efstathopoulos, G.; Sarigiannidis, P. End-to-End Deep Graph Convolutional Neural

Network Approach for Intentional Islanding in Power Systems Considering Load-Generation Balance. *Sensors* **2021**, *21*, 1650. [CrossRef] **Disclaimer/Publisher’s Note:** The statements, opinions and data contained in all publications are solely those of the individual author(s) and contributor(s) and not of MDPI and/or the editor(s). MDPI and/or the editor(s) disclaim responsibility for any injury to people or property resulting from any ideas, methods, instructions or products referred to in the content.
