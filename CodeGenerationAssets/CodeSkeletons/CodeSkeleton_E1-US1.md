# CodeSkeleton_E1-US1

- **Linked Spec ID:** TechnicalSpec_E1-US1

## Project Structure
```
com.client.project
├── config
├── controller
├── dto
├── entity
├── exception
├── kafka.consumer
├── kafka.producer
├── mapper
├── repository
├── service
├── service.impl
└── test
```

## Controller
```java
package com.client.project.controller;

import com.client.project.service.FeedInventoryService;
import com.client.project.dto.FeedInventoryResponse;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/feed-inventory")
public class FeedInventoryController {

    private final FeedInventoryService feedInventoryService;

    public FeedInventoryController(FeedInventoryService feedInventoryService) {
        this.feedInventoryService = feedInventoryService;
    }

    @GetMapping
    public ResponseEntity<FeedInventoryResponse> getFeedInventory() {
        return ResponseEntity.ok(feedInventoryService.getFeedInventory());
    }
}
```

## Service Interface
```java
package com.client.project.service;

import com.client.project.dto.FeedInventoryResponse;

public interface FeedInventoryService {
    FeedInventoryResponse getFeedInventory();
}
```

## Service Implementation
```java
package com.client.project.service.impl;

import com.client.project.dto.FeedInventoryResponse;
import com.client.project.repository.FeedMetadataRepository;
import com.client.project.service.FeedInventoryService;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Service;

@Service
public class FeedInventoryServiceImpl implements FeedInventoryService {

    private static final Logger logger = LoggerFactory.getLogger(FeedInventoryServiceImpl.class);
    private final FeedMetadataRepository feedMetadataRepository;

    public FeedInventoryServiceImpl(FeedMetadataRepository feedMetadataRepository) {
        this.feedMetadataRepository = feedMetadataRepository;
    }

    @Override
    public FeedInventoryResponse getFeedInventory() {
        logger.info("Starting feed inventory generation");
        var feeds = feedMetadataRepository.findAll();
        // TODO: Transform metadata into response structure
        logger.info("Completed feed inventory generation with {} feeds", feeds.size());
        return FeedInventoryResponse.builder().feedMetadata(feeds).build();
    }
}
```

## DTO Classes
```java
package com.client.project.dto;

import lombok.Builder;
import lombok.Value;
import java.util.List;

@Value
@Builder
public class FeedInventoryResponse {
    List<FeedMetadataDto> feedMetadata;
}

@Value
@Builder
public class FeedMetadataDto {
    String feedName;
    String layout;
    String schedule;
    String transmissionMechanism;
    String downstreamConsumer;
    String dependencies;
}
```

## Entity Classes
```java
package com.client.project.entity;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import lombok.Data;

@Entity
@Data
public class FeedMetadata {
    @Id
    private String feedId;
    private String feedName;
    private String layout;
    private String schedule;
    private String transmissionMechanism;
    private String downstreamConsumer;
    private String dependencies;
}
```

## Repository Layer
```java
package com.client.project.repository;

import com.client.project.entity.FeedMetadata;
import org.springframework.data.jpa.repository.JpaRepository;

public interface FeedMetadataRepository extends JpaRepository<FeedMetadata, String> {
}
```

## Kafka Producer
```java
package com.client.project.kafka.producer;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.kafka.core.KafkaTemplate;
import org.springframework.stereotype.Component;

@Component
public class FeedInventoryProducer {

    private static final Logger logger = LoggerFactory.getLogger(FeedInventoryProducer.class);
    private final KafkaTemplate<String, String> kafkaTemplate;

    public FeedInventoryProducer(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendInventoryUpdate(String topic, String payload) {
        logger.info("Sending feed inventory update to topic {}", topic);
        kafkaTemplate.send(topic, payload);
    }
}
```

## Kafka Consumer
```java
package com.client.project.kafka.consumer;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.kafka.annotation.KafkaListener;
import org.springframework.stereotype.Component;

@Component
public class FeedInventoryConsumer {

    private static final Logger logger = LoggerFactory.getLogger(FeedInventoryConsumer.class);

    @KafkaListener(topics = "feed-inventory-updates", groupId = "feed-inventory-group")
    public void consume(String message) {
        logger.info("Received feed inventory update message: {}", message);
        // TODO: process feed inventory messages if required
    }
}
```

## Configuration Classes
```java
package com.client.project.config;

import org.springframework.context.annotation.Configuration;

@Configuration
public class KafkaConfig {
    // TODO: Add Kafka producer and consumer configuration
}
```

## Exception Handler
```java
package com.client.project.exception;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@ControllerAdvice
public class GlobalExceptionHandler {

    private static final Logger logger = LoggerFactory.getLogger(GlobalExceptionHandler.class);

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleException(Exception ex) {
        logger.error("Unhandled exception", ex);
        return ResponseEntity.status(500).body("Internal server error");
    }
}
```

## Logging Statements
- `logger.info("Starting feed inventory generation");`
- `logger.info("Completed feed inventory generation with {} feeds", feeds.size());`
- `logger.error("Unhandled exception", ex);`
- `logger.info("Sending feed inventory update to topic {}", topic);`
- `logger.info("Received feed inventory update message: {}", message);`

## Unit Tests
```java
package com.client.project.test;

import com.client.project.repository.FeedMetadataRepository;
import com.client.project.service.FeedInventoryService;
import com.client.project.service.impl.FeedInventoryServiceImpl;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import static org.assertj.core.api.Assertions.assertThat;

class FeedInventoryServiceTest {

    @Test
    void shouldReturnFeedInventory() {
        var repository = Mockito.mock(FeedMetadataRepository.class);
        Mockito.when(repository.findAll()).thenReturn(java.util.Collections.emptyList());

        FeedInventoryService service = new FeedInventoryServiceImpl(repository);
        var response = service.getFeedInventory();

        assertThat(response).isNotNull();
        assertThat(response.getFeedMetadata()).isEmpty();
    }
}
```

## TODO markers
- TODO: Transform metadata into response structure
- TODO: Add Kafka producer and consumer configuration
- TODO: process feed inventory messages if required
