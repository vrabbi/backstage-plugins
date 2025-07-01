# VCF Automation Frontend Plugin

[![npm latest version](https://img.shields.io/npm/v/@terasky/backstage-plugin-vcf-automation/latest.svg)](https://www.npmjs.com/package/@terasky/backstage-plugin-vcf-automation)

## Overview

The VCF Automation frontend plugin provides a comprehensive interface for managing and monitoring VMware Cloud Foundation (VCF) deployments within Backstage. It offers detailed views of deployment operations, resource states, and project configurations while integrating with Backstage's permission framework.

## Features

### Deployment Management
- Deployment status tracking
- Operation monitoring
- Configuration management
- Status visualization
- Event tracking

### Resource Management
- vSphere VM monitoring
- Resource state tracking
- Configuration viewing
- Performance metrics
- Status updates

### Project Configuration
- Project status overview
- Configuration management
- Resource allocation
- Access control
- Deployment tracking

## Components

### Deployment Components

#### VCFAutomationDeploymentOverview
The overview component that provides:
- Deployment status
- Operation progress
- Configuration summary
- Quick actions

Example usage:
```typescript
import { VCFAutomationDeploymentOverview } from '@terasky/backstage-plugin-vcf-automation';

const deploymentPage = (
  <Grid container>
    <Grid item md={6}>
      <VCFAutomationDeploymentOverview />
    </Grid>
  </Grid>
);
```

#### VCFAutomationDeploymentDetails
Detailed deployment information:
- Complete configuration
- Operation history
- Resource allocation
- Status details

Example usage:
```typescript
import { VCFAutomationDeploymentDetails } from '@terasky/backstage-plugin-vcf-automation';

const deploymentDetailsPage = (
  <EntityLayout.Route path="/vcf-automation" title="VCF Automation">
    <VCFAutomationDeploymentDetails />
  </EntityLayout.Route>
);
```

### VM Components

#### VCFAutomationVSphereVMOverview
VM overview component showing:
- VM status
- Resource usage
- Configuration summary
- Quick actions

Example usage:
```typescript
import { VCFAutomationVSphereVMOverview } from '@terasky/backstage-plugin-vcf-automation';

const vmPage = (
  <Grid container>
    <Grid item md={6}>
      <VCFAutomationVSphereVMOverview />
    </Grid>
  </Grid>
);
```

#### VCFAutomationVSphereVMDetails
Detailed VM information:
- Complete configuration
- Performance metrics
- Network settings
- Storage details

Example usage:
```typescript
import { VCFAutomationVSphereVMDetails } from '@terasky/backstage-plugin-vcf-automation';

const vmDetailsPage = (
  <EntityLayout.Route path="/vcf-automation" title="VCF Automation">
    <VCFAutomationVSphereVMDetails />
  </EntityLayout.Route>
);
```

### Resource Components

#### VCFAutomationGenericResourceOverview
Generic resource overview showing:
- Resource status
- Type information
- Configuration summary
- Quick actions

Example usage:
```typescript
import { VCFAutomationGenericResourceOverview } from '@terasky/backstage-plugin-vcf-automation';

const resourcePage = (
  <Grid container>
    <Grid item md={6}>
      <VCFAutomationGenericResourceOverview />
    </Grid>
  </Grid>
);
```

#### VCFAutomationGenericResourceDetails
Detailed resource information:
- Complete configuration
- Status history
- Dependencies
- Related resources

Example usage:
```typescript
import { VCFAutomationGenericResourceDetails } from '@terasky/backstage-plugin-vcf-automation';

const resourceDetailsPage = (
  <EntityLayout.Route path="/vcf-automation" title="VCF Automation">
    <VCFAutomationGenericResourceDetails />
  </EntityLayout.Route>
);
```

### Project Components

#### VCFAutomationProjectOverview
Project overview component showing:
- Project status
- Resource summary
- Deployment overview
- Quick actions

Example usage:
```typescript
import { VCFAutomationProjectOverview } from '@terasky/backstage-plugin-vcf-automation';

const projectPage = (
  <Grid container>
    <Grid item md={6}>
      <VCFAutomationProjectOverview />
    </Grid>
  </Grid>
);
```

#### VCFAutomationProjectDetails
Detailed project information:
- Complete configuration
- Resource allocation
- Deployment status
- Access control

Example usage:
```typescript
import { VCFAutomationProjectDetails } from '@terasky/backstage-plugin-vcf-automation';

const projectDetailsPage = (
  <EntityLayout.Route path="/vcf-automation" title="VCF Automation">
    <VCFAutomationProjectDetails />
  </EntityLayout.Route>
);
```

## Integration Examples

### Complete Entity Page
```typescript
import {
  VCFAutomationDeploymentOverview,
  VCFAutomationDeploymentDetails,
  isVcfAutomationAvailable,
} from '@terasky/backstage-plugin-vcf-automation';

const entityPage = (
  <EntitySwitch>
    <EntitySwitch.Case if={isVcfAutomationAvailable}>
      <EntityLayout>
        <EntityLayout.Route path="/" title="Overview">
          <Grid container spacing={3}>
            <Grid item md={6}>
              <VCFAutomationDeploymentOverview />
            </Grid>
          </Grid>
        </EntityLayout.Route>
        <EntityLayout.Route path="/vcf-automation" title="VCF Automation">
          <VCFAutomationDeploymentDetails />
        </EntityLayout.Route>
      </EntityLayout>
    </EntitySwitch.Case>
  </EntitySwitch>
);
```

### Resource Type Detection
```typescript
import { Entity } from '@backstage/catalog-model';

const hasVcfAutomationResourceType = (entity: Entity): boolean => 
  Boolean(entity.metadata?.annotations?.['terasky.backstage.io/vcf-automation-resource-type']);

const resourcePage = (
  <EntitySwitch>
    <EntitySwitch.Case if={hasVcfAutomationResourceType}>
      <EntityLayout>
        <EntityLayout.Route path="/" title="Overview">
          <VCFAutomationGenericResourceOverview />
        </EntityLayout.Route>
        <EntityLayout.Route path="/vcf-automation" title="Details">
          <VCFAutomationGenericResourceDetails />
        </EntityLayout.Route>
      </EntityLayout>
    </EntitySwitch.Case>
  </EntitySwitch>
);
```

For installation and configuration details, refer to the [Installation Guide](./install.md) and [Configuration Guide](./configure.md).
