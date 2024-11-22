<<<<<<< HEAD
# controller_configuration.dispatch

## Description

An Ansible Role to run all roles on Ansible Controller.

## Requirements

ansible-galaxy collection install -r tests/collections/requirements.yml to be installed
Currently:
  awx.awx
  or
  ansible.controller

## Variables

Each role has its own variables, for information on those please see each role which this role will call. This role has one key variable `controller_configuration_dispatcher_roles` and its default value is shown below:

```yaml
controller_configuration_dispatcher_roles:
  - {role: settings, var: controller_settings, tags: settings}
  - {role: instances, var: controller_instances, tags: instances}
  - {role: instance_groups, var: controller_instance_groups, tags: instance_groups}
  - {role: organizations, var: controller_organizations, tags: organizations}
  - {role: labels, var: controller_labels, tags: labels}
  - {role: users, var: controller_user_accounts, tags: users}
  - {role: teams, var: controller_teams, tags: teams}
  - {role: credential_types, var: controller_credential_types, tags: credential_types}
  - {role: credentials, var: controller_credentials, tags: credentials}
  - {role: credential_input_sources, var: controller_credential_input_sources, tags: credential_input_sources}
  - {role: execution_environments, var: controller_execution_environments, tags: execution_environments}
  - {role: notification_templates, var: controller_notifications, tags: notification_templates}
  - {role: organizations, var: controller_organizations, tags: organizations} # Rerunning with additional dependant values set to be added to the org
  - {role: projects, var: controller_projects, tags: projects}
  - {role: inventories, var: controller_inventories, tags: inventories}
  - {role: inventory_sources, var: controller_inventory_sources, tags: inventory_sources}
  - {role: inventory_source_update, var: controller_inventory_sources, tags: inventory_sources}
  - {role: applications, var: controller_applications, tags: applications}
  - {role: hosts, var: controller_hosts, tags: hosts}
  - {role: bulk_host_create, var: controller_bulk_hosts, tags: bulk_hosts}
  - {role: groups, var: controller_groups, tags: inventories}
  - {role: job_templates, var: controller_templates, tags: job_templates}
  - {role: workflow_job_templates, var: controller_workflows, tags: workflow_job_templates}
  - {role: schedules, var: controller_schedules, tags: schedules}
  - {role: roles, var: controller_roles, tags: roles}
  - {role: job_launch, var: controller_launch_jobs, tags: job_launch}
  - {role: workflow_launch, var: controller_workflow_launch_jobs, tags: workflow_launch}
```

Note that each item has three elements:

- `role` which is the name of the role within infra.controller_configuration
=======
# infra.aap_configuration.dispatch

## Description

An Ansible Role to run all roles in the infra.aap_configuration collection.

## Variables

This is a meta role, its purpose is to run the other roles in the collection, it does not run all of them, and can be used to call roles in a custom order, The control variable is the `aap_configuration_dispatcher_roles` which will pull roles from the different services and run them, If you wish to just run a subset of services from our default lists remove entries from this var.

```yaml
aap_configuration_dispatcher_roles: >
  {{ gateway_configuration_dispatcher_roles
   + hub_configuration_dispatcher_roles
   + controller_configuration_dispatcher_roles
   + eda_configuration_dispatcher_roles
  }}
```

In addition each service has its own subset of roles, and each role has its own tag that can be used as well.

### Gateway Roles

```yaml
gateway_configuration_dispatcher_roles:
  - role: gateway_authenticators
    var: gateway_authenticators
    tags: authenticators
  - role: gateway_authenticator_maps
    var: gateway_authenticator_maps
    tags: authenticator_maps
  - role: gateway_settings
    var: gateway_settings
    tags: settings
  - role: gateway_applications
    var: aap_applications
    tags: applications
  - role: gateway_http_ports
    var: http_ports_list
    tags: http_ports
  - role: gateway_organizations
    var: aap_organizations
    tags: organizations
  - role: gateway_service_clusters
    var: gateway_service_clusters
    tags: service_clusters
  - role: gateway_service_keys
    var: gateway_service_keys
    tags: service_keys
  - role: gateway_service_nodes
    var: gateway_service_nodes
    tags: service_nodes
  - role: gateway_services
    var: gateway_services
    tags: services
  - role: gateway_role_user_assignments
    var: gateway_role_user_assignments
    tags: role_user_assignments
  - role: gateway_routes
    var: gateway_routes
    tags: routes
  - role: gateway_teams
    var: aap_teams
    tags: teams
  - role: gateway_users
    var: aap_user_accounts
    tags: teams
```

