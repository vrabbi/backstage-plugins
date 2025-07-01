# Configuring the VCF Automation Frontend Plugin

This guide covers the configuration options available for the VCF Automation frontend plugin.

## Configuration File

The plugin is configured through your `app-config.yaml`. Here's a comprehensive example:

```yaml
vcfAutomation:
  # Base URL of your VCF Automation service
  baseUrl: http://your-vcf-automation-service
  
  # Enable permission checks
  enablePermissions: true
  
  # UI configuration
  ui:
    # Refresh interval in seconds
    refreshInterval: 30
    
    # Default view mode
    defaultView: 'summary'
    
    # Enable debug mode
    debug: false

# Proxy configuration for API access
proxy:
  endpoints:
    '/vcf-automation':
      target: http://your-vcf-automation-service
      changeOrigin: true
      # Optional: Headers for authentication
      headers:
        Authorization: ${VCF_AUTH_TOKEN}
```

## Component Configuration

### VCFAutomationDeploymentOverview Props
```typescript
interface VCFAutomationDeploymentOverviewProps {
  // Optional: Custom refresh interval
  refreshInterval?: number;
  
  // Optional: View mode
  viewMode?: 'summary' | 'detailed';
  
  // Optional: Custom styling
  className?: string;
  
  // Optional: Error handling
  onError?: (error: Error) => void;
}
```

### VCFAutomationVSphereVMOverview Props
```typescript
interface VCFAutomationVSphereVMOverviewProps {
  // Optional: Custom refresh interval
  refreshInterval?: number;
  
  // Optional: Show performance metrics
  showMetrics?: boolean;
  
  // Optional: Custom styling
  className?: string;
  
  // Optional: Error handling
  onError?: (error: Error) => void;
}
```

### VCFAutomationGenericResourceOverview Props
```typescript
interface VCFAutomationGenericResourceOverviewProps {
  // Optional: Custom refresh interval
  refreshInterval?: number;
  
  // Optional: Resource type filter
  resourceType?: string;
  
  // Optional: Custom styling
  className?: string;
  
  // Optional: Error handling
  onError?: (error: Error) => void;
}
```

## Integration Examples

### Basic Configuration
```typescript
import { VCFAutomationDeploymentOverview } from '@terasky/backstage-plugin-vcf-automation';

const deploymentPage = (
  <VCFAutomationDeploymentOverview
    refreshInterval={30}
    viewMode="summary"
  />
);
```

### Advanced Configuration
```typescript
import { VCFAutomationVSphereVMOverview } from '@terasky/backstage-plugin-vcf-automation';

const vmPage = (
  <VCFAutomationVSphereVMOverview
    refreshInterval={15}
    showMetrics={true}
    className="custom-vm-overview"
    onError={(error) => {
      console.error('VM Overview Error:', error);
      // Custom error handling
    }}
  />
);
```

## Permission Configuration

### Basic Permissions
```yaml
permission:
  vcfAutomation:
    roles:
      - name: admin
        permissions: ['read', 'write']
      - name: viewer
        permissions: ['read']
```

### Advanced Permissions
```yaml
permission:
  vcfAutomation:
    roles:
      - name: admin
        permissions: ['read', 'write', 'delete']
        resources: ['deployments', 'vms', 'projects']
      - name: operator
        permissions: ['read', 'write']
        resources: ['deployments', 'vms']
      - name: viewer
        permissions: ['read']
        resources: ['deployments']
```

## Environment Variables

Recommended environment variables:

```bash
# Authentication
export VCF_AUTH_TOKEN=your-auth-token

# Service configuration
export VCF_SERVICE_URL=http://your-vcf-automation-service

# UI configuration
export VCF_REFRESH_INTERVAL=30
export VCF_DEFAULT_VIEW=summary

# Debug mode
export VCF_DEBUG=false
```

## Best Practices

1. **Component Configuration**
   - Set appropriate refresh intervals
   - Handle errors gracefully
   - Use consistent styling
   - Implement proper validation

2. **Permission Management**
   - Define clear role boundaries
   - Implement least privilege
   - Document access levels
   - Regular permission audits

3. **Performance Optimization**
   - Cache API responses
   - Minimize refresh frequency
   - Implement error boundaries
   - Monitor resource usage

4. **Security**
   - Use secure tokens
   - Implement HTTPS
   - Validate input data
   - Regular security audits

## Example Configurations

### Development Environment
```yaml
vcfAutomation:
  baseUrl: http://localhost:8080
  enablePermissions: false
  ui:
    refreshInterval: 10
    debug: true
```

### Production Environment
```yaml
vcfAutomation:
  baseUrl: https://vcf-automation.example.com
  enablePermissions: true
  ui:
    refreshInterval: 30
    debug: false
```

### High-Security Environment
```yaml
vcfAutomation:
  baseUrl: https://vcf-automation.internal
  enablePermissions: true
  ui:
    refreshInterval: 60
  security:
    requireMFA: true
    tokenExpiry: 3600
```

For installation instructions, refer to the [Installation Guide](./install.md).
