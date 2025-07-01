# VCF Automation Plugin

The VCF Automation plugin provides comprehensive visibility and management capabilities for VMware Cloud Foundation (VCF) deployments within Backstage. It enables teams to monitor deployments, manage resources, and oversee project configurations through an intuitive interface.

## Features

- **Deployment Monitoring**: Track VCF deployment status and operations
- **Resource Management**: Manage vSphere VMs and other VCF resources
- **Project Configuration**: Configure and monitor VCF projects
- **Permission Controls**: Fine-grained access control integration
- **Entity Integration**: Seamless catalog entity synchronization
- **Status Tracking**: Real-time deployment and resource status

## Plugin Components

### Frontend Plugin
The plugin provides frontend components for:
- Deployment visualization
- Resource management
- Project configuration
- Status monitoring

[Learn more about the frontend plugin](./frontend/about.md)

### Backend Plugin
The plugin requires a backend deployment that:
- Handles API integration
- Manages permissions
- Processes operations
- Tracks resources

[Learn more about the backend plugin](./backend/about.md)

## Available Components

### Deployment Components
- `VCFAutomationDeploymentOverview`: High-level deployment status
- `VCFAutomationDeploymentDetails`: Detailed deployment information

### VM Components
- `VCFAutomationVSphereVMOverview`: VM status overview
- `VCFAutomationVSphereVMDetails`: Detailed VM configuration

### Resource Components
- `VCFAutomationGenericResourceOverview`: Resource status
- `VCFAutomationGenericResourceDetails`: Resource configuration

### Project Components
- `VCFAutomationProjectOverview`: Project status overview
- `VCFAutomationProjectDetails`: Project configuration details

## Documentation Structure

- Frontend Plugin
  - [About](./frontend/about.md)
  - [Installation](./frontend/install.md)
  - [Configuration](./frontend/configure.md)
- Backend Plugin
  - [About](./backend/about.md)
  - [Installation](./backend/install.md)
  - [Configuration](./backend/configure.md)

## Prerequisites

Before getting started, ensure you have:

1. VCF Automation Backend Plugin installed
2. VCF Ingestor Plugin configured
3. Access to VCF deployments
4. Proper permissions setup

## Getting Started

To get started with the VCF Automation plugin:

1. Install frontend and backend plugins
2. Configure API integration
3. Set up entity synchronization
4. Configure permissions
5. Add components to entity pages

For detailed installation and configuration instructions, refer to the frontend and backend documentation linked above.
