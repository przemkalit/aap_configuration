# controller_configuration.filetree_create

The role `filetree_create` is intended to be used as the first step to begin using the Configuration as Code on Ansible Tower or Ansible Automation Platform, when you already have a running instance of any of them. Obviously, you also could start to write your objects as code from scratch, but the idea behind the creation of that role is to simplify your lives and make that task a little bit easier.

## Requirements

That role requires the following:

- [awx.awx](https://docs.ansible.com/ansible/latest/collections/awx/awx/index.html) or [ansible.controller]ansible collection.

## Role Variables

The following variables are required for that role to work properly:

| Variable Name | Default Value | Required | Type | Description |
| :------------ | :-----------: | :------: | :------: | :---------- |
| `controller_api_plugin` | `ansible.controller` | yes | str | Full path for the controller_api_plugin to be used. <br/> Can have two possible values: <br/>&nbsp;&nbsp;- awx.awx.controller_api             # For the community Collection version <br/>&nbsp;&nbsp;- ansible.controller.controller_api  # For the Red Hat Certified Collection version|
| `organization_filter` | N/A | no | str | Exports only the objects belonging to the specified organization (applies to all the objects that can be assigned to an organization). |
| `organization_id` | N/A | no | int | Alternative to `organization_filter`, but specifiying the current organization's ID to filter by. Exports only the objects belonging to the specified organization (applies to all the objects that can be assigned to an organization). |
| `output_path` | `/tmp/filetree_output` | yes | str | The path to the output directory where all the generated `yaml` files with the corresponding Objects as code will be written to. |
| `input_tag` | `['all']` | no | List of Strings | The tags which are applied to the 'sub-roles'. If 'all' is in the list (the default value) then all roles will be called. |

## Dependencies

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

## Example Playbook

```yaml
---
- hosts: localhost
  connection: local
  gather_facts: false
  vars:
    controller_hostname: "{{ vault_controller_hostname }}"
    controller_username: "{{ vault_controller_username }}"
    controller_password: "{{ vault_controller_password }}"
    controller_validate_certs: "{{ vault_controller_validate_certs }}"
  pre_tasks:
    - name: "Check if the required input variables are present"
      assert:
        that:
          - input_tag is defined
          - (input_tag | type_debug) == 'list'
        fail_msg: 'A variable called ''input_tag'' of type ''list'' is needed: -e ''{input_tag: [organizations, projects]}'''
        quiet: true

    - name: "Check if the required input values are correct"
      assert:
        that:
          - tag_item in valid_tags
        fail_msg: "The valid tags are the following ones: {{ valid_tags | join(', ') }}"
        quiet: true
      loop: "{{ input_tag }}"
      loop_control:
        loop_var: tag_item
  roles:
    - infra.controller_configuration.filetree_create

  post_tasks:
    - name: "Delete the Authentication Token used"
      ansible.builtin.uri:
        url: "https://{{ controller_hostname }}{{ controller_oauthtoken_url }}"
        user: "{{ controller_username }}"
        password: "{{ controller_password }}"
        method: DELETE
        force_basic_auth: true
        validate_certs: "{{ controller_validate_certs }}"
        status_code: 204
      when: controller_oauthtoken_url is defined
...
```

## License

GPLv3+

## Author Information

- [Ivan Aragonés](https://github.com/ivarmu)
