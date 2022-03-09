# Google Summer of Code

!!! warning "Rejected for GSoC 2022"

    Sadly, we have not been accepted for GSoC 2022. We plan to apply again in
    2023 and hope that we have better luck then. Stay tuned!

    Still the topics mentioned below are very relevant for the Carbyne Stack
    community. In case you are interested in working on one of them or you have
    your own ideas on how to advance the Carbyne Stack project or community,
    please do not hesitate to get in touch. We'll probably find a way to get you
    supported by one of the backing organizations. Please also have a look at
    the [opportunities](../opportunities) page for open positions. 

Carbyne Stack is applying to become a [Google Summer of Code (GSoC) 2022][gsoc]
mentoring organization. For those not aware of what GSoC is, here comes the
official description from Google:

!!! cite "What is _Google Summer of Code_?"

    _Google Summer of Code_ is a global, online program focused on bringing new
    contributors into open source software development. 
    (Source: [GSoC Website][gsoc])

[gsoc]: https://summerofcode.withgoogle.com/

Students interested in becoming a contributor to the Carbyne Stack open source
project as part of the GSoC programme can find all the information related to
our participation as a mentoring organization on this page.

## Application 101

Want to apply for working with Carbyne Stack in GSoC 2022? Great! Read below
what it needs to get things going[^1].

### How to Apply

!!! warning "Application only via GSoC System"

    All applications must go through Google's GSoC application system.
    We can't accept any application unless it is submitted there.

Applying is not witchcraft as long as you take the following points to heart:

1. Read carefully all the information provided on this page and in the GSoC
   [student guide][gsoc-student-guide].
2. Get in touch with the Carbyne Stack maintainers
   (:material-email-box: [Caryne Stack Maintainers](mailto:rng_cr_carbynestack@bosch.com))
   or with your prospective mentors (see contact information below in the
   topics list) in case you want to discuss your project idea or understand
   better the expectations towards GSoC applicants.
3. Write your application. See hints on this below.
4. Submit your application to Google before the deadline. We actually recommend
   you to submit a few days early in case you have internet problems or the
   system is down. Google does not extend this deadline, so it's best to be
   prepared early! You can edit your application up until the system closes.

### Anatomy of a good Application

A promising application will contain at least:

1. A **descriptive title** for the project. In case you are interested in one
   of the projects proposed by us that should be straightforward. Otherwise,
   put some effort into to this as it's the first opportunity to trigger the
   interest of potential mentors.
2. A **concise CV** with a special focus on your practical experiences with
   relevant technology and open source projects, including contact information.
   No problem if you don't have that much experience yet, as long as you have
   the skills to master the envisioned project.
3. Information about **your (proposed) project**. This should be reasonably
   detailed and include a timeline with concrete milestones. Please also
   include any learning tasks that you anticipate to get you prepared for
   mastering your GSoC project.
4. Information about **other commitments** that might affect your ability to
   work during the GSoC period (exams, classes, holidays, other jobs, weddings,
   etc.). We can work around a lot of things, but it helps to know in advance.

[^1]:
    Based on the excellent guideline examples given on the GSoC website, in
    particular the [Python GSoC website][python-gsoc].

## Topics

We have an attractive mix of potential GSoC project proposals. Each project is
rated according to effort (:material-timer-sand-empty:, either `175h` or `350h`)
and difficulty / complexity (:material-certificate-outline:, one of `Easy`,
`Medium`, or `Hard`). While projects rated low complexity tend to require
adapting a single or a small number of Carbyne Stack software components (like a
microservice) only, those with high complexity require contributors to get a
good understanding of the Carbyne Stack system as a whole or require in-depth
knowledge of a single or a few components.

!!! info "BYOT (Bring Your Own Topic)"

    In case you have your own small or big idea how to advance Carbyne Stack
    or our open source community, don't hesitate to get in touch
    (:material-email-box: [GSoC Organization Administrator](mailto:rng_cr_carbynestack@bosch.com))
    to discuss your proposal.

!!! tip

    Please use the tabs below to navigate through the GSoC project proposals.

