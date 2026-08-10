# CodeSkeleton_E1-US4

- **Linked Spec ID:** TechnicalSpec_E1-US4

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

import com.client.project.dto.IngestionStatusResponse;
import com.client.project.service.DataIngestionService;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/data-ingestion")
public class DataIngestionController {

    private final DataIngestionService dataIngestionService;

    public DataIngestionController(DataIngestionService dataIngestionService) {
        this.dataIngestionService = dataIngestionService;
    }

    @GetMapping("/status")
    public ResponseEntity<IngestionStatusResponse> getIngestionStatus() {
        return ResponseEntity.ok(dataIngestionService.getIngestionStatus());
    }
}
```

## Service Interface
```java
package com.client.project.service;

import com.client.project.dto.IngestionStatusResponse;

public interface DataIngestionService {
    IngestionStatusResponse getIngestionStatus();
}
```

## Service Implementation
```java
package com.client.project.service.impl;

import com.client.project.dto.IngestionStatusResponse;
import com.client.project.repository.IngestionStatusRepository;
import com.client.project.service.DataIngestionService;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Service;

@Service
public class DataIngestionServiceImpl implements DataIngestionService {

    private static final Logger logger = LoggerFactory.getLogger(DataIngestionServiceImpl.class);
    private final IngestionStatusRepository ingestionStatusRepository;

    public DataIngestionServiceImpl(IngestionStatusRepository ingestionStatusRepository) {
        this.ingestionStatusRepository = ingestionStatusRepository;
    }

    @Override
    public IngestionStatusResponse getIngestionStatus() {
        logger.info("Retrieving ingestion status");
        var statuses = ingestionStatusRepository.findAll();
        // TODO: Convert ingestion statuses into response format
        logger.info("Retrieved ingestion status records: {}", statuses.size());
        return IngestionStatusResponse.builder().statuses(statuses).build();
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
public class IngestionStatusResponse {
    List<IngestionStatusDto> statuses;
}

@Value
@Builder
public class IngestionStatusDto {
    String sourceSystem;
    String ingestionStatus;
    String lastUpdated;
    String recordsLoaded;
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
public class IngestionStatus {
    @Id
    private String statusId;
    private String sourceSystem;
    private String ingestionStatus;
    private String lastUpdated;
    private Long recordsLoaded;
}
```

## Repository Layer
```java
package com.client.project.repository;

import com.client.project.entity.IngestionStatus;
import org.springframework.data.jpa.repository.JpaRepository;

public interface IngestionStatusRepository extends JpaRepository<IngestionStatus, String> {
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
public class DataIngestionProducer {

    private static final Logger logger = LoggerFactory.getLogger(DataIngestionProducer.class);
    private final KafkaTemplate<String, String> kafkaTemplate;

    public DataIngestionProducer(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendIngestionEvent(String topic, String payload) {
        logger.info("Sending ingestion event to topic {}", topic);
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
public class DataIngestionConsumer {

    private static final Logger logger = LoggerFactory.getLogger(DataIngestionConsumer.class);

    @KafkaListener(topics = "source-data-events", groupId = "data-ingestion-group")
    public void consume(String message) {
        logger.info("Received source data event: {}", message);
        // TODO: Process source data ingestion events and persist to target database
    }
}
```

## Configuration Classes
```java
package com.client.project.config;

import org.springframework.context.annotation.Configuration;

@Configuration
public class DataIngestionConfig {
    // TODO: Add API and Kafka ingestion configuration
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
- `logger.info("Retrieving ingestion status");`
- `logger.info("Retrieved ingestion status records: {}", statuses.size());`
- `logger.info("Sending ingestion event to topic {}", topic);`
- `logger.info("Received source data event: {}", message);`
- `logger.error("Unhandled exception", ex);`

## Unit Tests
```java
package com.client.project.test;

import com.client.project.repository.IngestionStatusRepository;
import com.client.project.service.DataIngestionService;
import com.client.project.service.impl.DataIngestionServiceImpl;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import static org.assertj.core.api.Assertions.assertThat;

class DataIngestionServiceTest {

    @Test
    void shouldReturnIngestionStatus() {
        var repository = Mockito.mock(IngestionStatusRepository.class);
        Mockito.when(repository.findAll()).thenReturn(java.util.Collections.emptyList());

        DataIngestionService service = new DataIngestionServiceImpl(repository);
        var response = service.getIngestionStatus();

        assertThat(response).isNotNull();
        assertThat(response.getStatuses()).isEmpty();
    }
}
```

## TODO markers
- TODO: Convert ingestion statuses into response format
- TODO: Process source data ingestion events and persist to target database
- TODO: Add API and Kafka ingestion configuration
