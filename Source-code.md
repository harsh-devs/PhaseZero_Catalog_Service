1. pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>4.0.0</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>phasezero-catalog-service</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>phasezero-catalog-service</name>
	<description>Demo project for Spring Boot</description>
	<url/>
	<licenses>
		<license/>
	</licenses>
	<developers>
		<developer/>
	</developers>
	<scm>
		<connection/>
		<developerConnection/>
		<tag/>
		<url/>
	</scm>
	<properties>
		<java.version>17</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-validation</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-webmvc</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>com.mysql</groupId>
			<artifactId>mysql-connector-j</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-validation-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-webmvc-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
=======================================================================================================================
2.Application Properties
spring.application.name=phasezero-catalog-service

# Server configuration (optional overrides)
server.port=8080
server.servlet.context-path=/phasezero-catalog-service

# ================================
# DataSource (MySQL)
# ================================
spring.datasource.url=jdbc:mysql://localhost:3306/phasezero_catalog_service?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# ================================
# JPA / Hibernate
# ================================
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect

# Optional: SQL formatting & logging
spring.jpa.properties.hibernate.format_sql=true
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=TRACE
==============================================================================================================================
3.PhasezeroCatalogServiceApplication
package com.example.phasezero_catalog_service;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.jdbc.autoconfigure.DataSourceAutoConfiguration;

@SpringBootApplication
public class PhasezeroCatalogServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(PhasezeroCatalogServiceApplication.class, args);
		
	}
==============================================================================================================================
4. Product.java
package com.example.phasezero_catalog_service.model;

import java.time.LocalDateTime;

public class Product {
	private Long id;
    private String partNumber;
    private String partName;
    private String category;
    private double price;
    private int stock;
    private String brand;
    private LocalDateTime createdAt;

    public Product() {
        this.createdAt = LocalDateTime.now();
    }

    public Product(Long id, String partNumber, String partName, String category, 
                   double price, int stock, String brand) {
        this.id = id;
        this.partNumber = partNumber;
        this.partName = partName != null ? partName.toLowerCase() : null;
        this.category = category;
        this.price = price;
        this.stock = stock;
        this.brand = brand;
        this.createdAt = LocalDateTime.now();
    }

    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }

    public String getPartNumber() { return partNumber; }
    public void setPartNumber(String partNumber) { this.partNumber = partNumber; }

    public String getPartName() { return partName; }
    public void setPartName(String partName) { 
        this.partName = partName != null ? partName.toLowerCase() : null; 
    }

    public String getCategory() { return category; }
    public void setCategory(String category) { this.category = category; }

    public double getPrice() { return price; }
    public void setPrice(double price) { this.price = price; }

    public int getStock() { return stock; }
    public void setStock(int stock) { this.stock = stock; }

    public String getBrand() { return brand; }
    public void setBrand(String brand) { this.brand = brand; }

    public LocalDateTime getCreatedAt() { return createdAt; }
    public void setCreatedAt(LocalDateTime createdAt) { this.createdAt = createdAt; }

    @Override
    public String toString() {
        return "Product{" + "id=" + id + ", partNumber='" + partNumber + '\'' +
                ", partName='" + partName + '\'' + ", category='" + category + '\'' +
                ", price=" + price + ", stock=" + stock + ", brand='" + brand + 
                '\'' + ", createdAt=" + createdAt + '}';
}
}

}
==============================================================================================================================
5.ProductController.java
package com.example.phasezero_catalog_service.controller;


import com.example.phasezero_catalog_service.service.ProductService;
import com.example.phasezero_catalog_service.dto.ProductRequest;
import com.example.phasezero_catalog_service.model.Product;
import com.example.phasezero_catalog_service.dto.InventoryValueResponse;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/products")
public class ProductController {

    @Autowired
    private ProductService productService;

    // POST: Add a new product
    @PostMapping
    public ResponseEntity<Product> addProduct(@RequestBody ProductRequest request) {
        Product product = productService.addProduct(request);
        return ResponseEntity.status(HttpStatus.CREATED).body(product);
    }

    // GET: List all products
    @GetMapping
    public ResponseEntity<List<Product>> getAllProducts() {
        List<Product> products = productService.getAllProducts();
        return ResponseEntity.ok(products);
    }

    // GET: Search products by name
    @GetMapping("/search")
    public ResponseEntity<List<Product>> searchProducts(@RequestParam String query) {
        List<Product> products = productService.searchByName(query);
        return ResponseEntity.ok(products);
    }

    // GET: Filter products by category
    @GetMapping("/filter")
    public ResponseEntity<List<Product>> filterByCategory(@RequestParam String category) {
        List<Product> products = productService.filterByCategory(category);
        return ResponseEntity.ok(products);
    }