#### Hub Roles

```yaml
hub_configuration_dispatcher_roles:
  - role: hub_namespace
    var: hub_namespaces
    tags: namespaces
  - role: hub_collection
    var: hub_collections
    tags: collections
  - role: hub_ee_registry
    var: hub_ee_registries
    tags: registries
  - role: hub_ee_repository
    var: hub_ee_repositories
    tags: repos
  - role: hub_ee_repository_sync
    var: hub_ee_repository_sync
    tags: reposync
  - role: hub_ee_image
    var: hub_ee_images
    tags: images
  - role: hub_ee_registry
    var: hub_ee_registries
    tags: registry
  - role: hub_ee_registry_index
    var: hub_ee_registries
    tags: ee_indices
  - role: hub_ee_registry_sync
    var: hub_ee_registries
    tags: regsync
  - role: hub_collection_remote
    var: hub_collection_remotes
    tags: collectionremote
  - role: hub_collection_repository
    var: hub_collection_repositories
    tags: collectionsrep
  - role: hub_collection_repository_sync
    var: hub_collection_repositories
    tags: collectionsrepsync
```

#### Controller Roles

```yaml
controller_configuration_dispatcher_roles:
  - role: controller_settings
    var: controller_settings
    tags: settings
  - role: controller_organizations
    var: aap_organizations
    tags: organizations
    assign_galaxy_credentials_to_org: false
    assign_default_ee_to_org: false
    assign_notification_templates_to_org: false
  - role: controller_instances
    var: controller_instances
    tags: instances
  - role: controller_instance_groups
    var: controller_instance_groups
    tags: instance_groups
  - role: controller_labels
    var: controller_labels
    tags: labels
  - role: controller_credential_types
    var: controller_credential_types
    tags: credential_types
  - role: controller_credentials
    var: controller_credentials
    tags: credentials
  - role: controller_credential_input_sources
    var: controller_credential_input_sources
    tags: credential_input_sources
  - role: controller_execution_environments
    var: controller_execution_environments
    tags: execution_environments
  - role: controller_applications
    var: aap_applications
    tags: applications
  - role: controller_notification_templates
    var: controller_notifications
    tags: notification_templates
  - role: controller_organizations
    var: aap_organizations
    tags: organizations
    assign_galaxy_credentials_to_org: true
    assign_default_ee_to_org: true
    assign_notification_templates_to_org: true
  - role: controller_projects
    var: controller_projects
    tags:
      - inventories
      - projects
  - role: controller_inventories
    var: controller_inventories
    tags: inventories
  - role: controller_inventory_sources
    var: controller_inventory_sources
    tags:
      - inventories
      - inventory_sources
  - role: controller_inventory_source_update
    var: controller_inventory_sources
    tags:
      - inventories
      - inventory_sources
  - role: controller_hosts
    var: controller_hosts
    tags:
      - inventories
      - hosts
  - role: controller_bulk_host_create
    var: controller_bulk_hosts
    tags:
      - inventories
      - bulk_hosts
  - role: controller_host_groups
    var: controller_groups
    tags:
      - inventories
      - host_groups
  - role: controller_job_templates
    var: controller_templates
    tags: job_templates
  - role: controller_workflow_job_templates
    var: controller_workflows
    tags: workflow_job_templates
  - role: controller_schedules
    var: controller_schedules
    tags: schedules
  - role: controller_job_launch
    var: controller_launch_jobs
    tags: job_launch
  - role: controller_workflow_launch
    var: controller_workflow_launch_jobs
    tags: workflow_launch

```

#### Eda Roles

```yaml
eda_configuration_dispatcher_roles:
  - role: eda_credentials
    var: eda_credentials
    tags: credential
  - role: eda_controller_tokens
    var: eda_controller_tokens
    tags: controller_token
  - role: eda_projects
    var: eda_projects
    tags: project
  - role: eda_decision_environments
    var: eda_decision_environments
    tags: decision_environment
  - role: eda_rulebook_activations
    var: eda_rulebook_activations
    tags: rulebook_activation
```

