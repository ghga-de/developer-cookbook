# Design Highlights

This GHGA Architecture is based on modern design patterns that allow us to reliably address our major challenges in the areas of information security, scalability, and multi-organizational operation. Moreover, we are strongly invested into our ability to adapt to new and changing use cases e.g. to expand the project to new research areas and be prepared for collaborations with other community initiatives. In the following we highlight our main enabling design patterns:

## One API Approach

All GHGA services contribute to one generic API landscape that is not bound to a specific client, use case, or audience. Thus every step that can be carried out in our web application can also be performed in an automated fashion. Moreover, new clients addressing novel use cases can be developed at any point, even without the involvement of GHGA. This enables the integration with other software projects and outlines GHGA's ambitions to become a key infrastructure player in the biomedical community. Thereby, no compromises are made with respect to information security since all security checks are handled at the API level making it unnecessary to place trust in the client side which we potentially not control.

## Compliance with Community Standards

To tear down barriers for community interactions even further, we are deeply invested into the vision of the Global Alliance for Genomics & Health (GA4GH) that promotes a standard-based federated cloud infrastructure for genomics research. We employ all technical standards defined by the GA4GH that are relevant for the operation of a genome archive. This includes the GA4GH Passport format for describing user identities and authorities, the GA4GH Data Repository Service (DRS) as an API for standardized retrieval of research data, and the crypt4GH standard for state of the art encryption with minimal performance overhead.

## Event-driven Microservice Architecture

The GHGA software portfolio is realized as a choreography of independent microservices. Thereby, we primarily employ an event-driven inter-service communication paradigm. Compared to alternative approaches, such as the request-response model (e.g. based on REST or gRPC), our approach reduces the degree of coupling between microservices. This simplifies the development of individual microservices and increases their reusability within and beyond GHGA. Moreover, new features or requirements can often be satisfied by simply adding another microservice and letting it subscribe to already existing events streams. 

## Infrastructure Agnosticity

Since GHGA is federated across many organizations, its software has to be prepared to the highly diverse infrastructure available at the different organizations and we must avoid being locked into a technological platform, framework, or cloud provider. Thus we employ the Hexagonal Architecture pattern in which the application core solely focuses on the domain logic and is connected to the surrounding infrastructure through variable adapters. Support for a new infrastructure technology, such as an alternative database technology or a different message bus, can be realized by simply implementing a new adapter while the application core stays unchanged. An elaborate assay on the Hexagonal Architecture can be found [here](./articles/hexagonal.md)

## Hexagonal Microservice Chassis

To standardize and streamline the development of microservices, it is a common pattern to establish a so-called Microservice Chassis that centralizes cross-cuttings concerns, which are required in most or all microservices, into a single library or package. We identified a strong synergy between the Microservice Chassis pattern and the above-mentioned Hexagonal Architecture since most of the cross-cutting concerns deal with the communication between a microservice and its surrounding infrastructure. We thus proposed the Triple Hexagonal Architecture which extends the classical Hexagonal Architecture by dividing the adapters into two parts. One part that is Microservice-agnostic but specific to an infrastructure technology and can thus be deposited into a Microservice Chassis. The second part contains optional microservice-specific modifications to the first part. A detailed discussion of the design pattern can be found [here](./articles/3hexagonal.md). Based on this concept, we implemented a hexagonal Microservices Chassis with support for common technologies such as Apache Kafka, S3 Object Storages, and MongoDB. The open source library can be found on GitHub (https://github.com/ghga-de/hexkit) and PyPI (https://pypi.org/project/hexkit/) and is a general purpose solution for building Python-based microservices applicable well beyond the use-cases of GHGA.

## Zero Trust and Sandboxing

The Zero Trust paradigm is an alternative to the classical perimeter security. Instead of relying on the safety of internal networks and their protection e.g. through firewalls, the Zero Trust paradigm assumes no safe spaces and considers every network interaction, no matter whether happens internal or via the public internet, as a vulnerability. Thus, all internal and external network traffic happens in an encrypted fashion and rules restrict which network partner can talk to a given application component. We unleash the full potential of the Zero Trust paradigm by combining it with the Microservice pattern. The fine-grained compartmentalization of the application into microservices allows to effectively isolate especially vulnerable parts of the system. As an example, we could isolate the microservice managing encrypted research data from the microservice controlling the corresponding encryption secrets. As a result, an intruder would have to compromise both services to read the sensitive research data.

## Developers as First-Class Citizen
We identified the shortage of skilled IT professionals on the German job market as a key risk for the GHGA initiative. However, attracting outstanding talent is critical for the realization of our ambitious and technically demanding project goals. To compensate for our recruitment disadvantages, we treat the development experience as a priority which impacts high level design decisions. Developers at GHGA are given the opportunity to work with an uncompromisingly modern software stack and gain experiences with technologies of high demand in the job market of the future. Moreover, we let developers experience the benefits of academic freedom by leaving them room to address problems on a higher level resulting in applications and frameworks that are generically applicable well beyond the GHGA project. The aforementioned hexkit library is an example for such an open source framework. Like that we can create a wide spectrum of niches that allows both ambitious beginners and developer veterans to grow.  