# CodeSkeleton_E1-US2

- **Linked Spec ID:** TechnicalSpec_E1-US2

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

import com.client.project.dto.MappingReportResponse;
import com.client.project.service.FeedMappingService;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/feed-mapping")
public class FeedMappingController {

    private final FeedMappingService feedMappingService;

    public FeedMappingController(FeedMappingService feedMappingService) {
        this.feedMappingService = feedMappingService;
    }

    @GetMapping("/report")
    public ResponseEntity<MappingReportResponse> getMappingReport() {
        return ResponseEntity.ok(feedMappingService.getMappingReport());
    }
}
```

## Service Interface
```java
package com.client.project.service;

import com.client.project.dto.MappingReportResponse;

public interface FeedMappingService {
    MappingReportResponse getMappingReport();
}
```

## Service Implementation
```java
package com.client.project.service.impl;

import com.client.project.dto.MappingReportResponse;
import com.client.project.repository.FieldMappingRepository;
import com.client.project.service.FeedMappingService;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Service;

@Service
public class FeedMappingServiceImpl implements FeedMappingService {

    private static final Logger logger = LoggerFactory.getLogger(FeedMappingServiceImpl.class);
    private final FieldMappingRepository fieldMappingRepository;

    public FeedMappingServiceImpl(FieldMappingRepository fieldMappingRepository) {
        this.fieldMappingRepository = fieldMappingRepository;
    }

    @Override
    public MappingReportResponse getMappingReport() {
        logger.info("Generating source-to-target mapping report");
        var mappings = fieldMappingRepository.findAll();
        // TODO: Build mapping report from repository data
        logger.info("Completed mapping report generation with {} mappings", mappings.size());
        return MappingReportResponse.builder().mappings(mappings).build();
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
public class MappingReportResponse {
    List<FieldMappingDto> mappings;
}

@Value
@Builder
public class FieldMappingDto {
    String sourceField;
    String targetField;
    String transformationRule;
    String status;
    String comments;
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
public class FieldMapping {
    @Id
    private String mappingId;
    private String sourceField;
    private String targetField;
    private String transformationRule;
    private String status;
    private String comments;
}
```

## Repository Layer
```java
package com.client.project.repository;

import com.client.project.entity.FieldMapping;
import org.springframework.data.jpa.repository.JpaRepository;

public interface FieldMappingRepository extends JpaRepository<FieldMapping, String> {
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
public class FieldMappingProducer {

    private static final Logger logger = LoggerFactory.getLogger(FieldMappingProducer.class);
    private final KafkaTemplate<String, String> kafkaTemplate;

    public FieldMappingProducer(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendMappingUpdate(String topic, String payload) {
        logger.info("Sending field mapping update to topic {}", topic);
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
public class FieldMappingConsumer {

    private static final Logger logger = LoggerFactory.getLogger(FieldMappingConsumer.class);

    @KafkaListener(topics = "field-mapping-updates", groupId = "field-mapping-group")
    public void consume(String message) {
        logger.info("Received field mapping update message: {}", message);
        // TODO: process mapping updates if required
    }
}
```

## Configuration Classes
```java
package com.client.project.config;

import org.springframework.context.annotation.Configuration;

@Configuration
public class MappingConfig {
    // TODO: Add mapping-specific configuration if needed
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
- `logger.info("Generating source-to-target mapping report");`
- `logger.info("Completed mapping report generation with {} mappings", mappings.size());`
- `logger.info("Sending field mapping update to topic {}", topic);`
- `logger.info("Received field mapping update message: {}", message);`
- `logger.error("Unhandled exception", ex);`

## Unit Tests
```java
package com.client.project.test;

import com.client.project.repository.FieldMappingRepository;
import com.client.project.service.FeedMappingService;
import com.client.project.service.impl.FeedMappingServiceImpl;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import static org.assertj.core.api.Assertions.assertThat;

class FeedMappingServiceTest {

    @Test
    void shouldReturnMappingReport() {
        var repository = Mockito.mock(FieldMappingRepository.class);
        Mockito.when(repository.findAll()).thenReturn(java.util.Collections.emptyList());

        FeedMappingService service = new FeedMappingServiceImpl(repository);
        var response = service.getMappingReport();

        assertThat(response).isNotNull();
        assertThat(response.getMappings()).isEmpty();
    }
}
```

## TODO markers
- TODO: Build mapping report from repository data
- TODO: process mapping updates if required
- TODO: Add mapping-specific configuration if needed
