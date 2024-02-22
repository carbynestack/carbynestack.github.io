# API Specification

The Carbyne Stack platform provides a comprehensive set of microservices,
working together to create a fully functional MPC-party. To facilitate
interaction and communication, each  service exposes a REST API. This API serves
as a standardized interface for accessing and utilizing the functionalities
provided by the individual services, to facilitate both, inter and intra VCP
communication, as well as interaction between clients (users) and the services.

The REST API of the individual Carbyne Stack services are documented using the
[OpenAPI](https://www.openapis.org/) Specification, a widely adopted standard
for describing RESTful APIs. This specification provides a comprehensive and
structured overview of the API endpoints, request and response payloads,
authentication mechanisms, and other important details.

The REST API documentation serves as a valuable resource for individuals
interested in implementing a client application or developing a new microservice
that leverages the capabilities of Carbyne Stack. By studying the API
description, developers can gain insights into the available functionality,
understand the expected input and output formats, and effectively integrate
their applications with Carbyne Stack.

However, it is important to note that for most users, leveraging the existing
clients provided is the recommended approach. These clients have already
implemented the necessary logic to interact with the REST API, abstracting away
the underlying complexities and providing a simplified interface for accessing
the services. Using the existing clients ensures a smoother and more efficient
integration with the Carbyne Stack platform, allowing users to focus on
utilizing the services rather than dealing with low-level API interactions.
