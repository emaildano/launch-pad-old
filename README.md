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

## Sites Config
1. Default site vars is located `playbooks/group_vars/all/sites.yml`
2. `~/sites.yml` overrides the default settings
3. This override location can be edited in `playbooks/group_vars/all/main.yml`, `sites_config`

## Adding Sites
1. Edit your prefered `sites.yml` file. If you haven't set one up refer to the Sites Config section above
2. Run `vagrant reload --provision`

## Resolving Errors
1. Try `vagrant reload` or `vagrant reload --provision` again
2. [Submit an issue](https://github.com/emaildano/launch-pad/issues) if all else fails

## Playbooks
For all various mission

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

## Deploying to AWS

### Target Specific Tag
`ansible-playbook -l tag_Key_value playbooks/server.yml`

### Override Defaults with Variable in CLI
`ansible-playbook -l tag_Name_ansible playbooks/sites.yml --extra-vars "sites_config=~/other-sites.yml"`

### Launch Instance, Create Security Group, etc.
`ansible-playbook playbooks/aws.yml`