#### Dispatch role var list keys

Each role in each service has its own variables, for information on those please see each role which this role will call. Each role is called and each item has three elements:

- `role` which is the name of the role within infra.aap_configuration
>>>>>>> 05b830566dcc61b49195ee27848426b7a0a12574
- `var` which is the variable which is used in that role. We use this to prevent the role being called if the variable is not set
- `tags` the tags which are applied to the role so it is possible to apply tags to a playbook using the dispatcher with these tags.

It is possible to redefine this variable with a subset of roles or with different tags. In general we suggest keeping the same structure and perhaps just using a subset.

<<<<<<< HEAD
### Authentication

|Variable Name|Default Value|Required|Description|Example|
|:---|:---:|:---:|:---|:---|
|`controller_state`|"present"|no|The state all objects will take unless overridden by object default|'absent'|
|`controller_hostname`|""|yes|URL to the Ansible Controller Server.|127.0.0.1|
|`controller_validate_certs`|`True`|no|Whether or not to validate the Ansible Controller Server's SSL certificate.||
|`controller_username`|""|no|Admin User on the Ansible Controller Server. Either username / password or oauthtoken need to be specified.||
|`controller_password`|""|no|Controller Admin User's password on the Ansible Controller Server. This should be stored in an Ansible Vault at vars/controller-secrets.yml or elsewhere and called from a parent playbook. Either username / password or oauthtoken need to be specified.||
|`controller_oauthtoken`|""|no|Controller Admin User's token on the Ansible Controller Server. This should be stored in an Ansible Vault at or elsewhere and called from a parent playbook. Either username / password or oauthtoken need to be specified.||
|`controller_request_timeout`|`10`|no|Specify the timeout in seconds Ansible should use in requests to the controller host.||

### Secure Logging Variables

The role defaults to False as normally most projects task does not include sensitive information.
Each role the dispatch role calls has a separate variable which can be turned on to enforce secure logging for that role but defaults to the value of controller_configuration_secure_logging if it is not explicitly called. This allows for secure logging to be toggled for the entire suite of configuration roles with a single variable, or for the user to selectively use it. If neither value is set then each role has a default value of true or false depending on the Red Hat COP suggestions.

|Variable Name|Default Value|Required|Description|
|:---:|:---:|:---:|:---:|
|`controller_configuration_secure_logging`|""|no|This variable enables secure logging as well, but is shared across multiple roles, see above.|

### Asynchronous Retry Variables

The following Variables set asynchronous retries for the role.
If neither of the retries or delay or retries are set, they will default to their respective defaults.
This allows for all items to be created, then checked that the task finishes successfully.
This also speeds up the overall role. Each individual role has its own variable which can allow the individual setting of values. See each role for more the variable names.

|Variable Name|Default Value|Required|Description|
|:---:|:---:|:---:|:---:|
|`controller_configuration_async_retries`|30|no|This variable sets the number of retries to attempt for the role globally.|
|`controller_configuration_async_delay`|1|no|This sets the delay between retries for the role globally.|

## Playbook Examples

### Standard Role Usage

```yaml
---
- name: Playbook to configure ansible controller post installation
  hosts: localhost
  connection: local
  # Define following vars here, or in controller_configs/controller_auth.yml
  # controller_hostname: ansible-controller-web-svc-test-project.example.com
  # controller_username: admin
  # controller_password: changeme
  pre_tasks:
    - name: Include vars from controller_configs directory
      ansible.builtin.include_vars:
        dir: ./yaml
        ignore_files: [controller_config.yml.template]
        extensions: ["yml"]
  roles:
    - infra.controller_configuration.dispatch
```

## License

[GPL-3.0](https://github.com/redhat-cop/controller_configuration#licensing)

## Author

[Tom Page](https://github.com/Tompage1994)
=======
For more information about variables, see [top-level README](../../README.md).
For more information about roles, see each roles' README (also linked in the top-level README)

## License

[GPL-3.0](https://github.com/redhat-cop/aap_configuration#licensing)
>>>>>>> 05b830566dcc61b49195ee27848426b7a0a12574
