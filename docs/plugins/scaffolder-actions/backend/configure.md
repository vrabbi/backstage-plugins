# Configuring the Scaffolder Backend Module TeraSky Utils Backend Plugin

This guide covers the configuration options available for the Scaffolder Backend Module TeraSky Utils backend plugin.

## Action Configuration

### terasky:claim-template

The claim template action accepts the following inputs:

```yaml
input:
  # Required: Target cluster name
  cluster: string
  
  # Required: Target namespace
  namespace: string
  
  # Required: Resource kind
  kind: string
  
  # Required: Resource parameters
  parameters: object
  
  # Optional: Resource API version
  apiVersion: string
  
  # Optional: Resource name
  name: string
  
  # Optional: Output file path
  outputPath: string
```

Example configuration:
```yaml
steps:
  - id: generate-claim
    name: Generate Crossplane Claim
    action: terasky:claim-template
    input:
      cluster: production
      namespace: web-apps
      kind: PostgreSQLInstance
      apiVersion: database.example.org/v1alpha1
      name: my-database
      parameters:
        size: small
        version: "13"
        storageGB: 100
```

### terasky:catalog-info-cleaner

The catalog info cleaner action accepts the following inputs:

```yaml
input:
  # Required: Entity to clean
  entity: object
  
  # Optional: Output file name
  outputFileName: string
  
  # Optional: Fields to preserve
  preserveFields: string[]
  
  # Optional: Additional fields to remove
  removeFields: string[]
```

Example configuration:
```yaml
steps:
  - id: clean-entity
    name: Clean Entity Manifest
    action: terasky:catalog-info-cleaner
    input:
      entity:
        apiVersion: backstage.io/v1alpha1
        kind: Component
        metadata:
          name: example-service
      outputFileName: catalog-info.yaml
      preserveFields:
        - metadata.annotations.github.com/project-slug
      removeFields:
        - metadata.annotations.backstage.io/managed-by-location
```

## Template Examples

### Database Claim Template

```yaml
apiVersion: scaffolder.backstage.io/v1beta3
kind: Template
metadata:
  name: database-claim
  title: Create Database Instance
  description: Creates a Crossplane claim for a database instance
spec:
  owner: platform-team
  type: service
  
  parameters:
    - title: Database Configuration
      required:
        - name
        - size
      properties:
        name:
          title: Name
          type: string
          description: Name of the database instance
        size:
          title: Size
          type: string
          description: Size of the database instance
          enum:
            - small
            - medium
            - large
        version:
          title: Version
          type: string
          description: Database version
          default: "13"
  
  steps:
    - id: generate-claim
      name: Generate Database Claim
      action: terasky:claim-template
      input:
        cluster: ${{ parameters.cluster | default "production" }}
        namespace: ${{ parameters.namespace | default "databases" }}
        kind: PostgreSQLInstance
        name: ${{ parameters.name }}
        parameters:
          size: ${{ parameters.size }}
          version: ${{ parameters.version }}
    
    - id: publish
      name: Publish to Git
      action: publish:github
      input:
        repoUrl: github.com?owner=org&repo=infrastructure
        branchName: create-db-${{ parameters.name }}
        title: Create database ${{ parameters.name }}
        description: |
          Creates a new PostgreSQL database instance:
          - Name: ${{ parameters.name }}
          - Size: ${{ parameters.size }}
          - Version: ${{ parameters.version }}
```

### Entity Cleanup Template

```yaml
apiVersion: scaffolder.backstage.io/v1beta3
kind: Template
metadata:
  name: entity-cleanup
  title: Clean Entity for Git
  description: Prepares an auto-ingested entity for git-based management
spec:
  owner: platform-team
  type: service
  
  parameters:
    - title: Entity Selection
      required:
        - entityRef
      properties:
        entityRef:
          title: Entity Reference
          type: string
          description: Reference to the entity to clean
        repoUrl:
          title: Repository URL
          type: string
          description: Where to publish the cleaned entity
  
  steps:
    - id: fetch-entity
      name: Fetch Current Entity
      action: catalog:fetch
      input:
        entityRef: ${{ parameters.entityRef }}
    
    - id: clean-entity
      name: Clean Entity Manifest
      action: terasky:catalog-info-cleaner
      input:
        entity: ${{ steps.fetch-entity.output.entity }}
        preserveFields:
          - metadata.annotations.github.com/project-slug
          - metadata.annotations.backstage.io/techdocs-ref
        removeFields:
          - metadata.annotations.backstage.io/managed-by-location
          - metadata.annotations.backstage.io/managed-by-origin-location
    
    - id: publish
      name: Publish to Git
      action: publish:github
      input:
        repoUrl: ${{ parameters.repoUrl }}
        branchName: migrate-${{ parameters.entityRef | replace(':', '-') }}
        title: Migrate ${{ parameters.entityRef }} to git
        description: |
          Migrates the auto-ingested entity to git-based management.
          Entity: ${{ parameters.entityRef }}
```

## Best Practices

1. **Claim Template Usage**
   - Use descriptive names
   - Validate parameters
   - Set default values
   - Handle errors gracefully

2. **Entity Cleanup**
   - Preserve essential fields
   - Remove runtime data
   - Maintain relationships
   - Document changes

3. **Template Organization**
   - Group related templates
   - Use consistent naming
   - Add descriptions
   - Include examples

For installation instructions, refer to the [Installation Guide](./install.md). 