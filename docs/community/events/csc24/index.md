# CarbyneStackCon '24

<p style="text-align: center; margin: 5em 0 5em 0;">
  <img alt="CarbyneStackCon Logo" src="/images/events/csc/24/csc24-logo.png">
</p>

_CarbyneStackCon_ is the annual gathering of the Carbyne Stack Open Source
Community, designed to foster collaboration, discussion, and knowledge sharing
on the latest advancements in secure multiparty computation (MPC).

---

## Registration

CarbyneStackCon '24 (CSC24) is an open event sponsored by Bosch Research taking
place on **November 27th (talks)** and **28th (workshops)**, 2024 in Renningen,
Germany. Everyone interested in **enterprise-grade open MPC** is welcome to
attend! However, seats for in-person participation are limited and registration
is required for both in-person and virtual attendance in order to facilitate
our planning.

!!! warning "Registration closed"
    Registration is closed since November 21, 2024 (CET).

---

## Program

### Conference Day (Nov 27)

<!-- markdownlint-capture -->
<!-- markdownlint-disable MD013 -->

| Time                    | Speaker                                                                   | Title                                                                                                    |
|-------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| **8:30 am - 9:30 am**   | -                                                                         | Registration & Welcome Coffee                                                                            |
| **9:30 am - 9:45 am**   | Dr. Sven Trieflinger                                                      | Welcome & Opening Remarks                                                                                |
| **9:45 am - 10:15 am**  | Dr. Marcel Keller                                                         | [MP-SPDZ After 6 Years](#talk-1)                                                                   |
| **10:15 am - 10:45 am** | Abhilash Venkatesh                                                        | [TEE-based Secure Computation and its Application to Carbyne Stack](#talk-2)                             |
| **10:45 am - 11:00 am** | -                                                                         | **Coffee Break**                                                                                         |
| **11:00 am - 11:30 am** | Dr. Ajith Suresh                                                          | [Revitalizing Privacy-Preserving Machine Learning: Introducing FANNG-MPC](#talk-3)                       |
| **11:30 am - 12:00 pm** | Kert Tali                                                                 | [Self-Service Deployment in MPC-as-a-Service](#talk-4)                                                   |
| **12:00 pm - 1:00 pm**  | -                                                                         | **Lunch Break & Group Photo**                                                                            |
| **1:00 pm - 1:30 pm**   | Dr. Jonas Böhler                                                          | [Trees & Trade-offs for Secure Predictions](#talk-5)                                                     |
| **1:30 pm - 2:00 pm**   | Dr. Christoph Bösch <br> Prof. Dr. Thomas Hoeren <br> Merlin Rombach                                                 | [MPC as a Tool for Data Anonymization in Data Analytics](#talk-6)                                        |
| **2:00 pm - 2:30 pm**   | Adrián Vaca Humanes <br> Daniele Romanini                                 | [Building a Privacy-First Ecosystem: SMPC in AdTech Analytics](#talk-7)                                  |
| **2:30 pm - 3:00 pm**   | Dr. Ian Zhou <br> Vincent Rieder                                          | [Silentium: Low Communication & Hardware Acceleration for Beaver Triple Generation](#talk-8)             |
| **3:00 pm - 3:15 pm**   | -                                                                         | **Coffee Break**                                                                                         |
| **3:15 pm - 3:45 pm**   | Dr. Sven Trieflinger <br> Sebastian Becker <br> Dr. Benjamin Hettwer      | [Thymus: Adding essential Security Features to the Carbyne Stack platform the Cloud-Native Way](#talk-9) |
| **3:45 pm - 4:15 pm**   | Dr. John Liagouris                                                        | [Integrating the BU Secure Analytics Stack with Carbyne Stack](#talk-10)                                 |
| **4:15 pm - 4:45 pm**   | Dr. Brian LaMacchia <br> Dr. Sven Trieflinger <br> Dr. Christian Hoeppler | [Open Source MPC - Quo Vadis](#talk-11)                                                                  |
| **4:45 pm - 5:00 pm**   | -                                                                         | Closing Remarks                                                                                          |

---

#### Talk Details

---
:octicons-clock-16: <span class="gsoc-legend">9:45 am - 10:15 am</span>  
<span id="talk-1" class="gsoc-topic-section-title">MP-SPDZ After 6 Years</span>

???+ info "Dr. Marcel Keller (Senior Research Scientist, CSIRO Data61)"

    <div style="display: flex; align-items: center; gap: 15px;">
        <img src="images/speakers/marcel-keller.jpg" alt="Dr. Marcel Keller" style="width: 150px; border-radius: 8px;">
        <p>Dr. Marcel Keller is a senior research scientist with CSIRO's Data61, a research unit of Australia's national science agency. 
        After completing his PhD with Ivan Damgård at Aarhus University, he spent a few years at the University of Bristol under 
        the supervision of Nigel Smart. There, he started working on an implementation of multi-party computation that eventually 
        would form the basis of MP-SPDZ, an open-source project used by researchers all over the world.</p>
    </div>

???+ abstract "Abstract"

    MP-SPDZ is the leading open-source research prototype for multi-party computation, a major privacy-enhancing technology. 
    In this talk, I will present more recent developments in MP-SPDZ. On the protocol side, this includes secure shuffling, 
    which enables more efficient data analytics, and the switch to SoftSpokenOT, a more secure and flexible basis for scaling 
    two-party computation. In addition, I will cover MP-SPDZ's approach to scaling computation efficiently, which has been 
    inspired by HyCC (CCS'18).

---
:octicons-clock-16: <span class="gsoc-legend">10:15 am - 10:45 am</span>  
<span id="talk-2" class="gsoc-topic-section-title">TEE-based Secure Computation
and its Application to Carbyne Stack</span>

???+ info "Abhilash Venkatesh (Lead Engineer, CDPG, IISc)"

    <div style="display: flex; align-items: center; gap: 15px;">
        <img src="images/speakers/abhilash-venkatesh.jpg" alt="Abhilash Venkatesh" style="width: 150px; border-radius: 8px;">
        <p>Abhilash is a Lead Engineer at the Centre of Data for Public Good (CDPG), Foundation of Society (FSID), Innovation and 
        Development, IISc. He works primarily on container, cloud-based technologies and Secure Enclave, SMPC-based privacy-enhancing 
        technology (PET) at CDPG's research division. He loves Linux, computer networks, security, privacy, and anything related to 
        systems in general.</p>
    </div>

???+ abstract "Abstract"

    Trusted Execution Environment (TEE) is a hardware-based technology that provides data integrity, confidentiality, and code 
    integrity during data processing. Commonly used Multi-Party Computation (MPC) paradigms provide privacy-preserving computation 
    via two phases: an input-independent offline phase, which uses heavyweight cryptographic tools to generate cryptographic 
    randomness in advance, and a second online phase consisting of lightweight operations that consume this randomness. This talk 
    will cover the integration of TEE with MPC to securely accelerate the MPC offline phase in collaboration with the Carbyne Stack team at Bosch Research. 
    The talk will also present the applications of TEEs to various privacy-enhancing use cases.

---
:octicons-clock-16: <span class="gsoc-legend">10:45 am - 11:00 am</span>  
:material-coffee: <span class="gsoc-legend">Coffee Break</span>
---
:octicons-clock-16: <span class="gsoc-legend">11:00 am - 11:30 am</span>  
<span id="talk-3" class="gsoc-topic-section-title">Revitalizing
Privacy-Preserving Machine Learning: Introducing FANNG-MPC for Actively Secure
MLaaS</span>

???+ info "Dr. Ajith Suresh (Senior MPC Researcher, Technology Innovation Institute, Abu Dhabi)"

    <div style="display: flex; align-items: center; gap: 15px;">
        <img src="images/speakers/ajith-suresh.jpg" alt="Dr. Ajith Suresh" style="width: 150px; border-radius: 8px;">
        <p>Dr. Ajith Suresh is a Senior MPC Researcher at the Technology Innovation Institute (TII) in Abu Dhabi, affiliated with the 
        Cryptography Research Center. Prior to joining TII, he completed 1.5 years of post-doctoral research at the Cryptography 
        and Privacy Engineering (ENCRYPTO) group at the Technical University of Darmstadt, under the supervision of Prof. Thomas 
        Schneider. He holds a PhD from the Indian Institute of Science Bangalore. His research focuses on the design and development 
        of applied Multi-Party Computation (MPC) protocols, with additional interests in privacy-preserving machine learning and federated 
        learning.</p>
    </div>

???+ abstract "Abstract"

    In response to the reproducibility crisis in scientific research, we present FANNG-MPC, a versatile secure multi-party 
    computation (MPC) framework designed for privacy-preserving machine learning as a service. FANNG is a data-oriented fork of 
    the now-deprecated SCALE-MAMBA, featuring new libraries and instructions optimized for private neural networks. Key innovations 
    include decoupling offline and online phases, a software-based dealer model, and integrated database support. It also introduces 
    advanced protocols for garbled circuits, convolution operations, and private comparisons. This talk will explore FANNG’s design 
    challenges, solutions, and unresolved issues, with a focus on engaging CarbyneStackCon’s community to refine and extend FANNG’s 
    capabilities for privacy-preserving ML.

---
:octicons-clock-16: <span class="gsoc-legend">11:30 am - 12:00 pm</span>  
<span id="talk-4" class="gsoc-topic-section-title">Self-Service Deployment of
Computation Tasks in a Multi-Tenant MPC-as-a-Service</span>

???+ info "Kert Tali (Architect, Sharemind MPC Product Development, Cybernetica)"

    <div style="display: flex; align-items: center; gap: 15px;">
        <img src="images/speakers/kert-tali.png" alt="Kert Tali" style="width: 150px; border-radius: 8px;">
        <p>Kert joined the Sharemind MPC product development team at Cybernetica in 2021 as a programmer. Fascinated by the engineering 
        challenges of MPC deployments, he began exploring ways to enhance the practicality of production-grade MPC. In 2022, Kert 
        defended his Master's thesis on scaling parallel algorithms on MPC. Shortly after, he set his sights on Carbyne Stack. Realizing 
        its immense potential for standardizing the way MPC is deployed, he spearheaded the initiative to integrate Sharemind MPC with 
        the Carbyne Stack platform. As the team grew around this shared ambition, he assumed the role of an architect.</p>
    </div>

???+ abstract "Abstract"

    Employing MPC for cross-organizational data processing has well-established merits. Yet, the selection of technological solutions 
    that prioritize viability as much as security is limited. The UNECE Input Privacy Preservation Project assessed that servitisation 
    (i.e., MPC-as-a-Service) is key for the widespread adoption of MPC; however, it's not without critical design criteria to compete 
    with data sharing as the preferred method. Such a system would need to propose comprehensive processes and lightweight workflows— 
    accessible to non-technical users for securely and collaboratively organizing and executing MPC tasks. This talk expands on the 
    ongoing work in *JOCONDE*, a project between Eurostat and Cybernetica to specify an MPCaaS system for use in official statistics. 
    We will explore how general-purpose MPC platforms like Carbyne Stack play a vital role in powering this system.

---
:octicons-clock-16: <span class="gsoc-legend">12:00 pm - 1:00 pm</span>  
:material-food: <span class="gsoc-legend">Lunch Break & Group Photo</span>
---
:octicons-clock-16: <span class="gsoc-legend">1:00 pm - 1:30 pm</span>  
<span id="talk-5" class="gsoc-topic-section-title">Trees & Trade-offs for
Secure Predictions</span>

???+ info "Dr. Jonas Böhler (Lead AI Security & Privacy Researcher, SAP)"

    <div style="display: flex; align-items: center; gap: 15px;">
        <img src="images/speakers/jonas-boehler.jpg" alt="Dr. Jonas Böhler" style="width: 150px; border-radius: 8px;">
        <p>Dr. Jonas Böhler is the lead AI security & privacy researcher for the SAP Foundation Model, SAP's table-native AI solution for 
        prediction tasks on tabular data. Previously, he was a senior researcher at SAP Security Research and also serves as SAP's 
        project lead for the EU project *Glaciation*, focusing on privacy-preserving collaborative learning. Jonas received his PhD 
        from the Karlsruhe Institute of Technology (KIT), where his thesis received awards from the ERCIM Security and Trust 
        Management Working Group and the KIT faculty of computer science. His research interests focus on privacy-enhancing 
        technologies with applications in cross-company collaborations.</p>
    </div>

???+ abstract "Abstract"

    Privacy-preserving edge-to-cloud data operations are central to the EU project *Glaciation*. SAP explores collaborative learning 
    across companies, empowered by Carbyne Stack’s cloud-native secure multi-party computation stack. For structured data, as 
    commonly found in industry settings, tree-based models offer simple, efficient, and often accurate prediction solutions. 
    However, secure multi-party training of tree-based models still incurs significant overhead. Fortunately, there exists a 
    spectrum between cloud-outsourced secure multi-party training and edge-local processing. This talk will delve into *Glaciation*’s 
    progress, discuss secure tree training, and examine various trade-offs considered to optimize the process.

---
:octicons-clock-16: <span class="gsoc-legend">1:30 pm - 2:00 pm</span>  
<span id="talk-6" class="gsoc-topic-section-title">MPC as a Tool for Data
Anonymization in Data Analytics: A Legal and Technical Perspective</span>

???+ info "Dr. Christoph Bösch (Research Engineer, Bosch Research), Prof. Dr. Thomas Hoeren (Head of ITM, University of Münster), and Merlin Rombach (Academic Associate, ITM, University of Münster)"

    Dr. Christoph Bösch is a research engineer at Bosch Research in the field of security, privacy, and cryptography in general, with a particular focus on applied cryptography 
    and the challenges associated with privacy engineering.

    <div style="display: flex; align-items: flex-start; gap: 15px;">
        <img src="images/speakers/thomas-hoeren.webp" alt="Thomas Hoeren" style="width: 150px; border-radius: 8px;">
        <p>Prof. Dr. Thomas Hoeren is Head of the Institute for Information, Telecommunications and 
        Media Law (ITM) at the University of Münster and current lecturer at the University of Vienna, 
        he is a experienced legal scholar and former judge at the Court of Appeal of Düsseldorf 
        (Copyright Senate). Throughout his career, he has held various prestigious positions and has 
        made significant contributions to the fields of information technology law and intellectual 
        property rights.</p>
    </div>
    
    <div style="display: flex; align-items: flex-start; gap: 15px; margin-top: 15px;">
        <img src="images/speakers/merlin-rombach.jpg" alt="Merlin Rombach" style="width: 150px; border-radius: 8px;">
        <p>Merlin Rombach is a Academic Associate at the Institute for Information, Telecommunications 
        and Media Law (ITM) at the University of Münster. His research interests span cybersecurity law,
         data protection law, and AI regulation, with his doctoral research specifically exploring how 
         cryptographic methods can enhance personal data protection and potentially even achieve 
         anonymization. Through his work, he examines the legal framework for privacy-enhancing 
         technologies in the digital age.</p>
    </div>
???+ abstract "Abstract"

    As organizations increasingly rely on data to drive decision-making, handling sensitive information responsibly and in compliance with data protection regulations like the GDPR is essential. 
    Multi-Party Computation (MPC) provides a secure way to process data for analytical purposes while meeting privacy standards. However, companies have been cautious in adopting MPC for 
    real-world applications due to legal uncertainties and unclear benefits. This talk presents the findings from a legal assessment of MPC’s potential as a tool for 
    data anonymization in line with GDPR, with a special focus on HR analytics, where sensitive employee data is involved.

---

:octicons-clock-16: <span class="gsoc-legend">2:00 pm - 2:30 pm</span>  
<span id="talk-7" class="gsoc-topic-section-title">Building a Privacy-First
Ecosystem: How SMPC is Transforming AdTech Analytics</span>

???+ info "Adrián Vaca Humanes (Engineering Lead, Resolve) and Daniele Romanini (Senior Privacy Engineer, Resolve)"

    <div style="display: flex; align-items: flex-start; gap: 15px;">
        <img src="images/speakers/adrian-vaca-humanes.jpg" alt="Adrián Vaca Humanes" style="width: 150px; border-radius: 8px;">
        <p>Adrián Vaca Humanes is the Engineering Lead at Resolve, focusing on Data Analytics and Cloud Architecture. 
        His experience spans various industries, including banking, consumer discretionary, and telecommunications. At Resolve, he is dedicated to 
        building privacy-preserving solutions for the AdTech industry, with a current focus on data engineering, platform optimization, 
        and cost reduction for products based on Secure Multi-Party Computation (SMPC).</p>
    </div>
    
    <div style="display: flex; align-items: flex-start; gap: 15px; margin-top: 15px;">
        <img src="images/speakers/daniele-romanini.jpg" alt="Daniele Romanini" style="width: 150px; border-radius: 8px;">
        <p>Daniele Romanini is a Senior Privacy Engineer at Resolve, bringing expertise in both data science and software engineering. 
        His background includes experience in academia, government organizations, and the AdTech industry. Daniele is an advocate for 
        privacy-by-design and a privacy tech enthusiast, actively integrating privacy threat modeling and a privacy-first approach 
        into the software development lifecycle. He is currently focused on contributing to the development of a decentralized 
        measurement and analytics platform built with privacy-enhancing technologies at its core.</p>
    </div>

???+ abstract "Abstract"

    In a rapidly evolving landscape marked by growing user awareness of privacy and increasingly tightening privacy regulations, 
    the AdTech industry faces challenges to remain relevant and profitable. With increased attention to privacy-enhancing 
    technologies, including decentralized computation, Resolve is building decentralized programmatic advertising solutions that 
    emphasize privacy. Secure Multi-Party Computation (MPC) is a fundamental tool in Resolve's arsenal. Specifically, Resolve 
    leverages Carbyne Stack to construct a general-purpose collaborative analytics platform for the AdTech industry. This talk 
    will demonstrate how Carbyne Stack can be applied in programmatic advertising, detailing concrete use cases for collaborative 
    analytics. Additionally, the presentation will dive into technical aspects, addressing scalability and security challenges 
    encountered in real-world deployments.

---
:octicons-clock-16: <span class="gsoc-legend">2:30 pm - 3:00 pm</span>  

<span id="talk-8" class="gsoc-topic-section-title">Silentium: Beaver Triple
Generation with Low Communication and Hardware Acceleration</span>

???+ info "Dr. Ian Zhou (Research Engineer, UTS) and Vincent Rieder (PhD Student, Bosch Research)"

    <div style="display: flex; align-items: flex-start; gap: 15px; margin-top: 10px;">
        <img src="images/speakers/ian-zhou.jpg" alt="Dr. Ian Zhou" style="width: 150px; border-radius: 8px; margin-top: 10px;">
        <p> Dr. Ian Zhou received his B.S. degree in computer science from The University of Sydney in 2016 and completed his M.B.A. and 
        Ph.D. degrees at the University of Technology Sydney in 2019 and 2023, respectively. His Ph.D. research focused on 
        machine learning-based frost monitoring systems. Currently, Dr. Zhou is a Research Engineer specializing in privacy-preserving 
        technologies, working on accelerating multi-party secure machine learning algorithms with CUDA and other GPU platforms. His 
        research interests include AI, IoT, cyber-physical systems, and blockchain.</p>
    </div>
    
    <div style="display: flex; align-items: flex-start; gap: 15px; margin-top: 15px;">
        <img src="images/speakers/vincent-rieder.jpeg" alt="Vincent Rieder" style="width: 150px; border-radius: 8px; margin-top: 10px;">
        <p> Vincent Rieder is a PhD student at Bosch Research in Renningen, focusing on Secure Multi-Party Computation (MPC). Holding a 
        Master’s degree in mathematics, his research centers on the algorithmic aspects of MPC. He aims to introduce new protocols 
        into the Carbyne Stack MPC cloud platform, specifically by optimizing and implementing an enhanced MPC offline phase for 
        generating Beaver triples using a Pseudorandom Correlation Generator (PCG) with low communication overhead, named *Silentium*.</p>
    </div>

???+ abstract "Abstract"

    The MPC framework of Carbyne Stack relies on the SPDZ protocol, where the most resource-intensive task is the generation of 
    Beaver triples in the offline phase. A recent advancement for offline phases, particularly relevant for the cloud context, 
    involves Pseudorandom Correlation Generators (PCGs) with reduced communication demands. This talk introduces *Silentium*, 
    a PCG implementation designed to generate Beaver triples with hardware acceleration. The first part of the presentation will 
    cover *Silentium*’s design principles, highlighting how this approach outperforms previous offline phases in MP-SPDZ. The second 
    part will focus on how further enhance *Silentium*’s local phase with a GPU-optimized Number Theoretic Transform for large 
    degrees. Altogether, this talk discusses how *Silentium* can enhance Klyshko, the offline phase engine of Carbyne Stack, potentially 
    offering significant cost savings in future applications.

---
:octicons-clock-16: <span class="gsoc-legend">3:00 pm - 3:15 pm</span>  
:material-coffee: <span class="gsoc-legend">Coffee Break</span>

---
:octicons-clock-16: <span class="gsoc-legend">3:15 pm - 3:45 pm</span>

<span id="talk-9" class="gsoc-topic-section-title">Thymus: Adding essential
Security Features to the Carbyne Stack platform the Cloud-Native Way</span>

???+ info "Dr. Sven Trieflinger (Senior Project Manager, Bosch Research), Sebastian Becker (Research Engineer, Bosch Research), and Dr. Benjamin Hettwer (Research Engineer, Bosch Research)"

    <div style="display: flex; align-items: flex-start; gap: 15px;">
        <img src="images/speakers/sebastian-becker.jpeg" alt="Sebastian Becker" style="width: 150px; border-radius: 8px;">
        <p>Sebastian Becker is a Research Engineer at Robert Bosch GmbH. His work focuses on making privacy-enhancing technologies easily adaptable to the needs of the wide range of 
        application domains at Bosch. In this context, Sebastian also works as one of the maintainers of and main contributors to Carbyne Stack.</p>
    </div>
    
    <div style="display: flex; align-items: flex-start; gap: 15px; margin-top: 15px;">
        <img src="images/speakers/benjamin-hettwer.jpeg" alt="Benjamin Hettwer" style="width: 150px; border-radius: 8px;">
        <p> Dr. Benjamin Hettwer is a research engineer at Robert Bosch Corporate Research in Renningen, Germany. He received his Ph.D. in Electrical Engineering and Information Technology 
        from Ruhr-Universität Bochum, Germany, in 2020. His research interests focus on the intersection of hardware security and machine learning. Recently, he also started 
        working on privacy-enhancing technologies such as the secure multi-party computation solution Carbyne Stack and its application to industrial use cases.</p>
    </div>

    <div style="display: flex; align-items: flex-start; gap: 15px; margin-top: 15px;">
        <img src="images/speakers/sven-trieflinger.png" alt="Sven Trieflinger " style="width: 150px; border-radius: 8px;">
        <p> Dr. Sven Trieflinger is a Senior Project Manager, Group Manager for Security, Privacy, and Safety, Research Engineer, and open source software maintainer at Bosch Research. 
        He has over 15 years of experience in the design, architecture, and implementation of distributed systems and cloud platforms. With his team at Bosch, Sven drives innovation in 
        the area of privacy-preserving computing technologies and is spearheading open source computing on encrypted data technology with the Carbyne Stack cloud-native Secure Multiparty 
        Computation platform.</p>
    </div>

???+ abstract "Abstract"

    There is a new kid on the block: Thymus adds eagerly awaited security-related capabilities to the Carbyne Stack platform. In this talk, we will share, how we are providing versatile 
    authentication via Ory Kratos, flexible authorisation based on the Open Policy Agent, and security for inter-VCP communication channels and user-facing APIs via 
    Istio Kubernetes-native network machinery. We will discuss the rationale behind technology choices and provide insights into how to use the new features.

---
:octicons-clock-16: <span class="gsoc-legend">3:45 pm - 4:15 pm</span>  
<span id="talk-10" class="gsoc-topic-section-title">Integrating the BU Secure
Analytics Stack with Carbyne Stack</span>

???+ info "Dr. John Liagouris (Assistant Professor, Boston University)"

    <div style="display: flex; align-items: flex-start; gap: 15px; margin-top: 15px;">
        <img src="images/speakers/john-liagouris.jpeg" alt="Dr. John Liagouris" style="width: 150px; border-radius: 8px;">
        <p> Dr. John Liagouris is an assistant professor of Computer Science at Boston University, where he co-leads the Complex Analytics 
        and Scalable Processing research lab (CASP). He is a member of the Systems Group and is also affiliated with the Security 
        Group at BU. John's research focuses on distributed systems, cloud computing, security & privacy, and data management. 
        Prior to joining BU, he was a visiting scholar at the RISELab, UC Berkeley, a senior researcher at the Systems Group, ETH 
        Zurich, a visiting research fellow at the University of Hong Kong (HKU), and a research assistant at the “Athena” Research 
        Center in Greece. John earned his PhD from NTUA, Greece. His work has received several awards, including an “Outstanding 
        New Research Direction Award” at Usenix HotStorage 2020, a NSF SaTC Core Medium Award, a Bosch Research Award, and a Red 
        Hat Collaboratory Research Incubation Award.</p>
    </div>

???+ abstract "Abstract"

    In this talk, Dr. John Liagouris will present the latest developments in the BU secure analytics stack and its integration 
    with Carbyne Stack. The BU stack is a novel software stack developed at Boston University to facilitate general-purpose analytics 
    using secure Multiparty Computation (MPC). Its current version supports both relational and time series computations on 
    millions of input records, with configurable semi-honest or malicious security settings. Built from scratch, the BU stack 
    encompasses efficient implementations of low-level MPC functionalities up through high-level operators and programming 
    abstractions. All stack layers, except the bottom one, are protocol-agnostic, employing a hierarchical and modular design 
    that maximizes reusability and extensibility. Improvements in any component of the stack can cascade across other parts, 
    enhancing overall functionality. This talk will delve into the BU stack's 4-year research journey, detailing the innovation 
    led by the Systems Group and Security Group at BU.

---
:octicons-clock-16: <span class="gsoc-legend">4:15 pm - 4:45 pm</span>  
<span id="talk-11" class="gsoc-topic-section-title">Open Source MPC - Quo
Vadis</span>

???+ info "Dr. Brian LaMacchia (Executive Director, MPC Alliance), Dr. Sven Trieflinger (Senior Project Manager, Bosch Research), and Dr. Christian Höppler (Open Source Officer, Bosch Research)"

    <div style="display: flex; align-items: flex-start; gap: 15px; margin-top: 15px;">
        <img src="images/speakers/brian-lamacchia.jpg" alt="Brian LaMacchia" style="width: 150px; border-radius: 8px;">
        <p> Dr. Brian LaMacchia is an applied cryptographer and currently the Executive Director of the MPC Alliance, a consortium of over 50 organizations promoting secure multi-party 
        computation (MPC) technology. After a 25-year career at Microsoft Corporation, where he served as Distinguished Engineer for Cryptography and led the Security and Cryptography 
        team at Microsoft Research, Brian retired in December 2022. He also co-founded and chaired the Microsoft Cryptography Review Board. Beyond his role at the MPC Alliance, Brian is an 
        Adjunct Associate Professor at Indiana University-Bloomington’s School of Informatics and Computing, an Affiliate Faculty member at the University of Washington’s Department of 
        Computer Science and Engineering, and an Advisor to Quantropi, Inc. Brian currently serves as Treasurer for the International Association for Cryptologic Research (IACR) and as 
        Vice President on the Board of Directors of Seattle Opera. He earned his S.B., S.M., and Ph.D. degrees in Electrical Engineering and Computer Science from MIT in 
        1990, 1991, and 1996, respectively.</p>
    </div>

    <div style="display: flex; align-items: flex-start; gap: 15px; margin-top: 15px;">
        <img src="images/speakers/sven-trieflinger.png" alt="Sven Trieflinger " style="width: 150px; border-radius: 8px;">
        <p> Dr. Sven Trieflinger is a Senior Project Manager, Group Manager for Security, Privacy, and Safety, Research Engineer, and open source software maintainer at Bosch Research. 
        He has over 15 years of experience in the design, architecture, and implementation of distributed systems and cloud platforms. With his team at Bosch, Sven drives innovation in 
        the area of privacy-preserving computing technologies and is spearheading open source computing on encrypted data technology with the Carbyne Stack cloud-native Secure Multiparty 
        Computation platform.</p>
    </div>

    <div style="display: flex; align-items: flex-start; gap: 15px; margin-top: 15px;">
        <img src="images/speakers/christian-hoeppler.jpg" alt="Christian Höppler" style="width: 150px; border-radius: 8px;">
        <p> Dr. Christian Höppler is Open Source Officer at Bosch Research and member of Bosch's corporate Open Source Expert Team. Since joining Bosch Research a decade ago Chris has been 
        working on a wide range of open source topics from compliance to strategy. Currently, he focuses on working with internal teams to help with their open source contributions both 
        within Bosch Research and beyond. To support that work and to reduce some of the friction encountered he has begun to automate the contribution lifecycle. Additionally, Chris has 
        been a member of the research project "Economy of Things", which is working towards well-governed decentralized systems for a digital economy. Prior to that he's been working in 
        the automotive sector, mainly developing test automation software for hardware-in-the-loop testbenches.</p>
    </div>

???+ abstract "Abstract"

    Three years ago, we set sail on a quest to create the most advanced cloud-native Secure Multiparty Computation (MPC) platform. 
    Our goal was to plant a seed for an open ecosystem where state-of-the-art MPC technology can thrive. Building Carbyne Stack “in the open” has 
    been a deliberate choice and open source software plays a pivotal role as it fosters collaboration and transparency, allowing for continuous peer 
    review and rapid identification of vulnerabilities. As we approach a state where Carbyne Stack implements all the essential features for deployment in real-world use cases, 
    we believe it's time to rethink how we work together and decide on what a happy home for the Carbyne Stack community should look like. In this tag-team talk, Brian will 
    discuss the history and  importance of open source in the development of cryptographic software, then Sven and Chris will relate what Brian discusses to the history of Carbyne S
    tack and shed some light on the way forward for the initiative. 

---
:octicons-clock-16: <span class="gsoc-legend">4:45 pm - 5:00 pm</span>  
:material-factory: <span class="gsoc-legend">Closing Remarks</span>
---

### **Workshop Day (Nov 28)**

:octicons-clock-16: <span class="gsoc-legend">9:00 am - 10:30 am</span>  
<span class="gsoc-topic-section-title">Donating Carbyne Stack to an Open Source
Foundation</span>

???+ info "Details"

    _Carbyne Stack is coming of age._ Initially launched as a Bosch Research initiative,
    we think it is time to transition the project to a community-driven model. This workshop
    will explore the benefits, potential foundation options, and the process required for
    such a donation. Key discussion points include:

    - Advantages of transitioning Carbyne Stack to an open source foundation
    - Selection of the most suitable foundation
    - Steps and tasks involved in the donation process
    - Roles and responsibilities of funders post-donation
    - Governance structure for a community-led project

---

:octicons-clock-16: <span class="gsoc-legend">10:30 am - 11:00 am</span>  
:material-coffee: <span class="gsoc-legend">Coffee Break</span>
---

:octicons-clock-16: <span class="gsoc-legend">11:00 am - 12:30 pm</span>  
<span class="gsoc-topic-section-title">Generalizing Carbyne Stack for
Multi-Runtime / Multi-Protocol MPC Support</span>

???+ info "Details"

    Carbyne Stack is currently tightly integrated with MP-SPDZ as the underlying MPC engine,
    specifically in a malicious majority setting. This approach prioritizes security, but it
    may not always be feasible due to performance and cost considerations. To maximize the
    platform's value, this workshop will discuss ongoing efforts by various partners to support
    additional protocols and explore how to create a unified, more flexible framework that
    supports multiple MPC engines and protocols.

---

:octicons-clock-16: <span class="gsoc-legend">12:30 pm - 1:30 pm</span>  
:material-food: <span class="gsoc-legend">Lunch Break</span>
---

:octicons-clock-16: <span class="gsoc-legend">1:30 pm - 3:00 pm</span>  
<span class="gsoc-topic-section-title">Walk-in Topics</span>

???+ info "Details"

    Open time for additional discussions, spontaneous topics, and networking.

---

<!-- markdownlint-restore -->

## Venue Information

<div class="grid cards" markdown>

  - <p>:octicons-location-16:{ .middle } Bosch Research Campus</p>
  
    ---

    ![Bosch Research Campus][bosch-research-campus]
  
    <img alt="CarbyneStackCon Logo" src="/images/events/csc/24/clubhouse-rng.jpg"/>
  
    **Address**
  
    Robert-Bosch-Campus 1 <br>
    71272 Renningen, Germany
  
    [:octicons-arrow-right-24:{ .middle } Bosch Research website][bosch-research]

</div>

### Getting There

The Bosch Research Campus is located near Stuttgart. See our
[travel information sheet][bosch-research-campus-directions] for information
on how to get to the Research Campus by car or public transport.

---

## Accommodation

There are numerous hotels in and around Renningen. One within walking distance to the venue is:

[Hotel Campo][hotel-campo] <br>
Raitestraße 26 <br>
71272 Renningen <br>
:material-phone: +49 7159 939800 <br>
:material-email-outline: info@campo-renningen.de

---

## Catering

Food and beverages during the event will be complementary.

---

[bosch-research]: https://www.bosch.com/research/

[bosch-research-campus]: https://upload.wikimedia.org/wikipedia/commons/thumb/a/a0/BoschRenningen-pjt.jpg/1280px-BoschRenningen-pjt.jpg

[bosch-research-campus-directions]: rng_directions.pdf

[hotel-campo]: https://www.campo-renningen.de/en
