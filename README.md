# gitlab-vagrant-executor
Run vagrant libvirt vm as envs for gitlab jobs.

## Consept

Use https://docs.gitlab.com/runner/executors/custom.html for run libvirt kvm VMs use Vagrant as wrapper.

## Setup
On runner host

1. gitlab-runner register

and chose *custom*

2. Edit section of creation runner in file */etc/gitlab-runner/config.toml*

```toml
[[runners]]
  name = "your-runner-name"
  url = "https://gitlab.example.com"
  token = "SUPERSECRETTOKEN"
  executor = "custom"
  output_limit = 16384 # optional
  builds_dir = "/home/vagrant/builds"
  cache_dir = "/home/vagrant/cache"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.custom]
    prepare_exec = "/opt/vagrant-driver/prepare.sh"
    run_exec = "/opt/vagrant-driver/run.sh"
    cleanup_exec = "/opt/vagrant-driver/cleanup.sh"
```

3. Copy bash scripts and Vagrantfile in */opt/vagrant-driver*

4. kill -SIGHUP $(pidof gitlab-runner) or restart runner process.

5. Install qemu, kvm, libvirt, Vagrant, etc...

## Use

Use this runner as usual

.gitlab-ci.yml

```yaml
echo:
  stage: test
  script:
    - echo test
  tags:
    - vagrant-executor
```
Or configure VMs (all vars bellow are default, you can change it)

```yaml
echo:
  stage: test
  variables:
  	VAGRANT_VM_MEMORY: 2048
  	VAGRANT_VM_CPUS: 2
  	VAGRANT_BOX_NAME: andreineustroev/ubuntu-ci
  	VAGRANT_BOX_VERSION: 0.0.3
  script:
    - echo test
  tags:
    - vagrant-executor
```