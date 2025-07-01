# Configuring the Kubernetes Ingestor Backend Plugin

This guide covers the configuration options available for the Kubernetes Ingestor backend plugin.

## Configuration File

The plugin is configured through your `app-config.yaml`. Here's a comprehensive example:

```yaml
kubernetesIngestor:
  # Resource mapping configuration
  mappings:
    # How to map Kubernetes namespaces to Backstage
    namespaceModel: 'cluster' # cluster, namespace, default
    
    # How to generate entity names
    nameModel: 'name-cluster' # name-cluster, name-namespace, name-kind, name
    
    # How to generate entity titles
    titleModel: 'name' # name, name-cluster, name-namespace
    
    # How to map systems
    systemModel: 'namespace' # cluster, namespace, cluster-namespace, default
    
    # How to handle references
    referencesNamespaceModel: 'default' # default, same

  # Cluster selection
  allowedClusterNames:
    - production
    - staging

  # Component ingestion configuration
  components:
    # Enable component creation
    enabled: true
    
    taskRunner:
      # How often to query clusters (seconds)
      frequency: 10
      # Maximum processing time per cycle (seconds)
      timeout: 600
    
    # Namespaces to exclude
    excludedNamespaces:
      - kube-public
      - kube-system
    
    # Additional resource types to ingest
    customWorkloadTypes:
      - group: pkg.crossplane.io
        apiVersion: v1
        plural: providers
        # singular: provider # Optional: explicit singular form
    
    # Disable default workload types
    disableDefaultWorkloadTypes: false
    
    # Require explicit opt-in via annotations
    onlyIngestAnnotatedResources: false

  # Crossplane integration
  crossplane:
    # Enable Crossplane features
    enabled: true
    
    # Claim ingestion (v1 and v2)
    claims:
      ingestAllClaims: true
    
    # XRD handling
    xrds:
      # Template publishing configuration
      publishPhase:
        # Allowed Git hosts
        allowedTargets: ['github.com', 'gitlab.com']
        
        # Publishing target (github, gitlab, bitbucket, YAML)
        target: github
        
        # Git configuration
        git:
          repoUrl: github.com?owner=org&repo=templates
          targetBranch: main
        
        # Allow user repo selection
        allowRepoSelection: true
      
      # Enable template generation
      enabled: true
      
      taskRunner:
        frequency: 10
        timeout: 600
      
      # Require explicit opt-in
      ingestAllXRDs: true
      
      # Convert defaults to placeholders
      convertDefaultValuesToPlaceholders: true

  # Generic CRD template configuration
  genericCRDTemplates:
    publishPhase:
      allowedTargets: ['github.com', 'gitlab.com']
      target: github
      git:
        repoUrl: github.com?owner=org&repo=templates
        targetBranch: main
      allowRepoSelection: true
    
    # CRD selection
    crdLabelSelector:
      key: terasky.backstage.io/generate-form
      value: "true"
    
    # Specific CRDs to process
    crds:
      - certificates.cert-manager.io
```

## Mapping Models

### Namespace Model
Controls how Kubernetes namespaces map to Backstage:
- `cluster`: Use cluster name
- `namespace`: Use namespace name
- `default`: Use default namespace

### Name Model
Determines entity name generation:
- `name-cluster`: Combine name and cluster
- `name-namespace`: Combine name and namespace
- `name-kind`: Combine name and resource kind
- `name`: Use resource name only

### Title Model
Controls entity title generation:
- `name`: Use resource name
- `name-cluster`: Combine name and cluster
- `name-namespace`: Combine name and namespace

### System Model
Defines system mapping:
- `cluster`: Use cluster name
- `namespace`: Use namespace name
- `cluster-namespace`: Combine both
- `default`: Use default system

## Component Configuration

### Task Runner Settings
```yaml
taskRunner:
  # Run every 10 seconds
  frequency: 10
  
  # Allow up to 10 minutes per cycle
  timeout: 600
```

### Resource Type Configuration
```yaml
components:
  # Custom resource types
  customWorkloadTypes:
    - group: apps.example.com
      apiVersion: v1
      plural: applications
      singular: application
  
  # Exclude system namespaces
  excludedNamespaces:
    - kube-system
    - kube-public
```

## Crossplane Integration

### Claims Configuration
```yaml
crossplane:
  claims:
    # Auto-ingest all claims
    ingestAllClaims: true
```

### XRD Configuration
```yaml
xrds:
  # Template generation settings
  publishPhase:
    target: github
    git:
      repoUrl: github.com?owner=org&repo=templates
      targetBranch: main
  
  # Processing settings
  convertDefaultValuesToPlaceholders: true
  ingestAllXRDs: true
```

## Best Practices

1. **Resource Mapping**
   - Choose consistent mapping models
   - Use clear naming conventions
   - Consider namespace organization
   - Plan system boundaries

2. **Performance Tuning**
   - Adjust task runner frequency
   - Set appropriate timeouts
   - Configure excluded namespaces
   - Optimize resource selection

3. **Template Management**
   - Use version control
   - Maintain consistent structure
   - Document customizations
   - Test generated templates

For installation instructions, refer to the [Installation Guide](./install.md).
