# Scaffolder Backend Module TeraSky Utils Backend Plugin

[![npm latest version](https://img.shields.io/npm/v/@terasky/backstage-plugin-scaffolder-backend-module-terasky-utils/latest.svg)](https://www.npmjs.com/package/@terasky/backstage-plugin-scaffolder-backend-module-terasky-utils)

## Overview

The Scaffolder Backend Module TeraSky Utils backend plugin extends Backstage's scaffolder with powerful actions for managing Kubernetes resources and Backstage entities. It provides specialized actions for generating Crossplane claims and cleaning up catalog entities.

## Features

### Claim Template Action
- Parameter to YAML conversion
- Structured file organization
- Crossplane integration
- Resource management

### Catalog Info Cleaner
- Entity manifest cleanup
- Runtime data removal
- Git preparation
- YAML formatting

## Components

### terasky:claim-template
The action that provides:
- Parameter processing
- YAML generation
- File system handling
- Resource organization

Example usage:
```yaml
apiVersion: scaffolder.backstage.io/v1beta3
kind: Template
metadata:
  name: crossplane-claim
spec:
  steps:
    - id: generate-claim
      name: Generate Crossplane Claim
      action: terasky:claim-template
      input:
        cluster: production
        namespace: web-apps
        kind: PostgreSQLInstance
        parameters:
          size: small
          version: "13"
```

### terasky:catalog-info-cleaner
The action that handles:
- Entity processing
- Data cleanup
- File generation
- Format standardization

Example usage:
```yaml
apiVersion: scaffolder.backstage.io/v1beta3
kind: Template
metadata:
  name: catalog-cleanup
spec:
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
```

## Technical Details

### Action Processing
The plugin processes actions through:
1. Input validation
2. Data transformation
3. File generation
4. Output organization

### File System Handling
Manages file operations for:
- Manifest creation
- Directory structure
- File naming
- Path organization

### Integration Points
- Backstage Scaffolder
- Kubernetes API
- Entity Catalog
- File System

## Use Cases

### Crossplane Claim Generation
1. Collect parameters
2. Generate manifest
3. Organize files
4. Prepare deployment

### Entity Cleanup
1. Process entity
2. Remove runtime data
3. Format manifest
4. Save to file

## Example Templates

### Crossplane Resource Template
```yaml
apiVersion: scaffolder.backstage.io/v1beta3
kind: Template
metadata:
  name: database-claim
  title: Create Database Claim
spec:
  parameters:
    - title: Database Configuration
      properties:
        name:
          title: Name
          type: string
        size:
          title: Size
          type: string
          enum: [small, medium, large]
        version:
          title: Version
          type: string
          default: "13"
  
  steps:
    - id: generate-claim
      name: Generate Database Claim
      action: terasky:claim-template
      input:
        cluster: ${{ parameters.cluster }}
        namespace: ${{ parameters.namespace }}
        kind: PostgreSQLInstance
        parameters:
          name: ${{ parameters.name }}
          size: ${{ parameters.size }}
          version: ${{ parameters.version }}
```

### Entity Cleanup Template
```yaml
apiVersion: scaffolder.backstage.io/v1beta3
kind: Template
metadata:
  name: git-entity-prep
  title: Prepare Entity for Git
spec:
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
    
    - id: publish
      name: Publish to Git
      action: publish:github
      input:
        path: ./catalog-info.yaml
        repoUrl: ${{ parameters.repoUrl }}
```

For installation and configuration details, refer to the [Installation Guide](./install.md) and [Configuration Guide](./configure.md). 