=== "Core Development"

    _Core development_ projects derive from the ongoing work from the core of our
    development team. The list of features and bugs is never-ending, and help
    is always welcome. Expect to be closely integrated into the frequent
    interactions of the Carbyne Stack core development team.

    === "Carbyne Stack Operator"

        <span class="gsoc-topic-section-title">Carbyne Stack Operator</span>

        :material-timer-sand-empty: <span class="gsoc-legend">175h (Basic), 350h (Extended)</span>
        :material-certificate-outline: <span class="gsoc-legend">Hard</span>

        Operators are software extensions to Kubernetes that make use of custom
        resources to manage applications and their components. Carbyne Stack is 
        a complex system consisting of multiple independently scalable 
        microservices with interwoven configuration requirements. This project
        is about implementing a **Kubernetes operator that can be used to
        deploy, configure, and operate a Carbyne Stack virtual cloud provider**
        and to establish virtual clouds by interconnecting multiple of them.

        <span class="gsoc-topic-section-h1">Expected Outcomes</span>

        !!! success "Value Proposition"
        
            DevOps will be able to easily deploy and operate Carbyne Stack
            virtual clouds.

        <span class="gsoc-topic-section-h2">Technical Key Results</span>

        === "Basic (175h)"

            - A set of basic operators for Carybne Stack microservices, i.e.,
              Castor, Amphora, Ephemeral is available.

        === "Extended (350h)"

            - A set of basic operators for Carybne Stack microservices, i.e.,
              Castor, Amphora, Ephemeral is available.
            - An additional "super" operator is implemented that can be used to
              deploy a complete Carbyne Stack virtual cloud provider and that
              provides mechanisms to establish a Carbyne Stack virtual cloud 
              by partnering with remote virtual cloud providers. This process
              includes things like key generation, CA certificate exchange, and
              parameter negotiation.

        <span class="gsoc-topic-section-h1">Required Skills</span>

        - Advanced coding skills in Go or Java
        - _(Bonus)_ Experience with an operator framework, e.g., Operator 
          SDK, kubebuilder, or the Java Operator SDK
        - _(Bonus)_ Knowledge on Carbyne Stack including how microservices work
          and how their configurations "interact"

        <span class="gsoc-topic-section-h1">Potential Mentors</span>
        
        - [Sven Trieflinger](https://github.com/strieflin) (Robert Bosch GmbH)

    === "Cloud-native Authentication and Authorization"

        <span class="gsoc-topic-section-title">Cloud-native Authentication and Authorization</span>

        :material-timer-sand-empty: <span class="gsoc-legend">350h</span>
        :material-certificate-outline: <span class="gsoc-legend">Medium</span>

        In this project, the CNCF projects [Dex][dex] and [OPA][opa] will be
        used to implement **OIDC authentication and policy-based authorization
        in the Carbyne Stack microservices, clients and CLI** in a cloud-native way.

        <span class="gsoc-topic-section-h1">Expected Outcomes</span>

        !!! success "Value Proposition"

            End-users will be able to define who is able to access Amphora
            secrets, Castor tuples, and Ephemeral functions.

        <span class="gsoc-topic-section-h2">Technical Key Results</span>

        - Authorization support is implemented in Carbyne Stack microservices
          including Amphora, Castor, and Ephemeral. 
        - End-user authentication support is set up using Istio features.
        - Support for Authentication and Authorization is implemented in Java
          clients and CLI.

        <span class="gsoc-topic-section-h1">Required Skills</span>

        - Good coding skills in Java and Go
        - Experience in working with K8s
        - Basic knowledge about authentication standards, in particular OIDC
        - _(Bonus)_ Experience with Dex and OPA

        <span class="gsoc-topic-section-h1">Potential Mentors</span>
        
        - [Sebastian Becker](https://github.com/sbckr) (Robert Bosch GmbH)
        - [Sven Trieflinger](https://github.com/strieflin) (Robert Bosch GmbH)
        - [Jared Weinfurtner](https://github.com/jaredweinfurtner) (Robert Bosch GmbH)

    === "Migrate to gRPC"

        <span class="gsoc-topic-section-title">Migrate to gRPC</span>

        :material-timer-sand-empty: <span class="gsoc-legend">175h (Basic), 350h (Extended)</span>
        :material-certificate-outline: <span class="gsoc-legend">Easy</span>

        As of today, communication in Carbyne Stack is based on a mix of REST
        and WebSockets. In this project, you get your feets wet with gRPC, the
        CNCF high performance, open source universal RPC framework  and use 
        **gRPC to replace the communication protocols** used currently in
        Carbyne Stack.

        <span class="gsoc-topic-section-h1">Expected Outcomes</span>

        !!! success "Value Proposition"

            Developers will be able to use a modern and fast RPC framework to
            interact with the Carbyne Stack services.

        <span class="gsoc-topic-section-h2">Technical Key Results</span>

        === "Basic (175h)"

            - Castor service and clients are migrated to gRPC.

        === "Extended (350h)"

            - Castor service and clients are migrated to gRPC.
            - Amphora service and clients are migrated to gRPC.

        <span class="gsoc-topic-section-h1">Required Skills</span>

        - Good coding skills in Java
        - Solid understanding of the RPC and REST paradigms
        - _(Bonus)_ Previous experience in working with gRPC

        <span class="gsoc-topic-section-h1">Potential Mentors</span>

        - [Sebastian Becker](https://github.com/sbckr) (Robert Bosch GmbH)        
        - [Sven Trieflinger](https://github.com/strieflin) (Robert Bosch GmbH)
        - [Jared Weinfurtner](https://github.com/jaredweinfurtner) (Robert Bosch GmbH)

=== "Infrastructure & Automation"

    _Infrastructure and automation_ projects are about things somewhat
    orthogonal to core development. Expect to work a little bit more
    indepedently but not really detached from the core developer team.
    Nevertheless, you will contribute things equally important that will be
    highly appreciated by the community.

    === "Security Hardening"

        <span class="gsoc-topic-section-title">Security Hardening</span>

        :material-timer-sand-empty: <span class="gsoc-legend">175h (Basic), 350h (Extended)</span>
        :material-certificate-outline: <span class="gsoc-legend">Medium</span>

        This project is about **securing Carbyne Stack deployments in various
        aspects** including east-west (intra-VCP) and north-south (inter-VCP)
        traffic using the open source Istio service mesh, Kubernetes RBAC
        configuration, and secrets management using Hashicorp Vault.

        <span class="gsoc-topic-section-h1">Expected Outcomes</span>

        !!! success "Value Proposition"

            DevOps will be able to securely operate CS virtual cloud providers.

        <span class="gsoc-topic-section-h2">Technical Key Results</span>

        === "Basic (175h)"

            - East-west (intra-VCP) inter-service communication channels are
              secured using Istio service-to-service Mutual TLS authentication.
            - North-south (inter-VCP) communication channels are secured using
              an Istio mutual TLS ingress gateway.
            - Namespace isolation and RBAC policies for Carbyne Stack K8s
              resources are in place.

        === "Extended (350h)"

            - East-west (intra-VCP) inter-service communication channels are
              secured using Istio service-to-service Mutual TLS authentication.
            - North-south (inter-VCP) communication channels are secured using
              an Istio mutual TLS ingress gateway.
            - Namespace isolation and RBAC policies for Carbyne Stack K8s
              resources are in place.
            - Secret management is implemented using Hashicorp Vault for MPC
              MAC and encryption keys, as well as local and remote 
              certificates.

        <span class="gsoc-topic-section-h1">Required Skills</span>

        - Experience in working with K8s
        - Solid understanding of TLS and Istio
        - _(Bonus)_ Previous experience with Hashicorp Vault

        <span class="gsoc-topic-section-h1">Potential Mentors</span>
        
        - [Sven Trieflinger](https://github.com/strieflin) (Robert Bosch GmbH)

    === "Public Cloud Deployment"

        <span class="gsoc-topic-section-title">Public Cloud Deployment</span>

        :material-timer-sand-empty: <span class="gsoc-legend">175h</span>
        :material-certificate-outline: <span class="gsoc-legend">Easy</span>

        Carbyne Stack currently comes along with instructions for local
        deployment of a virtual cloud on kind clusters using MetalLB as the
        bare-metal load balancer. The goal of this project is to **provide
        modern deployment machinery for a public cloud** using the
        [Terraform][terraform] infrastructure as code software tool.

        <span class="gsoc-topic-section-h1">Expected Outcomes</span>

        !!! success "Value Proposition"

            DevOps will be able to deploy Carbyne Stack virtual cloud providers
            to a public cloud.

        <span class="gsoc-topic-section-h2">Technical Key Results</span>

        - IaC code for deploying a Carbyne Stack virtual cloud (provider) to
          Azure based on Terraform and helm is available.

        <span class="gsoc-topic-section-h1">Required Skills</span>

        - Basic skills in working with an IaC tool
        - Familarity with Helm
        - Experience in working with a public cloud
        - _(Bonus)_ Experience with Azure Public Cloud and Terraform

        <span class="gsoc-topic-section-h1">Potential Mentors</span>
        
        - [Sven Trieflinger](https://github.com/strieflin) (Robert Bosch GmbH)

## FAQ

<span class="gsoc-topic-section-h1">Why do we want to participate in
GSoC?</span>

We are passionate about the ideas behind Carbyne Stack and the possibilities
cloud-native Secure Multiparty Computation opens up for secure and
privacy-friendly processing of sensitive data. With Carbyne Stack being a
security-related enabling technology project, we think going down the open
source path is exactly the right way to go.

By participating in the GSoC program, we aim to attract the interest of
tech-savvy, highly skilled people who are passionate about open source and help
them expand their skills while making useful contributions to the Carbyne Stack
platform to make it even better.

---

<span class="gsoc-topic-section-h1">What would we consider to be a successful
GSoC?</span>

Our foremost goal is to spark long-term interest in Carbyne Stack. A successful
GSoC would give us active community members who have come to stay.

As a young open source project and first-time participant in the GSoC program,
there is plenty for us to learn. In the best case, this year's participation
will lay the foundation for regular participation in the program.

We believe that the GSoC projects we propose will add significant value to the
Carbyne Stack developer and user community. We will do our best to ensure that
GSoC contributions actually find their way into the Carbyne Stack codebase.

---

<span class="gsoc-topic-section-h1">How will we keep mentors engaged with their
GSoC contributors?</span>

We only accept mentors who we know well, who are active members of the
community, and who are fully aware of their responsibilities as GSoC mentors.

We foster the exchange of knowledge and best practices between mentors in
regular mentoring meetings.

As administrators of the mentoring organization, we see it as one of our main
tasks to keep the mentors engaged and spend enough time to make sure that
everything runs smoothly.

---

<span class="gsoc-topic-section-h1">How will we keep GSoC contributors
involved in our community during (and after) GSoC?</span>

We host community meetings every two weeks. GSoC contributors are invited to
attend these meetings to present their work, interact with community members,
and receive the input and guidance they need to achieve their project goals.
Weekly meetings with their mentors help participants learn about relevant
activities within the community. All (written) interaction within the community
takes place on GitHub, which keeps the barriers to entry low for GSoC students.

The participating industry partners regularly offer opportunities
to interested students to do internships or academic theses on topics
related to Carbyne Stack within their company. This might be interesting
for certain GSoC contributors as a way to keep in touch with the community
after GSoC will have come to an end.

---

<span class="gsoc-topic-section-h1">How will we help our GSoC contributors stay
on schedule to complete their projects?</span>

We will implement the following measures to ensure the success of GSoC projects
for both GSoC students and us:

- Mentors will meet weekly with their mentees to identify and address emerging
  issues early.
- GSoC contributors are invited to attend our regular community meetings, and
  are encouraged to interact with others in the community via regular GitHub
  communication channels or email.
- Code reviews are an integral part of our routine within the Carbyne Stack
  community - not only as a quality measure, but also as a means of knowledge
  transfer. This routine is naturally extended to GSoC contributions as well.

<span class="gsoc-topic-section-h1">Have we been accepted as a GSoC mentor
organization before?</span>

Carbyne Stack is a young open source project kick-started some months back in
September 2021 by means of an initial contribution by Bosch Research.
Therefore, it is our first time applying as GSoC mentoring organization.

<span class="gsoc-topic-section-h1">Where does our source code live?"</span>

Our project and community is hosted on [GitHub][github] under the
[Carbyne Stack organization][csorg].

[github]: https://github.com/
[csorg]: https://github.com/carbynestack
[dex]: https://github.com/dexidp/dex
[opa]: https://www.openpolicyagent.org/
[terraform]: https://www.terraform.io/
[python-gsoc]: https://python-gsoc.org/index.html
[gsoc-student-guide]: https://google.github.io/gsocguides/student/
