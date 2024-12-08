Stateless Components:
├── Frontend (React/Angular)
├── API Gateway
├── Image Processing Service
└── Payment Processing Service
    
Stateful Components:
├── User Database (PostgreSQL)
├── Session Store (Redis)
├── Message Queue (Kafka)
└── File Storage (MongoDB)

Here's a column-wise comparison of 10 major stateful and stateless services:
Stateful ServicesStateless Services

Database Services (MySQL, MongoDB) | 1. Load Balancers (NGINX, HAProxy)
User Authentication Systems | 2. Static Content Servers (CDN)
Shopping Cart Services | 3. API Gateways
Session Management | 4. Image Processing Services
Email/Message Queues (Kafka, RabbitMQ) | 5. Log Processors
Gaming Leaderboard Systems | 6. Data Validation Services
Online Banking Systems | 7. Report Generators
Social Media Feeds | 8. File Format Converters
CRM Systems (Customer Data) | 9. Health Check Services
Order Management Systems | 10. Analytics Processors

Common Characteristics:
Stateful:

Must persist data
Needs backup/recovery
Sequential processing
Complex scaling
State replication needed

Stateless:

No data persistence
Easy to replace
Parallel processing
Simple scaling
No replication needed

