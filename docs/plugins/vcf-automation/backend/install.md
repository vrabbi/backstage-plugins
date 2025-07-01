# Installing the VCF Automation Backend Plugin

This guide will help you install and set up the VCF Automation backend plugin in your Backstage instance.

## Prerequisites

Before installing the plugin, ensure you have:

1. A running Backstage backend instance
2. Access to a VCF Automation service
3. VCF Ingestor Plugin installed
4. Proper authentication credentials

## Installation Steps

### 1. Add the Package

Install the plugin package using yarn:

```bash
yarn --cwd packages/backend add @terasky/backstage-plugin-vcf-automation-backend
```

### 2. Add to Backend

Modify your backend in `packages/backend/src/index.ts`:

```typescript
import { createBackend } from '@backstage/backend-defaults';

const backend = createBackend();

// Add the VCF Automation backend plugin
backend.add(import('@terasky/backstage-plugin-vcf-automation-backend'));

backend.start();
```

### 3. Configure Authentication

Add authentication configuration to your `app-config.yaml`:

```yaml
vcfAutomation:
  baseUrl: 'https://your-vcf-automation-instance'
  authentication:
    username: 'your-username'
    password: 'your-password'
    domain: 'your-domain'
```

### 4. Configure Environment Variables

Set up required environment variables:

```bash
export VCF_AUTOMATION_USERNAME=your-username
export VCF_AUTOMATION_PASSWORD=your-password
export VCF_AUTOMATION_DOMAIN=your-domain
```

### 5. Configure Permissions

Add permission configuration to your `app-config.yaml`:

```yaml
permission:
  vcfAutomation:
    roles:
      - name: admin
        permissions: ['read', 'write', 'delete']
      - name: operator
        permissions: ['read', 'write']
      - name: viewer
        permissions: ['read']
```

### 6. Configure API Routes

Add API route configuration to your `app-config.yaml`:

```yaml
backend:
  baseUrl: http://localhost:7007
  listen:
    port: 7007
  cors:
    origin: http://localhost:3000
    methods: [GET, POST, PUT, DELETE]
    credentials: true
```

## Verification

After installation, verify that:

1. The plugin appears in your package.json dependencies
2. The backend starts without errors
3. API endpoints are accessible
4. Authentication is working
5. Permissions are enforced

## Troubleshooting

Common issues and solutions:

1. **Authentication Issues**
   - Verify credentials
   - Check environment variables
   - Test VCF service access
   - Review error logs

2. **API Connection Problems**
   - Check service URL
   - Verify network access
   - Review proxy settings
   - Test API endpoints

3. **Permission Errors**
   - Check role configuration
   - Verify user permissions
   - Review access policies
   - Test with admin account

4. **Event Streaming Issues**
   - Check WebSocket connection
   - Verify event subscription
   - Review stream configuration
   - Test event flow

## Health Checks

Add health check endpoints to monitor the plugin:

```yaml
healthcheck:
  endpoints:
    - url: /api/vcf-automation/health
      method: GET
    - url: /api/vcf-automation/ready
      method: GET
```

## Logging Configuration

Configure logging for the plugin:

```yaml
logging:
  level: info
  format: json
  filters:
    - type: vcf-automation
      level: debug
```

## Metrics Configuration

Enable metrics collection:

```yaml
metrics:
  backends: ['prometheus']
  defaultMetrics:
    enabled: true
  vcfAutomation:
    enabled: true
    collectDefaultMetrics: true
```

For configuration options and customization, proceed to the [Configuration Guide](./configure.md).
