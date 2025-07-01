# Configuring the Crossplane Permissions Backend Plugin

The Crossplane Permissions backend plugin integrates with Backstage's permission framework to provide access control for Crossplane resources.

## Available Permissions

The plugin provides the following permissions for managing Crossplane resources:

### Claims
- `crossplane.claims.list`: List Crossplane Claims
- `crossplane.claims.view-yaml`: View YAML of Crossplane Claims
- `crossplane.claims.show-events`: View Events of Crossplane Claims

### Composite Resources
- `crossplane.composite-resources.list`: List Crossplane Composite Resources
- `crossplane.composite-resources.view-yaml`: View YAML of Crossplane Composite Resources
- `crossplane.composite-resources.show-events`: View Events of Crossplane Composite Resources

### Managed Resources
- `crossplane.managed-resources.list`: List Crossplane Managed Resources
- `crossplane.managed-resources.view-yaml`: View YAML of Crossplane Managed Resources
- `crossplane.managed-resources.show-events`: View Events of Crossplane Managed Resources

### Additional Resources
- `crossplane.additional-resources.list`: List Crossplane Additional Resources (XRD, Composition, Function)
- `crossplane.additional-resources.view-yaml`: View YAML of Crossplane Additional Resources
- `crossplane.additional-resources.show-events`: View Events of Crossplane Additional Resources

### Resource Graph
- `crossplane.resource-graph.show`: View Resource Graph of Crossplane Resources

## Basic Configuration

The plugin uses Backstage's permission framework. To enable it, add the following to your `app-config.yaml`:

```yaml
permission:
  enabled: true # Enable Backstage permission framework
```

## Permission Policy Configuration

You can configure permission policies in your Backstage permission policy file. Here's an example policy that grants all Crossplane permissions to a specific role:

```typescript
// packages/backend/src/plugins/permission.ts
import { CrossplanePermission } from '@terasky/backstage-plugin-crossplane-common';

class CrossplanePermissionPolicy implements PermissionPolicy {
  async handle(
    request: PolicyQuery,
    user?: BackstageIdentityResponse,
  ): Promise<PolicyDecision> {
    if (isPermission(request.permission, CrossplanePermission)) {
      // Implement your permission logic here
      return { result: AuthorizeResult.ALLOW };
    }

    return { result: AuthorizeResult.DENY };
  }
}

## Best Practices

### Security

1. **Permission Policies**
   - Follow the principle of least privilege
   - Regularly review and update policies
   - Use specific permissions over wildcards

2. **Authentication**
   - Use secure authentication methods
   - Implement token rotation
   - Enable audit logging

### Monitoring

1. **Logging**
   - Configure appropriate log levels
   - Implement log rotation
   - Set up log aggregation

2. **Metrics**
   - Monitor permission checks
   - Track API usage
   - Set up alerts for anomalies

## Troubleshooting

### Common Issues

1. **Permission Denied**
   - Check policy configuration
   - Verify user roles
   - Review permission logs

2. **Integration Problems**
   - Verify service connections
   - Check authentication configuration
   - Review Kubernetes plugin configurations
