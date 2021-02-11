# tower_configuration
![Ansible Lint](https://github.com/redhat-cop/tower_configuration/workflows/Ansible%20Lint/badge.svg)
![Galaxy Release](https://github.com/redhat-cop/tower_configuration/workflows/galaxy-release/badge.svg)
<!-- Further CI badges go here as above -->

<!-- Describe the collection and why a user would want to use it. What does the collection do? -->

This is a collection of roles for AWX/Ansible Tower.

## Release Process
This collection uses an auatomated GitHub workflow to publish releases to Ansible Galaxy. This workflow can be found in `.github/workflows/galaxy-release.yml`. It is dependent on `release.yml` and `galaxy.yml.j2`. See instructions below for usage.

To publish a release to Galaxy:
1) An administrator of the repository must configure a secret in the settings containing the API key for the Galaxy namespace. The secret should be called `ANSIBLE_GALAXY_APIKEY`. This is a one time step.
2) To publish a release, click "releases" at the top of repository home page and "Draft a new release"
3) The "Tag version" should be in the form `v#.#.#`. A suffix may be added such as `-dev#` or `rc#` for pre-releases. These will still publish to Galaxy but will not be chosen when installing `latest`. The version must be explicitly stated when installing to use pre-releases.
4) Title the release, it is recommended to use the "Tag version"
5) Clicking "Publish release" will initiate the workflow. If there are no errors building the collection, it will be automatically published to galaxy as the version suppied for "Tag version"

## Tested with Ansible

<!-- List the versions of Ansible the collection has been tested with. Must match what is in galaxy.yml. -->

## External requirements

This collection depends on the awx.collection since it uses the modules there.

### Supported connections
<!-- Optional. If your collection supports only specific connection types (such as HTTPAPI, netconf, or others), list them here. -->

## Included content

<!-- Galaxy will eventually list the module docs within the UI, but until that is ready, you may need to either describe your plugins etc here, or point to an external docsite to cover that information. -->

## Using this collection
The awx.awx or ansible.tower collection must be invoked in the playbook in order for ansible to pick up the correct modules to use.

Otherwise it will look for the modules only in your base installation. If there are errors complaining about "couldn't resolve module/action" this is the most likely cause.

```yaml
- name: Playbook to configure ansible tower post installation
  hosts: localhost
  connection: local
  vars:
    tower_validate_certs: false
  collections:
    - awx.awx
```

Define following vars here, or in `tower_configs/tower_auth.yml`
`tower_hostname: ansible-tower-web-svc-test-project.example.com`

<!--Include some quick examples that cover the most common use cases for your collection content. -->

 - tower_hostname, tower_username, tower_password
 - tower_hostname, tower_oauthtoken

The OAuth2 token is the preferred method. You can obtain the token through the prefered tower_token module, or through the
AWX CLI [login](https://docs.ansible.com/ansible-tower/latest/html/towercli/reference.html#awx-login)
command.

These can be specified via (from highest to lowest precedence):

 - direct role variables as mentioned above
 - environment variables (most useful when running against localhost)
 - a config file path specified by the `tower_config_file` parameter
 - a config file at `~/.tower_cli.cfg`
 - a config file at `/etc/tower/tower_cli.cfg`

Config file syntax looks like this:

```
[general]
host = https://localhost:8043
verify_ssl = true
oauth_token = LEdCpKVKc4znzffcpQL5vLG8oyeku6
```

Tower token module would be invoked with this code:
```yaml
    - name: Create a new token using tower username/password
      awx.awx.tower_token:
        description: 'Creating token to test tower jobs'
        scope: "write"
        state: present
        tower_host: "{{ tower_hostname }}"
        tower_username: "{{ tower_username }}"
        tower_password: "{{ tower_password }}"

```

### Tower Export
The awx command line can export json that is compatable with this collection.
More details can be found [here](playbooks/tower_configs_export_model/README.md)

### See Also:

* [Ansible Using collections](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html) for more details.

## Release and Upgrade Notes
Notable releases of the `awx.awx` collection:
 - 0.1.0 Initial Release Allows for a single json structure to import all modules in the awx.awx collection.

## Roadmap
Adding the ability to use direct output from the awx export command in the roles along with the current data model.

## Contributing to this collection

<!--Describe how the community can contribute to your collection. At a minimum, include how and where users can create issues to report problems or request features for this collection.  List contribution requirements, including preferred workflows and necessary testing, so you can benefit from community PRs. If you are following general Ansible contributor guidelines, you can link to - [Ansible Community Guide](https://docs.ansible.com/ansible/latest/community/index.html). -->


## Release notes
<!--Add a link to a changelog.md file or an external docsite to cover this information. -->

## Roadmap

<!-- Optional. Include the roadmap for this collection, and the proposed release/versioning strategy so users can anticipate the upgrade/update cycle. -->

## More information

<!-- List out where the user can find additional information, such as working group meeting times, slack/IRC channels, or documentation for the product this collection automates. At a minimum, link to: -->

- [Ansible Collection overview](https://github.com/ansible-collections/overview)
- [Ansible User guide](https://docs.ansible.com/ansible/latest/user_guide/index.html)
- [Ansible Developer guide](https://docs.ansible.com/ansible/latest/dev_guide/index.html)
- [Ansible Community code of conduct](https://docs.ansible.com/ansible/latest/community/code_of_conduct.html)

## Licensing

<!-- Include the appropriate license information here and a pointer to the full licensing details. If the collection contains modules migrated from the ansible/ansible repo, you must use the same license that existed in the ansible/ansible repo. See the GNU license example below. -->

GNU General Public License v3.0 or later.

See [LICENCE](https://www.gnu.org/licenses/gpl-3.0.txt) to see the full text.
