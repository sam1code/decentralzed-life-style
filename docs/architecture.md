# Architecture Diagram

```flowchart LR
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
NotificationCron["Notification Cron Jobs
(reminders, challenges)"]
EmailCron["Email Cron Jobs
(batch emails, reports)"]
end
A --- B
ClientApps["Client Apps
(Web, Mobile, Wallet)"] -- REST/gRPC --> APIGateway["API Gateway
(OAuth2, JWT, Rate Limit, Multi-Tenant)"]
APIGateway -- "Auth Token Validation" --> AuthService["Central Auth Service
(OAuth2, JWT, mTLS gRPC, OPA Policies)"]
AuthService --- SecretsVault["Secrets Manager
(Vault)"]
AuthService --- IdentityService["Identity Service
(Decentralized ID - DID, DIDComm)"]
AuthService --- WalletService["Wallet Service
(Crypto Wallet, Token Balances)"]
AuthService --- EcommerceService["E-Commerce Service
(Catalog, Orders, Payments)"]
AuthService --- FitnessService["Fitness Service
(Workouts, Nutrition, Rewards)"]
AuthService --- MiningService["Mining Service
(Activity Tracking, Token Minting)"]
AuthService --- NotificationService["Notification Service
(Push, Email, SMS, Cron Jobs)"]
AuthService --- EmailService["Email Service
(Batch Emails, Reports via Cron)"]
AuthService --- AnalyticsService["Analytics Service
(Event Sourcing, ML, Dashboards)"]
AuthService --- RecommendationService["Recommendation Service
(Personalized Fitness & Products)"]
AuthService --- Prometheus
AuthService -- "Policy Check" --> UserService["User Service
(Profiles, Preferences)"]
APIGateway -- gRPC --> UserService
APIGateway -- gRPC --> EcommerceService
APIGateway -- gRPC --> FitnessService
APIGateway -- gRPC --> WalletService
APIGateway -- gRPC --> MiningService
APIGateway -- gRPC --> NotificationService
UserService -- gRPC --> IdentityService
FitnessService -- gRPC --> WalletService
MiningService -- gRPC --> BlockchainNode["Blockchain Node
(Smart Contracts, Token Txn)"]
WalletService -- gRPC --> BlockchainNode
EcommerceService -. "Kafka Event: OrderCreated" .-> Kafka["Kafka Event Bus"]
FitnessService -. "Kafka Event: FitnessReward" .-> Kafka
MiningService -. "Kafka Event: TokenMinted" .-> Kafka
NotificationService -. "Kafka Event: NotificationQueued" .-> Kafka
EmailService -. "Kafka Event: EmailBatchReady" .-> Kafka
Kafka -.-> EcommerceService
Kafka -.-> FitnessService
Kafka -.-> MiningService
Kafka -.-> NotificationService
Kafka -.-> EmailService
Kafka -.-> AnalyticsService
Kafka -.-> RecommendationService
NotificationCron --> NotificationService
EmailCron --> EmailService
UserService --- RedisCache["Redis/Memcached Cache"]
UserService --- Postgres["PostgreSQL
(Relational DB)"]
UserService --- Prometheus
EcommerceService --- RedisCache
EcommerceService --- DistributedStorage["Distributed Storage
(IPFS/Filecoin)"]
EcommerceService --- Postgres
EcommerceService --- Prometheus
FitnessService --- RedisCache
FitnessService --- DistributedStorage
WalletService --- RedisCache
WalletService --- Postgres
NotificationService --- RedisCache
NotificationService --- Jaeger
IdentityService --- MongoDB["MongoDB
(Document DB)"]
AnalyticsService --- MongoDB
AnalyticsService --- FeatureStore["Feature Store
(Feast or custom)"]
RecommendationService --- FeatureStore
NotificationService -- "FCM|WEBSOCKET|SNS" --> ClientApps
MiningService --- Loki
CICD["CI/CD Pipeline
(ArgoCD, GitHub Actions, Helm, K8s)"] --- APIGateway
CICD --- UserService
Chaos["Chaos Testing
(Litmus)"] --- FitnessService
Chaos --- WalletService
ServiceMesh["Service Mesh
(Istio/Linkerd, mTLS, Policy, Retry)"] --- UserService
ServiceMesh --- WalletService
ServiceMesh --- EcommerceService
ServiceMesh --- FitnessService
ServiceMesh --- MiningService

```
