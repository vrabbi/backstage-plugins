# Configuring the VCF Automation Backend Plugin

This guide covers the configuration options available for the VCF Automation backend plugin.

## Configuration File

The plugin is configured through your `app-config.yaml`. Here's a comprehensive example:

```yaml
vcfAutomation:
  # Base URL of your VCF Automation service
  baseUrl: 'https://your-vcf-automation-instance'
  
  # Authentication configuration
  authentication:
    username: 'your-username'
    password: 'your-password'
    domain: 'your-domain'
    
    # Optional: Token-based authentication
    token: 'your-token'
    
    # Optional: Certificate authentication
    cert:
      path: '/path/to/cert.pem'
      key: '/path/to/key.pem'
  
  # API configuration
  api:
    # Request timeout in seconds
    timeout: 30
    
    # Maximum retries
    maxRetries: 3
    
    # Retry delay in milliseconds
    retryDelay: 1000
    
    # Rate limiting
    rateLimit:
      enabled: true
      maxRequests: 100
      timeWindow: 60
  
  # Event configuration
  events:
    # Enable event streaming
    streaming: true
    
    # WebSocket configuration
    websocket:
      heartbeatInterval: 30
      reconnectDelay: 5000
    
    # Event filtering
    filters:
      types: ['deployment', 'resource', 'project']
      severity: ['info', 'warning', 'error']
  
  # Cache configuration
  cache:
    enabled: true
    ttl: 300
    maxSize: 1000
  
  # Permission configuration
  permission:
    enabled: true
    defaultRole: 'viewer'
    roles:
      - name: admin
        permissions: ['read', 'write', 'delete']
        resources: ['deployments', 'resources', 'projects']
      - name: operator
        permissions: ['read', 'write']
        resources: ['deployments', 'resources']
      - name: viewer
        permissions: ['read']
        resources: ['deployments']
```

## Authentication Configuration

### Basic Authentication
```yaml
authentication:
  username: 'your-username'
  password: 'your-password'
  domain: 'your-domain'
```

### Token Authentication
```yaml
authentication:
  token: 'your-token'
```

### Certificate Authentication
```yaml
authentication:
  cert:
    path: '/path/to/cert.pem'
    key: '/path/to/key.pem'
    ca: '/path/to/ca.pem'
```

## API Configuration

### Request Settings
```yaml
api:
  timeout: 30
  maxRetries: 3
  retryDelay: 1000
  validateStatus: true
```

### Rate Limiting
```yaml
api:
  rateLimit:
    enabled: true
    maxRequests: 100
    timeWindow: 60
    strategy: 'sliding'
```

## Event Configuration

### WebSocket Settings
```yaml
events:
  streaming: true
  websocket:
    heartbeatInterval: 30
    reconnectDelay: 5000
    maxRetries: 10
```

### Event Filtering
```yaml
events:
  filters:
    types:
      - deployment
      - resource
      - project
    severity:
      - info
      - warning
      - error
    sources:
      - vcf-automation
      - vcf-ingestor
```

## Cache Configuration

### Memory Cache
```yaml
cache:
  enabled: true
  type: 'memory'
  ttl: 300
  maxSize: 1000
```

### Redis Cache
```yaml
cache:
  enabled: true
  type: 'redis'
  connection:
    host: 'localhost'
    port: 6379
    password: 'your-password'
```

## Permission Configuration

### Basic Roles
```yaml
permission:
  enabled: true
  defaultRole: 'viewer'
  roles:
    - name: admin
      permissions: ['read', 'write', 'delete']
    - name: viewer
      permissions: ['read']
```

### Resource-Based Roles
```yaml
permission:
  enabled: true
  defaultRole: 'viewer'
  roles:
    - name: admin
      permissions: ['read', 'write', 'delete']
      resources: ['deployments', 'resources', 'projects']
    - name: deployment-admin
      permissions: ['read', 'write']
      resources: ['deployments']
    - name: resource-viewer
      permissions: ['read']
      resources: ['resources']
```

## Environment Variables

Recommended environment variables:

```bash
# Authentication
export VCF_AUTOMATION_USERNAME=your-username
export VCF_AUTOMATION_PASSWORD=your-password
export VCF_AUTOMATION_DOMAIN=your-domain
export VCF_AUTOMATION_TOKEN=your-token

# Service configuration
export VCF_AUTOMATION_URL=https://your-vcf-automation-instance
export VCF_AUTOMATION_API_TIMEOUT=30
export VCF_AUTOMATION_MAX_RETRIES=3

# Cache configuration
export VCF_AUTOMATION_CACHE_TTL=300
export VCF_AUTOMATION_CACHE_SIZE=1000

# Event configuration
export VCF_AUTOMATION_EVENTS_ENABLED=true
export VCF_AUTOMATION_WEBSOCKET_INTERVAL=30
```

## Best Practices

1. **Authentication**
   - Use environment variables for credentials
   - Rotate tokens regularly
   - Implement certificate authentication
   - Use secure connections

2. **API Configuration**
   - Set appropriate timeouts
   - Configure retry logic
   - Implement rate limiting
   - Handle errors gracefully

3. **Event Handling**
   - Configure event filtering
   - Set up error handling
   - Monitor WebSocket connections
   - Implement reconnection logic

4. **Caching**
   - Set appropriate TTL
   - Monitor cache size
   - Implement cache invalidation
   - Use distributed caching

5. **Permissions**
   - Define clear roles
   - Implement resource-based access
   - Set default permissions
   - Regular permission audits

For installation instructions, refer to the [Installation Guide](./install.md).
