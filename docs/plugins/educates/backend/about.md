# Educates Backend Plugin

[![npm latest version](https://img.shields.io/npm/v/@terasky/backstage-plugin-educates-backend/latest.svg)](https://www.npmjs.com/package/@terasky/backstage-plugin-educates-backend)

## Overview

The Educates backend plugin provides the server-side functionality required to integrate Educates training portals with Backstage. It handles API communication, authentication, session management, and exposes endpoints for the frontend plugin to consume.

## Features

### API Integration
- Seamless communication with Educates training portals
- Support for multiple portal configurations
- Secure API token management
- Error handling and retries

### Authentication Management
- Token-based authentication
- Automatic token refresh
- Secure credential storage
- Session persistence

### Workshop Management
- Workshop catalog retrieval
- Workshop metadata handling
- Session creation and tracking
- Resource cleanup

### Multi-Portal Support
- Multiple portal configurations
- Portal health monitoring
- Load balancing capabilities
- Portal-specific settings

## API Endpoints

The plugin exposes the following REST endpoints:

### Training Portals
```
GET /api/educates/training-portals
```
Lists all configured training portals and their status.

### Workshops
```
GET /api/educates/training-portals/:portalName/workshops
```
Retrieves the workshop catalog from a specific portal.

```
GET /api/educates/training-portals/:portalName/workshops/:workshopName
```
Gets detailed information about a specific workshop.

### Sessions
```
POST /api/educates/training-portals/:portalName/workshops/:workshopName/sessions
```
Creates a new workshop session.

```
GET /api/educates/training-portals/:portalName/workshops/:workshopName/sessions/:sessionId
```
Retrieves the status of a specific session.

```
DELETE /api/educates/training-portals/:portalName/workshops/:workshopName/sessions/:sessionId
```
Terminates a workshop session.

## Technical Details

### Integration Points
- Educates Training Portal API
- Backstage backend services
- Permission framework
- Authentication system

### Type Definitions
Utilizes shared types from the common package:
- `TrainingPortalConfig`
- `EducatesConfig`
- `Workshop`
- `WorkshopEnvironment`
- `TrainingPortalStatus`
- `WorkshopSession`

### Error Handling
- Comprehensive error types
- Detailed error messages
- Automatic retries
- Rate limiting protection

### Security
- Secure credential management
- Token-based authentication
- Permission-based access control
- Request validation

## Architecture

### Components
1. **API Router**
   - Endpoint registration
   - Request handling
   - Response formatting

2. **Portal Manager**
   - Portal configuration
   - Health monitoring
   - Connection management

3. **Session Controller**
   - Session lifecycle
   - Resource allocation
   - Cleanup processes

4. **Authentication Handler**
   - Token management
   - Credential storage
   - Permission checks

### Data Flow
1. Request received from frontend
2. Authentication and permission validation
3. Portal communication
4. Response processing
5. Result returned to client

## Integration Examples

### Portal Communication
```typescript
async function getWorkshops(portalName: string): Promise<Workshop[]> {
  const portal = await portalManager.getPortal(portalName);
  return portal.listWorkshops();
}
```

### Session Management
```typescript
async function createSession(
  portalName: string,
  workshopName: string
): Promise<WorkshopSession> {
  const portal = await portalManager.getPortal(portalName);
  return portal.createSession(workshopName);
}
```

For installation and configuration details, refer to the [Installation Guide](./install.md) and [Configuration Guide](./configure.md).
