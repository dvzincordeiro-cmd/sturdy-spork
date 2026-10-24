# Architecture Overview

This diagram provides a high-level view of the system architecture and the primary interaction flow between clients, application services, and supporting infrastructure.

```mermaid
flowchart LR
    User[End Users] --> Web[Web / Mobile Client]
    Web --> CDN[CDN / Static Assets]
    Web --> API[API Gateway]

    API --> Auth[Authentication Service]
    API --> Users[User Service]
    API --> Catalog[Catalog Service]
    API --> Orders[Order Service]

    Users --> DB[(Primary Database)]
    Catalog --> DB
    Orders --> DB

    Auth --> Cache[(Redis Cache)]
    Users --> Cache

    API --> MQ[Message Broker]
    MQ --> Worker[Background Worker]
    Worker --> DB

    Monitoring[Monitoring & Logs] --> API
    Monitoring --> Worker
    Monitoring --> DB
```

## Overview

- Clients connect through a single API entry point.
- Core business services are separated by responsibility: authentication, users, catalog, and orders.
- A shared primary database stores transactional data.
- Redis provides fast access for session and caching needs.
- A message broker decouples asynchronous processing from the request path.
- Monitoring infrastructure observes traffic, background tasks, and data layer health.
