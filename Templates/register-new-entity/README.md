# Register New Entity Template

## Outline
The template allows for quickly and efficiently registering a repository to the Roadie Backstage catalog by adding and configuring the `catalog-info.yaml` with the recommended data points necessary for the entity to be listed in the catalog, as well as enabling TechDocs. Optionally, you are able to configure additional integrations such as Jenkins, Sentry, or SonarQube.

## Inputs
1. Collect general entity information:
   - **Kind**: \[Required\] Whether the entity is a System, Component, Resource, API, or TBC (placeholder for Tooling).
   - **Name**: \[Required\] The name of the entity being registered, typically the same name as the repo, in kebab-case.
   - **Title**: \[Optional\] Title for the entity which will take the place of the name in the catalog list. For example, if a component with the name `fancy-widget-nodejs` has the title `Fancy Widget`, then the name will appear as `Fancy Widget`.
   - **Description**: \[Required\] This should be used to provided detailed information in the catalog list.
   - **Owner**: \[Required\] Team or User who owns the entity.
   - **Tags**: \[Optional\] Any relevant tags to help classify catalog entries.
   - **Labels**: \[Optional\] A key-value pair attached to the entity mainly to reference other entities and specify identifying attributes. The use is identical to [Kubernetes object labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/).
2. Collect kind-specific entity details:
   - System:
      - **CMDB-ID**: \[Optional\] Service Now ID for the Business Service.
   - Component:
      - **Component Type**: \[Required\] The type of component the entity is.
      - **Lifecycle**: \[Required\] The maturity and support of an entity.
      - **Product Owner**: \[Optional\] User who owns the entity.
      - **System**: \[Optional\] What system the component belongs to.
      - **Subcomponent of**: \[Optional\] If this is part of another component (not system).
      - **Depends on**: \[Optional\] What dependencies the component has.
      - **Provides APIs**: \[Optional\] APIs provided by this component.
      - **Consumes APIs**: \[Optional\] APIs consumed by this component.
   - Resource:
      - **Resource Type**: \[Required\] The type of resource the entity is.
      - **Lifecycle**: \[Required\] The maturity and support of an entity.
      - **Subcomponent of**: \[Optional\] If this is part of another component (not system).
      - **Depends on**: \[Optional\] What dependencies the resource has.
      - **Provides APIs**: \[Optional\] APIs provided by this resource.
      - **Consumes APIs**: \[Optional\] APIs consumed by this resource.
   - API:
      - **Lifecycle**: \[Required\] The maturity and support of an entity.
      - **API Type**: \[Required\] The definition type of the API.
      - **API definition**: \[Required\] Provide a URL to the API spec or paste the spec directly into a text area.
   - TBC:
      - **TBC Type**: \[Required\] The type of tool the entity is.
      - **Lifecycle**: \[Required\] The maturity and support of an entity.
2. Collect entity location:
   - **Repo Host**: \[Required\] Hardcoded and hidden, this is used in building the pull request. Alternatively, you may expose this as a select list [example below].
   - **Repo Name**: \[Required\] The name of the repository to be registered in Roadie Backstage. The list is populated through the Roadie GitHub Proxy and requires a GitHub classic API Token to be configured in your Roadie Secrets.
   - **Links**: \[Optional\] A list of external hyperlinks related to the entity, such as links to the component itself.
      - **URL**: \[Required\] Standard URI format, e.g. https://example.com/path/somewhere
      - **Title**: \[Optional\] User-friendly display name for the link.
      - **Icon**: \[Optional\] Keyword representing the visual icon to be displayed in the UI.
      - **Type**: \[Optional\] Value to categorize links into specific groups.
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
     - Conditionally: Enables any integrations by adding the relevant annotations, if necessary data is provided.
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