# Installing the GitOps Manifest Updater Frontend Plugin

This guide will help you install and set up the GitOps Manifest Updater frontend plugin in your Backstage instance.

## Prerequisites

Before installing the plugin, ensure you have:

1. A working Backstage instance
2. The Scaffolder plugin installed and configured
3. Access to GitHub repositories
4. Kubernetes CRDs with OpenAPI schemas

## Installation Steps

### 1. Add the Package

Install the plugin package using yarn:

```bash
yarn --cwd packages/app add @terasky/backstage-plugin-gitops-manifest-updater
```

### 2. Add to Scaffolder Route

Modify your app routes in `packages/app/src/App.tsx`:

```typescript
import { GitOpsManifestUpdaterExtension } from '@terasky/backstage-plugin-gitops-manifest-updater';
import { ScaffolderFieldExtensions } from '@backstage/plugin-scaffolder-react';

const routes = (
  <FlatRoutes>
    {/* ... other routes ... */}
    <Route path="/create" element={<ScaffolderPage />}>
      <ScaffolderFieldExtensions>
        <GitOpsManifestUpdaterExtension />
      </ScaffolderFieldExtensions>
    </Route>
  </FlatRoutes>
);
```

### 3. Add to Entity Pages

Add the plugin to your entity pages in `packages/app/src/components/catalog/EntityPage.tsx`:

```typescript
import { GitOpsManifestUpdaterExtension } from '@terasky/backstage-plugin-gitops-manifest-updater';
import { ScaffolderFieldExtensions } from '@backstage/plugin-scaffolder-react';

const serviceEntityPage = (
  <EntityLayout>
    <EntityLayout.Route path="/scaffolder" title="Entity Scaffolder">
      <EntityScaffolderContent
        templateGroupFilters={[
          {
            title: 'Management Templates',
            filter: (entity, template) =>
              template.metadata?.labels?.target === 'component',
          },
        ]}
        buildInitialState={entity => ({
          entity: stringifyEntityRef(entity)
        })}
        ScaffolderFieldExtensions={
          <ScaffolderFieldExtensions>
            <RepoUrlPickerFieldExtension />
            <EntityPickerFieldExtension />
            <GitOpsManifestUpdaterExtension />
          </ScaffolderFieldExtensions>
        }
      />
    </EntityLayout.Route>
  </EntityLayout>
);
```

### 4. Add Example Template

Add the example template to your templates directory:

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

## Verification

After installation, verify that:

1. The plugin appears in your package.json dependencies
2. The GitOpsManifestUpdater field is available in templates
3. Forms are generated correctly from CRD schemas
4. Pull requests are created successfully

## Troubleshooting

Common issues and solutions:

1. **Field Not Available**
   - Verify scaffolder field extension registration
   - Check component imports
   - Ensure template configuration is correct

2. **Schema Loading Issues**
   - Verify CRD accessibility
   - Check OpenAPI schema format
   - Review error messages in browser console

3. **PR Creation Problems**
   - Check GitHub token permissions
   - Verify repository access
   - Review branch protection rules

For configuration options and customization, proceed to the [Configuration Guide](./configure.md).
