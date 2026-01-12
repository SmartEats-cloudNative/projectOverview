# projectOverview
SmartEats-CloudNative Food Delivery Platform

SmartEats is a production style microservices platform that shows how a modern food elivery system is designed, deployed, and operated on AWS using Kubernetes and GitOps.

It demonstrates the complete lifecycle:
**source code -> quality checks -> containers -> automated deployment -> live traffic on AWS.**
Built using **Spring Boot, Angular, AWS EKS, RDS (MySQL), MongoDB Atlas, Jenkins, SonarQube, Docker, and Argo CD.**

**##🌐 What SmartEats Does**

SmartEats allows users to:
Browse restaurants
View food menus
Add items to cart
Place orders
Persist orders in cloud databases

Behind the scenes, this flow travels through **multiple independent microservices** deployed on **AWS EKS** and exposed through **an AWS Application Load Balancer**.
