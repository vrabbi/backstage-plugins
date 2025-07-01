# Educates Frontend Plugin

[![npm latest version](https://img.shields.io/npm/v/@terasky/backstage-plugin-educates/latest.svg)](https://www.npmjs.com/package/@terasky/backstage-plugin-educates)

## Overview

The Educates frontend plugin provides a comprehensive interface for discovering and accessing educational workshops within Backstage. It enables users to browse workshops from multiple training portals, view detailed information, and manage workshop sessions, all integrated seamlessly into the Backstage interface.

## Features

### Workshop Discovery
- Browse available workshops across multiple portals
- Filter and search capabilities
- Workshop categorization and tagging
- Multi-portal support

### Workshop Details
- Comprehensive workshop information:
  - Title and description
  - Difficulty level
  - Duration estimates
  - Tags and labels
  - Capacity information
  - Availability status

### Session Management
- Launch workshops in new browser tabs
- Track active workshop sessions
- Monitor session status
- Session cleanup capabilities

### User Interface
- Material design integration
- Responsive layout
- Intuitive navigation
- Consistent Backstage styling

## Components

### EducatesPage
The main page component that provides:
- Workshop catalog view
- Portal selection
- Session management interface

### Workshop Cards
Individual workshop displays showing:
- Workshop title and description
- Key metadata
- Launch options
- Status indicators

### Portal Selection
Interface for managing multiple training portals:
- Portal switching
- Portal-specific workshop lists
- Portal status indicators

## Technical Details

### Integration Points
- Backstage core platform
- Educates backend plugin
- Training portal APIs
- Permission framework

### Permission Framework
Built-in support for Backstage's permission system:
- `educates.workshops.view`: Required for viewing workshops
- `educates.workshop-sessions.create`: Required for launching sessions

### Type Definitions
Utilizes shared types from the common package:
- `Workshop`
- `WorkshopEnvironment`
- `TrainingPortalStatus`
- `WorkshopSession`

## User Experience

### Workshop Discovery
1. Navigate to the Workshops page
2. Browse available workshops
3. Filter by portal, tags, or difficulty
4. View detailed workshop information

### Workshop Launch
1. Select desired workshop
2. Review workshop details
3. Click launch button
4. Access workshop in new browser tab

### Session Management
1. View active sessions
2. Monitor session status
3. End sessions when complete
4. Track session history