    // GET: Sort products by price
    @GetMapping("/sorted")
    public ResponseEntity<List<Product>> sortByPrice() {
        List<Product> products = productService.sortByPrice();
        return ResponseEntity.ok(products);
    }

    // GET: Calculate inventory value
    @GetMapping("/inventory/value")
    public ResponseEntity<InventoryValueResponse> getInventoryValue() {
        double value = productService.calculateInventoryValue();
        return ResponseEntity.ok(new InventoryValueResponse(value));
    }
}
==============================================================================================================================
6.ErrorResponse.java
package com.example.phasezero_catalog_service.dto;

import java.time.LocalDateTime;

public class ErrorResponse {
	private String code;
    private String message;
    private LocalDateTime timestamp;

    public ErrorResponse(String code, String message) {
        this.code = code;
        this.message = message;
        this.timestamp = LocalDateTime.now();
    }

    public String getCode() { return code; }
    public void setCode(String code) { this.code = code; }

    public String getMessage() { return message; }
    public void setMessage(String message) { this.message = message; }

    public LocalDateTime getTimestamp() { return timestamp; }
    public void setTimestamp(LocalDateTime timestamp) { this.timestamp = timestamp; }
}
==============================================================================================================================
7. InventoryValueResponse.java
package com.example.phasezero_catalog_service.dto;

public class InventoryValueResponse {
	 private double totalInventoryValue;

	    public InventoryValueResponse(double totalInventoryValue) {
	        this.totalInventoryValue = totalInventoryValue;
	    }

	    public double getTotalInventoryValue() { return totalInventoryValue; }
	    public void setTotalInventoryValue(double totalInventoryValue) { 
	        this.totalInventoryValue = totalInventoryValue; 
	    }
}
==============================================================================================================================
8.ProductRequest.java
package com.example.phasezero_catalog_service.dto;

public class ProductRequest {
	private String partNumber;
    private String partName;
    private String category;
    private double price;
    private int stock;
    private String brand;

    // Constructors
    public ProductRequest() {}

    public ProductRequest(String partNumber, String partName, String category, 
                         double price, int stock, String brand) {
        this.partNumber = partNumber;
        this.partName = partName;
        this.category = category;
        this.price = price;
        this.stock = stock;
        this.brand = brand;
    }

    // Getters and Setters
    public String getPartNumber() { return partNumber; }
    public void setPartNumber(String partNumber) { this.partNumber = partNumber; }

    public String getPartName() { return partName; }
    public void setPartName(String partName) { this.partName = partName; }

    public String getCategory() { return category; }
    public void setCategory(String category) { this.category = category; }

    public double getPrice() { return price; }
    public void setPrice(double price) { this.price = price; }

    public int getStock() { return stock; }
    public void setStock(int stock) { this.stock = stock; }

    public String getBrand() { return brand; }
    public void setBrand(String brand) { this.brand = brand; }

    @Override
    public String toString() {
        return "ProductRequest{" + "partNumber='" + partNumber + '\'' +
                ", partName='" + partName + '\'' + ", category='" + category + 
                '\'' + ", price=" + price + ", stock=" + stock + ", brand='" + 
                brand + '\'' + '}';
    }
}
==============================================================================================================================
9.DuplicateProductException.java
package com.example.phasezero_catalog_service.exception;

public class DuplicateProductException extends ProductException {
    public DuplicateProductException(String message) {
        super(message);
    }

}
==============================================================================================================================
10.GlobalExceptionHandler.java
package com.example.phasezero_catalog_service.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestController;

import com.example.phasezero_catalog_service.dto.ErrorResponse;

@RestController
public class GlobalExceptionHandler {
	@ExceptionHandler(DuplicateProductException.class)
    public ResponseEntity<ErrorResponse> handleDuplicateProduct(DuplicateProductException ex) {
        ErrorResponse error = new ErrorResponse("CONFLICT", ex.getMessage());
        return ResponseEntity.status(HttpStatus.CONFLICT).body(error);
    }

