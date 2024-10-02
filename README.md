# role-aap-object-credential
Ansible role to build AAP workflow objects

# Ref
https://console.redhat.com/ansible/automation-hub/repo/published/ansible/controller/content/module/schedule/

# How to use

Step 1: Install the role in your environment.
   - You could have roles/requirements.yml if running on AAP.
   - Or simple install on your environment.

Step 2: Define your variables in the structure below

- organization
- description
- list_of_credential_field
    - name of credential
    - credential type
    - username
    - password
    - host

Step 3: Call the role from your playbook.

# Example
## variables
---
organization: my-organization
description: it is an AAP credential


cisco_creds: 
  - "{{ cisco_creds_name }}"
  - Machine
  - "{{ cisco_username }}"
  - "{{ cisco_password }}"
 
git_credential: 
  - "{{ git_svc_acc_name }}"
  - Source Control
  - "{{ git_username }}"
  - "{{ git_token }}"

redhat_quay_io_credential:
  - "{{ redhat_quay_creds_name }}"
  - Container Registry
  - "{{ redhat_quay_username }}"
  - "{{ redhat_quay_token }}"
  - "{{ redhat_quay_host }}"

## playbook

---
- name: Playbook to configure AAP
  hosts: localhost
  gather_facts: false
 
  pre_tasks:
    - name: Include specific project variables
      ansible.builtin.include_vars:
        dir: group_vars

  tasks:
    !
    !
    - name: Import role-aap-object-credential
      ansible.builtin.include_role:
        name: role-aap-object-credential
      vars:
        build: true
        description: "{{ common_description }}"
        organization: "{{ organization_name }}"
        in_list: "{{ item }}"
      loop:
        - "{{ cisco_creds }}"
        - "{{ git_credential }}"
        - "{{ redhat_quay_io_credential }}"
    !
    !
