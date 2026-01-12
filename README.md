# projectOverview
SmartEats-CloudNative Food Delivery Platform

SmartEats is a production style microservices platform that shows how a modern food elivery system is designed, deployed, and operated on AWS using Kubernetes and GitOps.

It demonstrates the complete lifecycle:
**source code -> quality checks -> containers -> automated deployment -> live traffic on AWS.**
Built using **Spring Boot, Angular, AWS EKS, RDS (MySQL), MongoDB Atlas, Jenkins, SonarQube, Docker, and Argo CD.**

**#🌐 What SmartEats Does**

SmartEats allows users to:
Browse restaurants,
View food menus,
Add items to cart,
Place orders and 
Persist orders in cloud databases.

Behind the scenes, this flow travels through **multiple independent microservices** deployed on **AWS EKS** and exposed through **an AWS Application Load Balancer**.

# 🔁 Platform Flow (from code to cloud)
Developer → GitHub → Jenkins → Docker Images → GitOps Repo → Argo CD → EKS → Users

# 🏗️ Platform Architecture

```text
            ┌──────────────────────────────┐
            │          Jenkins CI/CD       │
            │  (GitHub → Build → Docker)   │
            └───────────────┬──────────────┘
                            │
                            ▼
                  ┌──────────────────┐
                  │   Docker Hub     │
                  │ (Image Registry) │
                  └─────────┬────────┘
                            │
                            ▼
        ┌────────────────────────────────────────┐
        │                Argo CD                 │
        │ (GitOps: watches manifests & syncs)    │
        └───────────────────┬────────────────────┘
                            │
                            ▼
              ┌─────────────────────────┐
              │        AWS EKS          │
              │  Kubernetes (Multi-AZ)  │
              └───────────┬─────────────┘
                          │
        ┌─────────────────┼──────────────────┐
        ▼                 ▼                  ▼
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│ Angular UI        │  │ Spring Boot MS   │  │ Eureka Server    │
│ (Nginx : 80)      │  │ (9001 – 9004)    │  │ (8761)           │
└─────────┬─────────┘  └─────────┬────────┘  └─────────┬────────┘
          │                      │                      │
          └───────────────┬──────┴───────────────┬──────┘
                          ▼                      ▼
                   ┌─────────────────────────────────┐
                   │      Kubernetes Ingress (ALB)    │
                   │   Routes: /, /user, /order, ...  │
                   └─────────────────┬───────────────┘
                                     ▼
                   ┌─────────────────────────────────┐
                   │   AWS Application Load Balancer  │
                   │           (Internet)             │
                   └─────────────────┬───────────────┘
                                     ▼
                   ┌─────────────────────────────────┐
                   │            Databases             │
                   │   RDS MySQL  +  MongoDB Atlas     │
                   └─────────────────────────────────┘

```

# 🧩 Services in the Platform

| Layer             | Components                              |
| ----------------- | --------------------------------------- |
| Frontend          | Angular UI (served via Nginx)           |
| Service Discovery | Eureka                                  |
| Business Services | User, Restaurant, Food Catalogue, Order |
| Data              | MySQL (RDS) + MongoDB Atlas             |
| Delivery          | AWS EKS + ALB + Ingress                 |
| Automation        | Jenkins + Argo CD                       |

# 📦 GitHub Repositories
All code lives under **SmartEats-cloudNative**:

| Component                     | Repository                                                                                                                 |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| GitOps / Kubernetes Manifests | [https://github.com/SmartEats-cloudNative/deployment-folder](https://github.com/SmartEats-cloudNative/deployment-folder)   |
| Angular Frontend              | [https://github.com/SmartEats-cloudNative/food-deliver-app](https://github.com/SmartEats-cloudNative/food-deliver-app)     |
| Eureka Server                 | [https://github.com/SmartEats-cloudNative/Eureka](https://github.com/SmartEats-cloudNative/Eureka)                         |
| User Service                  | [https://github.com/SmartEats-cloudNative/UserInfo](https://github.com/SmartEats-cloudNative/UserInfo)                     |
| Restaurant Service            | [https://github.com/SmartEats-cloudNative/restaurant-listing](https://github.com/SmartEats-cloudNative/restaurant-listing) |
| Food Catalogue Service        | [https://github.com/SmartEats-cloudNative/FoodCatalogue](https://github.com/SmartEats-cloudNative/FoodCatalogue)           |
| Order Service                 | [https://github.com/SmartEats-cloudNative/OrderInfo](https://github.com/SmartEats-cloudNative/OrderInfo)                   |

# 🌍 Routing & Endpoints
Traffic enters through AWS ALB and is routed by Kubernetes Ingress:

| Path             | Service              | Service Port |
|------------------|----------------------|--------------|
| `/`              | Angular UI           | 80           |
| `/restaurant`    | Restaurant Service   | 9091         |
| `/foodCatalogue` | Food Catalogue       | 9092         |
| `/user`          | User Service         | 9092         |
| `/order`         | Order Service        | 9094         |
| `/eureka`        | Eureka Dashboard     | 8761         |

# 🐳 Docker Images
| Image                                     | Purpose                             |
| ----------------------------------------- | ----------------------------------- |
| `lavanyasimham/food-deliver-app-fe`       | Angular frontend (served via Nginx) |
| `lavanyasimham/eureka-server`             | Eureka service registry             |
| `lavanyasimham/userinfo-service`          | User microservice                   |
| `lavanyasimham/listofrestaurants-service` | Restaurant microservice             |
| `lavanyasimham/foodinfo-service`          | Food catalogue microservice         |
| `lavanyasimham/order-service`             | Order processing microservice       |

# 🛢 Databases

| Database      | Technology      | Purpose                              | Database Name(s)                   |
| ------------- | --------------- | ------------------------------------ | ---------------------------------- |
| Relational DB | AWS RDS (MySQL) | Stores structured application data   | `userdb`, `restaurantdb`, `fooddb` |
| NoSQL DB      | MongoDB Atlas   | Stores order documents and cart data | `orderdb`                          |







