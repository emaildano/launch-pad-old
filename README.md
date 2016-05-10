# Launch Pad
Lifting space junk into orbit

## Requirements

- Ansible >= 2.0
- Virtualbox >= 4.3
- Vagrant >= 1.5
- Vagrant Host Manager >= 1.8

## Get Started

1. Clone or download repo
2. Add sites and config to `sites.yml`
3. Run `vagrant up`

## Playbooks
For all various stages

### server.yml
The foundation and first step of any environment

### sites.yml
Config for local or remote projects

### localhost.yml
Playbook for Vagrant and used during `vagrant up`

### aws.yml
Working out the kinks ;)

## Extras

### SSH Login as User
`ssh -p 2222 user@host`