    @ExceptionHandler(InvalidProductException.class)
    public ResponseEntity<ErrorResponse> handleInvalidProduct(InvalidProductException ex) {
        ErrorResponse error = new ErrorResponse("BAD_REQUEST", ex.getMessage());
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGenericException(Exception ex) {
        ErrorResponse error = new ErrorResponse("INTERNAL_SERVER_ERROR", 
            "An unexpected error occurred: " + ex.getMessage());
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
}
==============================================================================================================================
11.InvalidProductException.java
package com.example.phasezero_catalog_service.exception;

public class InvalidProductException extends ProductException {
    public InvalidProductException(String message) {
        super(message);

}
}
==============================================================================================================================
12.ProductException.java
package com.example.phasezero_catalog_service.exception;

public class ProductException extends RuntimeException {
    public ProductException(String message) {
        super(message);
    }
}
==============================================================================================================================
13.ProductRepository.java
package com.example.phasezero_catalog_service.repository;

import com.example.phasezero_catalog_service.model.*;
import org.springframework.stereotype.Repository;
import java.util.*;
import java.util.concurrent.ConcurrentHashMap;

@Repository
public class ProductRepository {
    
    private final Map<String, Product> products = new ConcurrentHashMap<>();
    private Long nextId = 1L;

    // Create/Add Product
    public Product save(Product product) {
        if (product.getId() == null) {
            product.setId(nextId++);
        }
        products.put(product.getPartNumber(), product);
        return product;
    }

    // Get all products
    public List<Product> findAll() {
        return new ArrayList<>(products.values());
    }

    // Find by partNumber
    public Product findByPartNumber(String partNumber) {
        return products.get(partNumber);
    }

    // Check if product exists
    public boolean existsByPartNumber(String partNumber) {
        return products.containsKey(partNumber);
    }

    // Get count
    public long count() {
        return products.size();
    }

    // Clear all
    public void deleteAll() {
        products.clear();
    }
}
==============================================================================================================================
14.ProductService.java
package com.example.phasezero_catalog_service.service;

import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.example.phasezero_catalog_service.dto.ProductRequest;
import com.example.phasezero_catalog_service.exception.DuplicateProductException;
import com.example.phasezero_catalog_service.exception.InvalidProductException;
import com.example.phasezero_catalog_service.model.Product;
import com.example.phasezero_catalog_service.repository.ProductRepository;

@Service
public class ProductService {

	@Autowired
    private ProductRepository productRepository;

    // Add new product
    public Product addProduct(ProductRequest request) {
        // Validation
        validateProductRequest(request);

        // Check for duplicate
        if (productRepository.existsByPartNumber(request.getPartNumber())) {
            throw new DuplicateProductException(
                "Product with partNumber '" + request.getPartNumber() + "' already exists"
            );
        }

        // Create and save product
        Product product = new Product(
            null,
            request.getPartNumber(),
            request.getPartName(),
            request.getCategory(),
            request.getPrice(),
            request.getStock(),
            request.getBrand()
        );

        return productRepository.save(product);
    }

    // Get all products
    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }

    // Search products by name
    public List<Product> searchByName(String query) {
        if (query == null || query.trim().isEmpty()) {
            return productRepository.findAll();
        }

        String searchQuery = query.toLowerCase();
        return productRepository.findAll().stream()
            .filter(p -> p.getPartName().contains(searchQuery))
            .collect(Collectors.toList());
    }

    // Filter products by category
    public List<Product> filterByCategory(String category) {
        if (category == null || category.trim().isEmpty()) {
            return productRepository.findAll();
        }

        return productRepository.findAll().stream()
            .filter(p -> p.getCategory().equalsIgnoreCase(category))
            .collect(Collectors.toList());
    }

    // Sort products by price (ascending)
    public List<Product> sortByPrice() {
        return productRepository.findAll().stream()
            .sorted(Comparator.comparingDouble(Product::getPrice))
            .collect(Collectors.toList());
    }

    // Calculate total inventory value
    public double calculateInventoryValue() {
        return productRepository.findAll().stream()
            .mapToDouble(p -> p.getPrice() * p.getStock())
            .sum();
    }

    // Validation
    private void validateProductRequest(ProductRequest request) {
        if (request.getPartNumber() == null || request.getPartNumber().trim().isEmpty()) {
            throw new InvalidProductException("partNumber is required");
        }
        if (request.getPartName() == null || request.getPartName().trim().isEmpty()) {
            throw new InvalidProductException("partName is required");
        }
        if (request.getCategory() == null || request.getCategory().trim().isEmpty()) {
            throw new InvalidProductException("category is required");
        }
        if (request.getPrice() < 0) {
            throw new InvalidProductException("price cannot be negative");
        }
        if (request.getStock() < 0) {
            throw new InvalidProductException("stock cannot be negative");
        }
    }
}
==============================================================================================================================
Project structure
phasezero-catalog-service/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/phasezero-catalog-service/
│   │   │       ├── controller/
│   │   │       │   └── ProductController.java
│   │   │       ├── service/
│   │   │       │   └── ProductService.java
│   │   │       ├── repository/
│   │   │       │   └── ProductRepository.java
│   │   │       ├── model/
│   │   │       │   └── Product.java
│   │   │       ├── dto/
│   │   │       │   ├── ProductRequest.java
│   │   │       │   ├── ErrorResponse.java
│   │   │       │   └── InventoryValueResponse.java
│   │   │       ├── exception/
│   │   │       │   ├── ProductException.java
│   │   │       │   ├── DuplicateProductException.java
│   │   │       │   ├── InvalidProductException.java
│   │   │       │   └── GlobalExceptionHandler.java
│   │   │       └── CatalogServiceApplication.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       └── java/
├── pom.xml
└── README.md
