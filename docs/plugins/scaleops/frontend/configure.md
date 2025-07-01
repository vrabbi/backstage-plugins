# Configuring the ScaleOps Frontend Plugin

This guide covers the configuration options available for the ScaleOps frontend plugin.

## Configuration File

The plugin is configured through your `app-config.yaml`. Here's a comprehensive example:

```yaml
scaleops:
  # Base URL of your ScaleOps instance
  baseUrl: 'https://your-scaleops-instance.com'
  
  # Enable direct links to ScaleOps dashboard
  linkToDashboard: true
  
  # Authentication configuration
  authentication:
    enabled: true
    user: 'YOUR_USERNAME'
    password: 'YOUR_PASSWORD'

# Proxy configuration for API access
proxy:
  endpoints:
    '/scaleops':
      target: 'https://your-scaleops-instance.com'
      changeOrigin: true
      # Optional: Additional headers
      headers:
        Authorization: 'Bearer ${SCALEOPS_TOKEN}'
```

## Authentication Options

### Internal Authentication
```yaml
authentication:
  enabled: true
  user: 'username'
  password: 'password'
```

### No Authentication
```yaml
authentication:
  enabled: false
```

### Environment Variables
```yaml
authentication:
  enabled: true
  user: ${SCALEOPS_USERNAME}
  password: ${SCALEOPS_PASSWORD}
```

## Component Configuration

### ScaleOpsDashboard Props

```typescript
interface ScaleOpsDashboardProps {
  // Optional: Default view mode
  defaultView?: 'summary' | 'detailed';
  
  // Optional: Refresh interval in seconds
  refreshInterval?: number;
  
  // Optional: Custom styling
  className?: string;
  
  // Optional: Error handling
  onError?: (error: Error) => void;
}
```

## Integration Examples

### Basic Dashboard
```typescript
import { ScaleOpsDashboard } from '@terasky/backstage-plugin-scaleops-frontend';

const dashboard = (
  <ScaleOpsDashboard
    defaultView="summary"
    refreshInterval={300}
  />
);
```

### Advanced Dashboard
```typescript
import { ScaleOpsDashboard } from '@terasky/backstage-plugin-scaleops-frontend';

const dashboard = (
  <ScaleOpsDashboard
    defaultView="detailed"
    refreshInterval={60}
    className="custom-dashboard"
    onError={(error) => {
      console.error('ScaleOps Error:', error);
      // Custom error handling
    }}
  />
);
```

## Proxy Configuration

### Basic Setup
```yaml
proxy:
  endpoints:
    '/scaleops':
      target: 'https://your-scaleops-instance.com'
      changeOrigin: true
```

### With Authentication
```yaml
proxy:
  endpoints:
    '/scaleops':
      target: 'https://your-scaleops-instance.com'
      changeOrigin: true
      headers:
        Authorization: 'Basic ${SCALEOPS_AUTH}'
```

### With Custom Headers
```yaml
proxy:
  endpoints:
    '/scaleops':
      target: 'https://your-scaleops-instance.com'
      changeOrigin: true
      headers:
        Authorization: 'Bearer ${SCALEOPS_TOKEN}'
        'X-Custom-Header': 'custom-value'
```

## Environment Variables

Recommended environment variables:

```bash
# Authentication
export SCALEOPS_USERNAME=your-username
export SCALEOPS_PASSWORD=your-password

# Or token-based auth
export SCALEOPS_TOKEN=your-token

# Base64 encoded auth
export SCALEOPS_AUTH=$(echo -n "username:password" | base64)

# Instance configuration
export SCALEOPS_URL=https://your-scaleops-instance.com
```

## Best Practices

1. **Authentication**
   - Use environment variables
   - Rotate credentials regularly
   - Implement proper secret management
   - Use secure authentication methods

2. **Performance**
   - Set appropriate refresh intervals
   - Configure caching if needed
   - Monitor API usage
   - Handle errors gracefully

3. **Integration**
   - Use consistent configuration
   - Document custom settings
   - Monitor dashboard performance
   - Maintain security standards

For installation instructions, refer to the [Installation Guide](./install.md).
