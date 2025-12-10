# PhaseZero_Catalog_Service
A Spring Boot microservice for managing a product catalogue via REST APIs. This assignment demonstrates backend fundamentals including API design, data modeling, validation, and clean code architecture.

📋 Overview
This project implements a Product Catalogue Service with REST APIs for:

✅ Adding products

✅ Listing all products

✅ Searching products by name (case-insensitive)

✅ Filtering products by category

✅ Sorting products by price

✅ Calculating total inventory value

🛠️ Tech Stack
Java: 17 (or 8+)

Framework: Spring Boot 2.7.14

Build Tool: Maven 3.6+

Data Storage: In-memory Map (ConcurrentHashMap)

HTTP: REST APIs with JSON

📦 Prerequisites
Java 17 or higher

Maven 3.6 or higher

Git (for cloning)

Postman or cURL (for testing)

Verify installations:

bash
java -version
mvn -version
🚀 Getting Started
Step 1: Clone the Repository
bash
git clone https://github.com/yourusername/phasezero-catalog-service.git
cd phasezero-catalog-service
Step 2: Build the Project
bash
mvn clean install
Step 3: Run the Application
bash
mvn spring-boot:run
Expected Output:

text
✓ PHASEZERO Catalog Service Started Successfully!
✓ Available at: http://localhost:8080
✓ Endpoints: GET /products, POST /products, etc.
Step 4: Verify Running
bash
curl http://localhost:8080/products
Should return [] (empty array) initially.

📡 API Endpoints
1. Add a Product
Endpoint: POST /products
Status Code: 201 Created

Request Body:

json
{
  "partNumber": "PART001",
  "partName": "Hydraulic Filter",
  "category": "Filters",
  "price": 150.50,
  "stock": 100,
  "brand": "TechBrand"
}
Response:

json
{
  "id": 1,
  "partNumber": "PART001",
  "partName": "hydraulic filter",
  "category": "Filters",
