# projectOverview
SmartEats is a **production-style, cloud-native microservices platform** that demonstrates how a modern food delivery system is **designed, built, deployed, and operated on AWS** using Kubernetes and GitOps practices.

The platform showcases an **end-to-end enterprise application lifecycle**  from source code and quality checks to containerization, automated deployments, and live traffic handling on AWS. It is built using **Spring Boot microservices, Angular, AWS EKS, RDS (MySQL), MongoDB Atlas, Jenkins, SonarQube, Docker, and Argo CD**, following industry-standard engineering and delivery practices.

SmartEats emphasizes **clean architecture, separation of concerns, scalability, and automation**, reflecting patterns commonly used in large-scale enterprise web applications.

**#🌐 What SmartEats Does**

SmartEats allows users to:
Browse restaurants,
View food menus,
Add items to cart,
Place orders and 
Persist orders in cloud databases.

Behind the scenes, this flow travels through **multiple independent microservices** deployed on **AWS EKS** and exposed through **an AWS Application Load Balancer**, and managed through a fully automated CI/CD and GitOps pipeline.

# 🔁 Platform Flow (from code to cloud)
Developer → GitHub → Jenkins → Docker Images → GitOps Repo → Argo CD → EKS → Users

# 🏗️ Platform Architecture

🧱 Application Architecture (Backend)

Each Spring Boot microservice follows a layered MVC-style architecture with clear
separation of concerns:
- **Controller layer**: REST API endpoints
- **Service layer**: business logic and orchestration
- **Data layer**: JPA repositories / database access

This mirrors common enterprise web application patterns used across Java and
Microsoft Web API ecosystems.


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
│ (Nginx : 80)      │  │ (9091 – 9094)    │  │ (8761)           │
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

# ⚙️Runtime Settings (Kubernetes)

**MySQL-based services**
(User, Restaurant, Food Catalogue)

```bash
SPRING_DATASOURCE_URL=jdbc:mysql://<RDS_ENDPOINT>:3306/?useSSL=false&serverTimezone=UTC
SPRING_DATASOURCE_USERNAME=<db_user>
SPRING_DATASOURCE_PASSWORD=<db_password>
```
**MongoDB-based service**
(Order Service)
```bash
MONGODB_URI=mongodb+srv://<user>:<password>@cluster.mongodb.net/orderdb
# or
MONGODB_URI=mongodb://<user>:<password>@host:27017/orderdb
```
**Eureka** (for all microservices)
```bash
EUREKA_CLIENT_SERVICEURL_DEFAULTZONE=http://eureka-service:8761/eureka
```

Whenever database endpoints or credentials change (for example after recreating RDS), update the Kubernetes ConfigMaps and Secrets and restart the affected services:
```bash
kubectl rollout restart deployment <service-name>
```

# 🚀 Continuous Delivery Pipeline

```bash
GitHub → Jenkins → Docker → GitOps → Argo CD → AWS EKS
```
- Any push to a service repository triggers **Jenkins** via a GitHub webhook  
- Jenkins builds the code and executes automated tests  
- **SonarQube** evaluates the code against quality gates  
- A **Docker image** is built and pushed to Docker Hub  
- Jenkins updates the image tag inside the **GitOps repository**  
- **Argo CD** detects the change and automatically deploys it to **AWS EKS**

Deployment status can be verified using:

```bash
kubectl get pods
kubectl get svc
kubectl get ingress
```
and by accessing the AWS ALB URL when the cluster is running.

# 🧪 Build Locally

Spring Boot Services
```bash
mvn clean package -DskipTests
java -jar target/<app>.jar
```

Angular Frontend
```bash
npm ci
npm run build
```

Optional Docker build:
```bash
docker build -t lavanyasimham/food-deliver-app-fe:local
```

## ☁️ Bring the Platform Online (7-Step Checklist)

**1.Create or verify the EKS cluster and node group**
   ```bash
   eksctl get cluster
   kubectl get nodes
   ```
**2.Create AWS RDS (MySQL) and import databases**
       - userdb
       - restaurantdb
       - fooddb
       
**3.Ensure MongoDB Atlas orderdb is available and EKS/NAT IPs are allowed**

**4.Install AWS ALB Ingress Controller using IAM + IRSA**
       - Configure IAM policy  
       - Enable **IRSA** (IAM Roles for Service Accounts)  
       - Set **IngressClass** to `alb`
       
**5.Configure Kubernetes Secrets and ConfigMaps**
      - RDS endpoint  
      - MongoDB URI  
      - Eureka URL 
      
**6.Verify GitOps manifests in** deployment-folder/aws
      - Image tags  
      - Service ports  
      - Ingress paths
      
**7.Open Argo CD, sync the application, and verify the full UI flow**

    (Restaurant → Menu → Order)





