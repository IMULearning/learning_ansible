# Learning Ansible

## Requirement
- Target machine must have Python at `/usr/bin/python`

## Host File
- Host file located at `/etc/ansible/hosts`
- To use a custom file: `ansible -i custom_host_file --list-hosts all`
- Put configuration in `ansible.cfg` to override defaults so don’t have to provide `-i dev` each time
- Host selection can be filtered

```
ansible --list-hosts "app*"
ansible --list-hosts webserver
ansible --list-hosts webserver[0]
ansible --list-hosts database,control
ansible --list-hosts \!webserver
```

## Task Module (Default Module)
- `ansible -m ping all` Invoke all command within ping module
- `ansible -m command -a "hostname" all` specify hostname on all hosts

## Playbook and Tasks
- command: `ansible-playbook book.yml`
- `become: true`: become sudo user
- Handlers: notify a set of tasks by name
- Collect facts from host: `ansible -m setup db01`
- `ignore_errors: true` ignore error of a task

Limit hosts during execution
- `ansible-playbook site.yml  --limit app01`

Tags
- `ansible-playbook site.yml  --list-tags`
- `ansible-playbook site.yml  --tags "packages"` select tasks with tag "packages"
- `ansible-playbook site.yml  --skip-tags "packages"` skip tasks with tag "packages"

Debug
- `ansible-playbook site.yml --step` step debug
- `ansible-playbook site.yml --list-tasks`
- `ansible-playbook site.yml —start-at-task "task_name"`

Dry Run
- `ansible-playbook --syntax-check site.yml`
- `ansible-playbook --check site.yml`	# dry run, but doesn’t support any dynamic value, like register

Debug Module
`- debug: var=var_name`	# use vars as var_name to print all variables

### Encryption
`ansible-playbook —ask-vault-pass somePlaybook.yml`

OR

```
echo “pwd” > ~/.vault_pass.txt
chmod 0600 !$
```
Specify `vault_password_file` in `ansible.cfg`
