# Ansible Role: Container Services

![Release](https://img.shields.io/gitlab/v/release/draget/ansible-role-container-services?sort=semver)
![Pipeline](https://gitlab.com/draget/ansible-role-container-services/badges/main/pipeline.svg)
![Coverage](https://gitlab.com/draget/ansible-role-container-services/badges/main/coverage.svg)

**Warning: This project is still in its setup phase and not yet usable!**

This Ansible role helps to deploy container services defined as docker-compose files. Right now it is used mostly privately and has not yet stood the test of time. Feedback welcome!

**Note:** This project is maintained on [GitLab](https://gitlab.com/draget/ansible-role-container-services) and mirrored to other platforms. Please open Issues and Merge Requests on GitLab!

## Links:

* Anisble Galaxy page: [https://galaxy.ansible.com/dragetd/ansible_role_container_services](https://galaxy.ansible.com/dragetd/ansible_role_container_services)
* Main repository: [https://gitlab.com/draget/ansible-role-container-services](https://gitlab.com/draget/ansible-role-container-services)

## Usage

TODO

## Role Variables

| Variable                      | Default        | Description                                            |
|-------------------------------|----------------|--------------------------------------------------------|
| `container_service_basedir`   | `/opt/service` | Base directory of services                             |
| `container_service_volumedir` | `/srv`         | Where to create directories for persistent host-mounts |
| `operator_user`               | `operator`     | Default user of files                                  |
| `operator_group`              | `operator`     | Default group of files                                 |
| `default_file_mode`           | `0760`         | Default access mode for files                          |
| `default_dir_mode`            | `0770`         | Default access mode for directories                    |

To configure the services, specify the following variables (example):

```yml
# Container services variables template
container_services:
  # External Docker networks to be created
  external_networks: [ "foobar" ]
  services:
    # Service name is used for dirs and such, should be lowercase
    - name: "example"
      # Optional. Set further options for the service persistence directory. Set to '' (nothing) to only create default service directory with default mode (0700)
      volume:
        mode: "0700"
        owner: "exampleEve"
        # Optional. Extra subdirectories
        subdirs:
          - name: "exampleDatabaseDir"
          - name: "otherExample"
            mode: "0700"
            owner: "exampleEve"
      # Optional. Static files to copy
      files:
        - name: "exampleFile.txt"
        - name: "someDirectory/"
        - name: "otherFile"
          mode: "0700"
          owner: "eve"
          validate: "check_stuff %s"
      # Optional. Templated files to copy (docker-compose is always copied)
      templates:
        - name: "config.toml"
        - name: "otherTemplate"
          mode: "0700"
          owner: "eve"
          validate: "check_stuff %s"
      # Optional. Files outside the service directory, must have 'dest'
      system_files:
        - name: "exampleFile"
          dest: "/usr/local/bin/"
        - name: "otherFile"
          dest: "/etc/logrotate.d/"
          mode: "0700"
          owner: "eve"
          validate: "check_stuff %s"
      # Optional. Create additional, external Docker networks
      docker_networks: [ "networkA", "networkB" ]
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md)
