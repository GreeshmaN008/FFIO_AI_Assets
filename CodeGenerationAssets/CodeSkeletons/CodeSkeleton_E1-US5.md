# CodeSkeleton_E1-US5

- **Linked Spec ID:** TechnicalSpec_E1-US5

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

import com.client.project.dto.FeedGenerationStatusResponse;
import com.client.project.service.FeedGenerationService;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/feed-generation")
public class FeedGenerationController {

    private final FeedGenerationService feedGenerationService;

    public FeedGenerationController(FeedGenerationService feedGenerationService) {
        this.feedGenerationService = feedGenerationService;
    }

    @GetMapping("/status")
    public ResponseEntity<FeedGenerationStatusResponse> getFeedGenerationStatus() {
        return ResponseEntity.ok(feedGenerationService.getFeedGenerationStatus());
    }
}
```

## Service Interface
```java
package com.client.project.service;

import com.client.project.dto.FeedGenerationStatusResponse;

public interface FeedGenerationService {
    FeedGenerationStatusResponse getFeedGenerationStatus();
}
```

## Service Implementation
```java
package com.client.project.service.impl;

import com.client.project.dto.FeedGenerationStatusResponse;
import com.client.project.repository.FeedGenerationStatusRepository;
import com.client.project.service.FeedGenerationService;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Service;

@Service
public class FeedGenerationServiceImpl implements FeedGenerationService {

    private static final Logger logger = LoggerFactory.getLogger(FeedGenerationServiceImpl.class);
    private final FeedGenerationStatusRepository feedGenerationStatusRepository;

    public FeedGenerationServiceImpl(FeedGenerationStatusRepository feedGenerationStatusRepository) {
        this.feedGenerationStatusRepository = feedGenerationStatusRepository;
    }

    @Override
    public FeedGenerationStatusResponse getFeedGenerationStatus() {
        logger.info("Retrieving feed generation status");
        var statuses = feedGenerationStatusRepository.findAll();
        // TODO: Convert feed generation records into response
        logger.info("Retrieved feed generation status records: {}", statuses.size());
        return FeedGenerationStatusResponse.builder().statuses(statuses).build();
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
public class FeedGenerationStatusResponse {
    List<FeedGenerationStatusDto> statuses;
}

@Value
@Builder
public class FeedGenerationStatusDto {
    String feedName;
    String schedule;
    String outputFormat;
    String transmissionEndpoint;
    String generationStatus;
    String lastRunTime;
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
public class FeedGenerationStatus {
    @Id
    private String statusId;
    private String feedName;
    private String schedule;
    private String outputFormat;
    private String transmissionEndpoint;
    private String generationStatus;
    private String lastRunTime;
}
```

## Repository Layer
```java
package com.client.project.repository;

import com.client.project.entity.FeedGenerationStatus;
import org.springframework.data.jpa.repository.JpaRepository;

public interface FeedGenerationStatusRepository extends JpaRepository<FeedGenerationStatus, String> {
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
public class FeedGenerationProducer {

    private static final Logger logger = LoggerFactory.getLogger(FeedGenerationProducer.class);
    private final KafkaTemplate<String, String> kafkaTemplate;

    public FeedGenerationProducer(KafkaTemplate<String, String> kafkaTemplate) {
        this.kafkaTemplate = kafkaTemplate;
    }

    public void sendFeedGenerationEvent(String topic, String payload) {
        logger.info("Sending feed generation event to topic {}", topic);
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
public class FeedGenerationConsumer {

    private static final Logger logger = LoggerFactory.getLogger(FeedGenerationConsumer.class);

    @KafkaListener(topics = "feed-generation-events", groupId = "feed-generation-group")
    public void consume(String message) {
        logger.info("Received feed generation event: {}", message);
        // TODO: Trigger feed generation logic or update status from event
    }
}
```

## Configuration Classes
```java
package com.client.project.config;

import org.springframework.context.annotation.Configuration;

@Configuration
public class FeedGenerationConfig {
    // TODO: Add feed generation and scheduling configuration
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
- `logger.info("Retrieving feed generation status");`
- `logger.info("Retrieved feed generation status records: {}", statuses.size());`
- `logger.info("Sending feed generation event to topic {}", topic);`
- `logger.info("Received feed generation event: {}", message);`
- `logger.error("Unhandled exception", ex);`

## Unit Tests
```java
package com.client.project.test;

import com.client.project.repository.FeedGenerationStatusRepository;
import com.client.project.service.FeedGenerationService;
import com.client.project.service.impl.FeedGenerationServiceImpl;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import static org.assertj.core.api.Assertions.assertThat;

class FeedGenerationServiceTest {

    @Test
    void shouldReturnFeedGenerationStatus() {
        var repository = Mockito.mock(FeedGenerationStatusRepository.class);
        Mockito.when(repository.findAll()).thenReturn(java.util.Collections.emptyList());

        FeedGenerationService service = new FeedGenerationServiceImpl(repository);
        var response = service.getFeedGenerationStatus();

        assertThat(response).isNotNull();
        assertThat(response.getStatuses()).isEmpty();
    }
}
```

## TODO markers
- TODO: Convert feed generation records into response
- TODO: Trigger feed generation logic or update status from event
- TODO: Add feed generation and scheduling configuration
