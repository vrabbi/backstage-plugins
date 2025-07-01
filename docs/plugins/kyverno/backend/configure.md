# Configuring the Kyverno Permissions Backend Plugin

This guide covers the configuration options for the Kyverno Permissions backend plugin.

## Basic Configuration

The plugin uses Backstage's permission framework. To enable it, add the following to your `app-config.yaml`:

```yaml
permission:
  enabled: true # Enable Backstage permission framework
```

## Permission Policy Configuration

You can configure permission policies in your Backstage permission policy file. Here's an example policy that implements Kyverno permissions:

```typescript
// packages/backend/src/plugins/permission.ts
import { KyvernoPermission } from '@terasky/backstage-plugin-kyverno-common';

class KyvernoPermissionPolicy implements PermissionPolicy {
  async handle(
    request: PolicyQuery,
    user?: BackstageIdentityResponse,
  ): Promise<PolicyDecision> {
    if (isPermission(request.permission, KyvernoPermission)) {
      // Implement your permission logic here
      return { result: AuthorizeResult.ALLOW };
    }

    return { result: AuthorizeResult.DENY };
  }
}
```

## Available Permissions

The plugin provides three main permission types that you can use in your policies:

1. `kyverno.overview.view`: Controls access to overview policy report data
2. `kyverno.reports.view`: Controls access to detailed policy report data
3. `kyverno.policy.view-yaml`: Controls access to policy YAML manifests

## Best Practices

1. **Permission Management**
   - Follow the principle of least privilege
   - Regularly review and update policies
   - Use specific permissions over wildcards

2. **Security**
   - Enable audit logging
   - Implement proper error handling
   - Validate all inputs

3. **Integration**
   - Verify Kubernetes plugin configurations
   - Check authentication settings
   - Monitor permission enforcement
