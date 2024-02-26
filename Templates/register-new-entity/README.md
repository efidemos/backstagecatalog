# Register New Entity Template

## Outline
The template allows for quickly and efficiently registering a repository to the Roadie Backstage catalog by adding and configuring the `catalog-info.yaml` with the recommended data points necessary for the entity to be listed in the catalog, as well as enabling TechDocs. Optionally, you are able to configure additional integrations such as Jenkins, Sentry, or SonarQube.

## Inputs
1. Collect general entity information:
   - Kind: Whether the entity is a System, Component, Resource, API, or TBC (placeholder for Tooling).
   - Name: The name of the entity we are registering, typically the same name as the repo and kebab-case.
   - Title: An optional title for the entity which will take the place of the name in the catalog list. For example, if a component with the name `fancy-widget-nodejs` has the title `Fancy Widget`, then the name will appear as `Fancy Widget`
   - Description: This should be used to provided detailed information in the catalog list.
   - Lifecycle: The maturity and support of an entity.
   - Owner: Team or User who owns the entity.
   - Tags: Any relevant tags to help classify catalog entries.
   - Labels: References to other entities. (?)
2. Collect kind-specific entity details:
   - System:
      - CMDB-ID: Service Now ID for the Business Service
   - Component:
      - Component Type: The type of component the entity is.
      - Product Owner: User who owns the entity.
      - System: What system the component belongs to.
      - Subcomponent of: If this is part of another component (not system).
      - Depends on: What dependencies the component has.
      - Provides APIs: APIs provided by this component.
      - Consumes APIs: APIs consumed by this component.
   - Resource:
      - Resource Type: The type of resource the entity is.
      - Subcomponent of: If this is part of another component (not system).
      - Depends on: What dependencies the component has.
      - Provides APIs: APIs provided by this component.
      - Consumes APIs: APIs consumed by this component.
   - API:
      - API Type: The definition type of the API.
      - API definition: Provide a URL to the API spec or paste the spec directly into a text area.
   - TBC:
      - TBC Type: The type of tool the entity is.
2. Collect entity location:
   - Repo Host: Hardcoded and hidden, this is used in building the pull request. Alternatively, you may expose this as a select list [example below].
   - Repo Name: The name of the repository to be registered in Roadie Backstage. The list is populated through the Roadie GitHub Proxy and requires a GitHub classic API Token to be configured in your Roadie Secrets.
3. Collect integration configuration data:
   - Configuration Values: Set the integration specific information, such as Jenkins job name, Sentry project slug, or SonarQube key.
   - TechDocs Path: Set the directory path to be used. The Default is `docs:.`, but `url:{url}` is supported.

Repo Host Example:
```
   repoUrl:
      title: Repository Location
      type: string
      ui:field: RepoUrlPicker
      ui:options:
        allowedHosts:
          - github.com
```

## Outputs
- Creates a pull request to add the following additions to the repository:
  1. Creates the `catalog-info.yaml` file for registration in the catalog.
     - Conditionally: Enables any integrations if necessary data is provided.
  2. Creates the required files for TechDocs if `Enable TechDocs` is checked:
     - `MKDocs.yaml`: The configuration file for the repositories TechDocs.
     - `/docs/index.md`: The index page for the repositories TechDocs.

## TODO's

### GitHub Proxy
- [ ] In GitHub, navigate to `Settings > Developer Settings > Personal access tokens > Tokens (classic)`. Press **Generate new token Classic**.
- [ ] In Roadie, navigate to `Administration > Settings > Roadie Settings | Secrets`. Press the pencil icon to edit the `GITHUB_TOKEN` and enter the generated token.

### Template Details
- [ ] `Spec > Owner`: Set the owner, most commonly the same person/group who owns the template.

### Component Details
Roadie Relationship.
- [ ] `type`: Confirm the `default` and the types available are options you support.
- [ ] `lifecycle`: Confirm the `default` and the lifecycles available are options you support.
- [ ] `owner`: Will the component be owned by an individual, group or both?

### Component Location
- [ ] `repoOrg`: Set your GitHub organization name.

### Integrations
- [ ] `properties`: Configure a property for each integration you would like to support enablement at registrations here.