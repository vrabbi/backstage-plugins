# Configuring the GitOps Manifest Updater Frontend Plugin

This guide covers the configuration options available for the GitOps Manifest Updater frontend plugin.

## Template Configuration

### Field Extension Options

The GitOpsManifestUpdater field extension accepts the following options in templates:

```yaml
manifestUpdate:
  title: Manifest Update
  type: string
  ui:field: GitOpsManifestUpdater
  ui:options:
    # Required: Git repository URL
    repositoryUrl: string
    
    # Optional: Branch to create/update (default: main)
    branch: string
    
    # Optional: Path to manifest files
    path: string
    
    # Optional: Pull request settings
    pullRequest:
      title: string
      description: string
      labels: string[]
      
    # Optional: Schema settings
    schema:
      validate: boolean
      strict: boolean
```

### Example Configurations

#### Basic Configuration
```yaml
apiVersion: scaffolder.backstage.io/v1beta3
kind: Template
metadata:
  name: update-manifest
  title: Update Kubernetes Manifest
spec:
  parameters:
    - title: Update Manifest
      properties:
        manifestUpdate:
          title: Manifest Update
          type: string
          ui:field: GitOpsManifestUpdater
          ui:options:
            repositoryUrl: ${{ parameters.repoUrl }}
            branch: main
            path: manifests/
```

#### Advanced Configuration
```yaml
apiVersion: scaffolder.backstage.io/v1beta3
kind: Template
metadata:
  name: update-crossplane-claim
  title: Update Crossplane Claim
spec:
  parameters:
    - title: Update Claim
      properties:
        claimUpdate:
          title: Claim Configuration
          type: string
          ui:field: GitOpsManifestUpdater
          ui:options:
            repositoryUrl: ${{ parameters.repoUrl }}
            branch: update-claim
            path: claims/
            pullRequest:
              title: "Update Crossplane claim configuration"
              description: "Updates resource requirements and settings"
              labels: ["crossplane", "config-update"]
            schema:
              validate: true
              strict: true
```

## Component Configuration

### GitOpsManifestUpdaterExtension Props

Configure the extension component behavior:

```typescript
interface GitOpsManifestUpdaterProps {
  // Optional: Default repository URL
  defaultRepositoryUrl?: string;
  
  // Optional: Default branch name
  defaultBranch?: string;
  
  // Optional: Default file path
  defaultPath?: string;
  
  // Optional: Custom schema validator
  schemaValidator?: (schema: any, value: any) => boolean;
  
  // Optional: Custom PR creator
  pullRequestCreator?: (options: PROptions) => Promise<void>;
}
```

### Integration Examples

#### Basic Integration
```typescript
<ScaffolderFieldExtensions>
  <GitOpsManifestUpdaterExtension />
</ScaffolderFieldExtensions>
```

#### Advanced Integration
```typescript
<ScaffolderFieldExtensions>
  <GitOpsManifestUpdaterExtension
    defaultBranch="develop"
    defaultPath="k8s/"
    schemaValidator={customValidator}
    pullRequestCreator={customPRCreator}
  />
</ScaffolderFieldExtensions>
```

## Template Examples

### Kubernetes Deployment Update
```yaml
apiVersion: scaffolder.backstage.io/v1beta3
kind: Template
metadata:
  name: update-deployment
  title: Update Kubernetes Deployment
spec:
  parameters:
    - title: Update Deployment
      properties:
        deployment:
          title: Deployment Configuration
          type: string
          ui:field: GitOpsManifestUpdater
          ui:options:
            repositoryUrl: ${{ parameters.repoUrl }}
            branch: update-deployment
            path: deployments/
            pullRequest:
              title: "Update deployment configuration"
              description: |
                Updates deployment configuration with:
                - Resource requirements
                - Environment variables
                - Container settings
              labels: ["deployment", "config"]
```

### Crossplane Resource Update
```yaml
apiVersion: scaffolder.backstage.io/v1beta3
kind: Template
metadata:
  name: update-xr
  title: Update Crossplane Resource
spec:
  parameters:
    - title: Update XR
      properties:
        xr:
          title: Crossplane Resource
          type: string
          ui:field: GitOpsManifestUpdater
          ui:options:
            repositoryUrl: ${{ parameters.repoUrl }}
            branch: update-xr
            path: crossplane/
            schema:
              validate: true
              strict: true
            pullRequest:
              title: "Update Crossplane resource"
              labels: ["crossplane", "infrastructure"]
```

## Best Practices

1. **Template Design**
   - Use clear, descriptive titles
   - Provide helpful descriptions
   - Set appropriate default values
   - Include validation rules

2. **Repository Structure**
   - Organize manifests logically
   - Use consistent file paths
   - Follow GitOps practices
   - Maintain clear documentation

3. **Pull Requests**
   - Use descriptive titles
   - Provide detailed descriptions
   - Apply appropriate labels
   - Follow team conventions

For installation instructions, refer to the [Installation Guide](./install.md).
