# Installing the VCF Automation Frontend Plugin

This guide will help you install and set up the VCF Automation frontend plugin in your Backstage instance.

## Prerequisites

Before installing the plugin, ensure you have:

1. A working Backstage instance
2. VCF Automation Backend Plugin installed
3. VCF Ingestor Plugin configured
4. Access to VCF deployments

## Installation Steps

### 1. Add the Package

Install the plugin package using yarn:

```bash
yarn --cwd packages/app add @terasky/backstage-plugin-vcf-automation
```

### 2. Register the Plugin

Add the plugin to your app's APIs in `packages/app/src/apis.ts`:

```typescript
import {
  vcfAutomationApiRef,
  VcfAutomationClient,
} from '@terasky/backstage-plugin-vcf-automation';

export const apis: AnyApiFactory[] = [
  // ... other API factories
  createApiFactory({
    api: vcfAutomationApiRef,
    deps: { discoveryApi: discoveryApiRef, identityApi: identityApiRef },
    factory: ({ discoveryApi, identityApi }) => 
      new VcfAutomationClient({ discoveryApi, identityApi }),
  }),
];
```

### 3. Add to App.tsx

Configure the plugin in `packages/app/src/App.tsx`:

```typescript
import { vcfAutomationPlugin } from '@terasky/backstage-plugin-vcf-automation';

const app = createApp({
  apis,
  bindRoutes({ bind }) {
    // ... other bindings
    bind(vcfAutomationPlugin.externalRoutes, {
      catalogIndex: catalogPlugin.routes.catalogIndex,
    });
  },
});
```

### 4. Add Components to Entity Pages

Add the VCF Automation components to your entity pages in `packages/app/src/components/catalog/EntityPage.tsx`:

```typescript
import {
  VCFAutomationDeploymentOverview,
  VCFAutomationDeploymentDetails,
  VCFAutomationVSphereVMOverview,
  VCFAutomationVSphereVMDetails,
  VCFAutomationGenericResourceOverview,
  VCFAutomationGenericResourceDetails,
  VCFAutomationProjectOverview,
  VCFAutomationProjectDetails,
} from '@terasky/backstage-plugin-vcf-automation';
import { Entity } from '@backstage/catalog-model';

// For VSphere VMs
const vcfAutomationVSphereVMPage = (
  <EntityLayout>
    <EntityLayout.Route path="/" title="Overview">
      <Grid container spacing={3} alignItems="stretch">
        <Grid item md={6}>
          <EntityAboutCard variant="gridItem" />
        </Grid>
        <Grid item md={6}>
          <VCFAutomationVSphereVMOverview />
        </Grid>
      </Grid>
    </EntityLayout.Route>
    <EntityLayout.Route path="/vcf-automation" title="VCF Automation">
      <VCFAutomationVSphereVMDetails />
    </EntityLayout.Route>
  </EntityLayout>
);

// Add to component page switch
const componentPage = (
  <EntitySwitch>
    <EntitySwitch.Case if={isComponentType('Cloud.vSphere.Machine')}>
      {vcfAutomationVSphereVMPage}
    </EntitySwitch.Case>
    // ... other cases
  </EntitySwitch>
);

// For VCF Deployments
const hasVcfAutomationDeploymentStatus = (entity: Entity): boolean => 
  Boolean(entity.metadata?.annotations?.['terasky.backstage.io/vcf-automation-deployment-status']);

const vcfAutomationDeploymentPage = (
  <EntityLayout>
    <EntityLayout.Route path="/" title="Overview">
      <Grid container spacing={3} alignItems="stretch">
        <Grid item md={6}>
          <VCFAutomationDeploymentOverview />
        </Grid>
      </Grid>
    </EntityLayout.Route>
    <EntityLayout.Route path="/vcf-automation" title="VCF Automation">
      <VCFAutomationDeploymentDetails />
    </EntityLayout.Route>
  </EntityLayout>
);

// For Generic Resources
const hasVcfAutomationResourceType = (entity: Entity): boolean => 
  Boolean(entity.metadata?.annotations?.['terasky.backstage.io/vcf-automation-resource-type']);

const vcfAutomationGenericResourcePage = (
  <EntityLayout>
    <EntityLayout.Route path="/" title="Overview">
      <Grid container spacing={3} alignItems="stretch">
        <Grid item md={6}>
          <VCFAutomationGenericResourceOverview />
        </Grid>
      </Grid>
    </EntityLayout.Route>
    <EntityLayout.Route path="/vcf-automation" title="VCF Automation">
      <VCFAutomationGenericResourceDetails />
    </EntityLayout.Route>
  </EntityLayout>
);

// For Projects (in Domain page)
const domainPage = (
  <EntityLayout>
    <EntityLayout.Route path="/" title="Overview">
      <Grid container spacing={3} alignItems="stretch">
        <Grid item md={6}>
          <VCFAutomationProjectOverview />
        </Grid>
      </Grid>
    </EntityLayout.Route>
    <EntityLayout.Route path="/vcf-automation" title="VCF Automation">
      <VCFAutomationProjectDetails />
    </EntityLayout.Route>
  </EntityLayout>
);

// Add a Resources Page
const resourcePage = (
  <EntitySwitch>
    <EntitySwitch.Case if={hasVcfAutomationResourceType}>
      {vcfAutomationGenericResourcePage}
    </EntitySwitch.Case>
    <EntitySwitch.Case>
      {defaultEntityPage}
    </EntitySwitch.Case>
  </EntitySwitch>
);

// Update the entityPage constant
export const entityPage = (
  <EntitySwitch>
    <EntitySwitch.Case if={isKind('resource')} children={resourcePage} />
    // ... other cases
  </EntitySwitch>
);
```

### 5. Configure Backend Integration

Add the following to your `app-config.yaml`:

```yaml
vcfAutomation:
  baseUrl: http://your-vcf-automation-service
```

## Verification

After installation, verify that:

1. The plugin appears in your package.json dependencies
2. The API client is registered correctly
3. Components are rendering on entity pages
4. Backend communication is working
5. Permissions are properly configured

## Troubleshooting

Common issues and solutions:

1. **Component Not Rendering**
   - Check component imports
   - Verify entity type conditions
   - Review route configuration
   - Check console errors

2. **API Communication Issues**
   - Verify backend URL
   - Check proxy configuration
   - Review API client setup
   - Test backend connectivity

3. **Permission Problems**
   - Check permission configuration
   - Verify user roles
   - Review access policies
   - Test with admin account

For configuration options and customization, proceed to the [Configuration Guide](./configure.md).
