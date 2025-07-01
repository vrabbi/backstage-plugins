# VCF Automation Backend Plugin

[![npm latest version](https://img.shields.io/npm/v/@terasky/backstage-plugin-vcf-automation-backend/latest.svg)](https://www.npmjs.com/package/@terasky/backstage-plugin-vcf-automation-backend)

## Overview

The VCF Automation backend plugin provides the server-side functionality required for managing VMware Cloud Foundation (VCF) deployments through Backstage. It handles API integration, permission management, and resource operations while providing a robust event handling system.

## Features

### API Integration
- VCF service communication
- Authentication handling
- Request processing
- Response formatting
- Error management

### Permission Management
- Role-based access control
- Permission validation
- Resource authorization
- User authentication
- Access policies

### Entity Processing
- Relationship mapping
- Entity updates
- State management
- Metadata handling
- Synchronization

### Operation Management
- Deployment operations
- Resource management
- Task processing
- Status tracking
- Event correlation

### Event Handling
- Real-time events
- Notification system
- Event streaming
- State updates
- Error reporting

## API Endpoints

### Deployment Management
```typescript
// Get deployment details
GET /api/vcf-automation/deployments/:id
Response: {
  id: string;
  status: string;
  configuration: object;
  operations: Operation[];
}

// Execute deployment operation
POST /api/vcf-automation/deployments/:id/operations
Request: {
  operationType: string;
  parameters: object;
}
Response: {
  operationId: string;
  status: string;
}
```

### Resource Management
```typescript
// Get resource details
GET /api/vcf-automation/resources/:id
Response: {
  id: string;
  type: string;
  status: string;
  configuration: object;
}

// Update resource
PUT /api/vcf-automation/resources/:id
Request: {
  configuration: object;
}
Response: {
  id: string;
  status: string;
}
```

### Project Management
```typescript
// Get project details
GET /api/vcf-automation/projects/:id
Response: {
  id: string;
  name: string;
  resources: Resource[];
  configuration: object;
}

// Update project
PUT /api/vcf-automation/projects/:id
Request: {
  configuration: object;
}
Response: {
  id: string;
  status: string;
}
```

### Event Streaming
```typescript
// Stream VCF events
GET /api/vcf-automation/events
Response: EventStream {
  type: string;
  data: object;
  timestamp: string;
}
```

## Technical Details

### Service Architecture
The plugin implements:
1. API controllers
2. Service layer
3. Permission handlers
4. Event processors
5. Entity managers

### Integration Points
- VCF Automation service
- Backstage catalog
- Permission framework
- Event system
- Entity providers

### Data Models
```typescript
interface Deployment {
  id: string;
  status: string;
  configuration: object;
  operations: Operation[];
  metadata: {
    createdAt: string;
    updatedAt: string;
    createdBy: string;
  };
}

interface Resource {
  id: string;
  type: string;
  status: string;
  configuration: object;
  relationships: {
    deployment?: string;
    project?: string;
    dependencies: string[];
  };
}

interface Project {
  id: string;
  name: string;
  resources: Resource[];
  configuration: object;
  metadata: {
    owner: string;
    domain: string;
  };
}

interface Operation {
  id: string;
  type: string;
  status: string;
  parameters: object;
  result?: object;
  timestamp: string;
}

interface Event {
  type: string;
  data: object;
  timestamp: string;
  source: {
    type: string;
    id: string;
  };
}
```

## Use Cases

### Deployment Management
1. Create deployments
2. Monitor status
3. Execute operations
4. Track progress
5. Handle errors

### Resource Tracking
1. Monitor resources
2. Update configurations
3. Track relationships
4. Manage state
5. Handle events

### Project Administration
1. Configure projects
2. Manage resources
3. Track deployments
4. Handle permissions
5. Monitor status

For installation and configuration details, refer to the [Installation Guide](./install.md) and [Configuration Guide](./configure.md).
