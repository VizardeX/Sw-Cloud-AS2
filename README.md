# AWS Event-Driven Order Notification System

This is a simplified backend for an e-commerce platform using **event-driven architecture** in AWS. It processes incoming orders, stores them in DynamoDB, and sends notifications using SNS, SQS, and Lambda.

---

## Architecture Overview

**Event Flow**:
Client -> SNS: OrderTopic -> SQS: OrderQueue (DLQ configured) -> AWS Lambda: OrderProcessor -> DynamoDB: Orders Table

---

## Setup Instructions

### 1. **Create the DynamoDB Table**
- **Table name**: `Orders`
- **Partition key**: `orderId` (String)
- Additional attributes: `userId`, `itemName`, `quantity`, `status`, `timestamp`

### 2. **Create SNS Topic**
- Name: `OrderTopic`

### 3. **Create SQS Queues**
- `OrderQueue` (Standard Queue)
- `OrderDLQ` (Dead-Letter Queue)
- Configure `OrderQueue` to send failed messages to `OrderDLQ` after 3 failed receives

### 4. **Subscribe SQS to SNS**
- Add `OrderQueue` as a subscriber to `OrderTopic`

### 5. **Deploy Lambda Function**
- Name: `OrderProcessor`
- Runtime: Python 3.12
- Trigger: `OrderQueue`
- IAM Role: Attach the following policies:
  - `AmazonDynamoDBFullAccess`
  - `AmazonSQSFullAccess`
 
### 6. **Lambda Code**
Copy the lambda code from the text file in this repository and paste it 
