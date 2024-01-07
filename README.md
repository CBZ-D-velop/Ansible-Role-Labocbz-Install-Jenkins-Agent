# Ansible role: labocbz.install_jenkins_agent

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)
![Tag: Jenkins](https://img.shields.io/badge/Tech-Jenkins-orange)
![Tag: Agent](https://img.shields.io/badge/Tech-Agent-orange)

An Ansible role to install and configure a Jenkins Agent on your host.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
install_jenkins_agent_user: "jenkins"
install_jenkins_agent_group: "jenkins"
install_jenkins_agent_password: "PASSWORDDDDD"

install_jenkins_agent_name: "Jenkins-Agent-1"
install_jenkins_agent_home: "/home/{{ install_jenkins_agent_user }}/{{ install_jenkins_agent_name | lower }}"
install_jenkins_agent_master_remote: "https://jenkins.domain.tld"
install_jenkins_agent_jar: "{{ install_jenkins_agent_master_remote }}/jnlpJars/agent.jar"
install_jenkins_agent_secret: "XXXXXXXX"

install_jenkins_agent_log_path: "/var/log/jenkins-agent"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_install_jenkins_agent_user: "jenkins"
inv_install_jenkins_agent_group: "jenkins"
inv_install_jenkins_agent_password: "PASSWORDDDDD"

inv_install_jenkins_agent_name: "TEST-AGENT-ANSIBLE"
inv_install_jenkins_agent_home: "/home/{{ inv_install_jenkins_agent_user }}/{{ inv_install_jenkins_agent_name | lower }}"
inv_install_jenkins_agent_master_remote: "https://jenkins.domain.tld"
inv_install_jenkins_agent_jar: "{{ inv_install_jenkins_agent_master_remote }}/jnlpJars/agent.jar"
inv_install_jenkins_agent_secret: "XXXXXXXX"

inv_install_jenkins_agent_log_path: "/var/log/jenkins-agent"

```

```YAML
# From AWX / Tower
---
all vars from to put/from AWX / Tower
```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
    - name: "Include labocbz.install_jenkins_agent"
      tags:
        - "labocbz.install_jenkins_agent"
      vars:
        install_jenkins_agent_user: "{{ inv_install_jenkins_agent_user }}"
        install_jenkins_agent_group: "{{ inv_install_jenkins_agent_group }}"
        install_jenkins_agent_name: "{{ inv_install_jenkins_agent_name }}"
        install_jenkins_agent_password: "{{ inv_install_jenkins_agent_password }}"
        install_jenkins_agent_home: "{{ inv_install_jenkins_agent_home }}"
        install_jenkins_agent_master_remote: "{{ inv_install_jenkins_agent_master_remote }}"
        install_jenkins_agent_jar: "{{ inv_install_jenkins_agent_jar }}"
        install_jenkins_agent_secret: "{{ inv_install_jenkins_agent_secret }}"
        install_jenkins_agent_log_path: "{{ inv_install_jenkins_agent_log_path }}"
      ansible.builtin.include_role:
        name: "labocbz.install_jenkins_agent"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2024-01-02: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
