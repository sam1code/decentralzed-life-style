# Architecture Diagram
### Paste it in the mermaid chart to get a picturial view

```

flowchart LR
 subgraph Legend["Legend"]
        B["Kafka/Event Bus"]
        A["REST/gRPC"]
  end
 subgraph Observability["Observability"]
        Prometheus["Prometheus"]
        Grafana["Grafana"]
        Loki["Loki/EFK"]
        Jaeger["Jaeger Tracing"]
  end
 subgraph CronJobs["Scheduled Jobs"]
    direction TB
        NotificationCron["Notification Cron Jobs<br>(reminders, challenges)"]
        EmailCron["Email Cron Jobs<br>(batch emails, reports)"]
  end
    A --- B
    ClientApps["Client Apps<br>(Web, Mobile, Wallet)"] -- REST/gRPC --> APIGateway["API Gateway<br>(OAuth2, JWT, Rate Limit, Multi-Tenant)"]
    APIGateway -- Auth Token Validation --> AuthService["Central Auth Service<br>(OAuth2, JWT, mTLS gRPC, OPA Policies)"]
    AuthService --- SecretsVault["Secrets Manager<br>(Vault)"] & IdentityService["Identity Service<br>(Decentralized ID - DID, DIDComm)"] & WalletService["Wallet Service<br>(Crypto Wallet, Token Balances)"] & EcommerceService["E-Commerce Service<br>(Catalog, Orders, Payments)"] & FitnessService["Fitness Service<br>(Workouts, Nutrition, Rewards)"] & MiningService["Mining Service<br>(Activity Tracking, Token Minting)"] & NotificationService["Notification Service<br>(Push, Email, SMS, Cron Jobs)"] & EmailService["Email Service<br>(Batch Emails, Reports via Cron)"] & AnalyticsService["Analytics Service<br>(Event Sourcing, ML, Dashboards)"] & RecommendationService["Recommendation Service<br>(Personalized Fitness &amp; Products)"] & Prometheus
    AuthService -- Policy Check --- UserService["User Service<br>(Profiles, Preferences)"]
    APIGateway -- gRPC --> UserService & EcommerceService & FitnessService & WalletService & MiningService & NotificationService
    UserService -- gRPC --> IdentityService
    FitnessService -- gRPC --> WalletService
    MiningService -- gRPC --> BlockchainNode["Blockchain Node<br>(Smart Contracts, Token Txn)"]
    WalletService -- gRPC --> BlockchainNode
    EcommerceService -. Kafka Event: OrderCreated .-> Kafka["Kafka Event Bus"]
    FitnessService -. Kafka Event: FitnessReward .-> Kafka
    MiningService -. Kafka Event: TokenMinted .-> Kafka
    NotificationService -. Kafka Event: NotificationQueued .-> Kafka
    EmailService -. Kafka Event: EmailBatchReady .-> Kafka
    Kafka -.-> EcommerceService & FitnessService & MiningService & NotificationService & EmailService & AnalyticsService & RecommendationService
    NotificationCron --> NotificationService
    EmailCron --> EmailService
    UserService --- RedisCache["Redis/Memcached Cache"] & Postgres["PostgreSQL<br>(Relational DB)"] & Prometheus
    EcommerceService --- RedisCache & DistributedStorage["Distributed Storage<br>(IPFS/Filecoin)"] & Postgres & Prometheus
    FitnessService --- RedisCache & DistributedStorage
    WalletService --- RedisCache & Postgres
    NotificationService --- RedisCache & Jaeger
    IdentityService --- MongoDB["MongoDB<br>(Document DB)"]
    AnalyticsService --- MongoDB & FeatureStore["Feature Store<br>(Feast or custom)"]
    RecommendationService --- FeatureStore
    NotificationService -- FCM|WEBSOCKET|SNS ---> ClientApps
    MiningService --- Loki
    CICD["CI/CD Pipeline<br>(ArgoCD, GitHub Actions, Helm, K8s)"] --- APIGateway & UserService
    Chaos["Chaos Testing<br>(Litmus)"] --- FitnessService & WalletService
    ServiceMesh["Service Mesh<br>(Istio/Linkerd, mTLS, Policy, Retry)"] --- UserService & WalletService & EcommerceService & FitnessService & MiningService

    style Legend fill:#f9f,stroke:#333,stroke-width:1px


```
