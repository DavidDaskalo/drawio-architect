# AWS Serverless Web Application Architecture - Test Diagram

**Generated:** 2026-02-15
**Purpose:** Validate AWS Part 2 service icons (Databases, Storage, Networking, Management Tools)
**File:** `aws-serverless-test-part2.drawio`

## Architecture Overview

This diagram demonstrates a complete AWS serverless web application using services from both Part 1 and Part 2 of the AWS DrawIO icon library.

## Services Used

### Part 1 Services (Compute)
- **AWS Lambda** (3 instances)
  - API Handler - Processes incoming API requests
  - Data Processor - Handles background data processing
  - File Processor - Processes files uploaded to S3

### Part 2 Services (Databases)
- **Amazon DynamoDB** - NoSQL database for user data and metadata

### Part 2 Services (Storage)
- **Amazon S3** (2 instances)
  - Static Assets bucket - Stores frontend assets (HTML, CSS, JS, images)
  - Data Lake bucket - Stores analytics data and uploaded files

### Part 2 Services (Networking & Content Delivery)
- **Amazon API Gateway** - REST API endpoint for client requests

### Part 2 Services (Management Tools)
- **Amazon CloudWatch** - Centralized logging and monitoring for all Lambda functions

## Architecture Components

### VPC Structure
- **VPC** - Virtual Private Cloud containing all resources
  - **Public Subnet** - Contains API Gateway (internet-facing)
  - **Private Subnet** - Contains Lambda functions, DynamoDB, S3, and CloudWatch

## Data Flow

1. **Client Request Flow:**
   - Client → API Gateway (HTTPS)
   - API Gateway → Lambda API Handler
   - Lambda API Handler ↔ DynamoDB (Read/Write user data)
   - Lambda API Handler → S3 Static Assets (Fetch frontend resources)

2. **Background Processing Flow:**
   - Lambda Data Processor → DynamoDB (Query data)
   - Lambda Data Processor → S3 Data Lake (Store analytics)

3. **Event-Driven File Processing:**
   - S3 Data Lake → Lambda File Processor (S3 event trigger on file upload)
   - Lambda File Processor → DynamoDB (Update file metadata)

4. **Monitoring Flow (dashed lines):**
   - All Lambda functions → CloudWatch (Logs and metrics)

## Connection Types

- **Solid lines** - Direct data flow
- **Dashed lines** - Monitoring/logging connections
- **Bidirectional arrows** - Read/Write operations (e.g., Lambda ↔ DynamoDB)
- **Single arrows** - One-way data flow

## Icon Validation

All services in this diagram use validated icons from the `aws-icons.json` database:

| Service | Shape Name | Category | Validation Status |
|---------|-----------|----------|-------------------|
| Lambda | `lambda` | compute | ✅ Part 1 (100%) |
| DynamoDB | `dynamodb` | databases | ✅ Part 2 (96.6%) |
| S3 | `s3` | storage | ✅ Part 2 (96.6%) |
| API Gateway | `api gateway` ⚠️ | networking_content_delivery | ✅ Part 2 (96.6%) |
| CloudWatch | `cloudwatch` | management_tools | ✅ Part 2 (96.6%) |

⚠️ **Note:** API Gateway uses space-separated name `api gateway`, NOT `api_gateway`

## Visual Design

- **Icon size:** 64x64 pixels (AWS standard)
- **Font color:** #232F3E (AWS universal dark gray)
- **Spacing:** 120px between icons (standard)
- **Connection style:** Orthogonal routing with 2pt stroke
- **Container colors:**
  - VPC border: #8C4FFF (purple)
  - Public subnet: #248814 (green) with #E9F3E6 background
  - Private subnet: #147EBA (blue) with #E6F2F8 background

## Testing

To validate this diagram:

```bash
# Validate XML structure
python scripts/validate-drawio.py output/aws-serverless-test-part2.drawio --verbose

# Validate icon references
python scripts/validate-aws-icons.py

# Open in DrawIO Desktop
./scripts/open-diagram.sh output/aws-serverless-test-part2.drawio

# Export to PNG
./scripts/export-diagram.sh output/aws-serverless-test-part2.drawio png
```

## Success Criteria

✅ All Part 2 icons render correctly
✅ Space-separated names work (e.g., `api gateway`)
✅ VPC and subnet containers display properly
✅ All connections reference valid IDs
✅ Labels and spacing follow AWS best practices

## Part 2 Categories Represented

This diagram validates icons from 5 of the 17 Part 2 categories:

1. ✅ **Databases** - DynamoDB
2. ✅ **Storage** - S3 (2 instances)
3. ✅ **Networking & Content Delivery** - API Gateway
4. ✅ **Management Tools** - CloudWatch
5. (Compute is Part 1, but included for completeness)

## Next Steps

Additional test diagrams could validate remaining Part 2 categories:
- Developer Tools (CodePipeline, CodeBuild, etc.)
- Security & Identity (IAM, Cognito, KMS, etc.)
- Migration & Modernization (DMS, Application Migration Service)
- Internet of Things (IoT Core, IoT Analytics)
- Media Services (MediaConvert, MediaLive)
