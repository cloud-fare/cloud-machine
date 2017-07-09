# Cloud-Machine Roadmap

Goals:
* to make Cloud-Machine the best command line resource for a production orchestration engine to use when spinning up new machines.
* be really good at autoscale
    * up can be optimized to use an snapshot of an already launched machine
    * down needs to gracefully stop a machine and wait for all queues to flush
* be really good at reprovisioning an existing machine
* allow for reimaging of a machine after running privileged containers
  in a shared environment while maintaining other id and state information such as IP addresses
    * specific use cases
        * many cloud providers have minimum billing increments far larger
          than the lifecycle of individual privileged jobs.
        * Privileged jobs can not be trusted to leave the state of persistent drive mounts
          in a known good state (free of malware, etc)
* move config options out of makefiles into a cloud-machine config-file
    * allow, in order of load:
        * global cloud-machine config-files,
        * user cloud-machine config-files,
        * project specific cloud-machine config-files,
        * followed by environment variables
        * followed by command line options
        * followed by cloud-init URL contents
        * followed by persistent drives in a machine with filesystem label "CLOUD-INIT"
    * default: allow all settings from earlier loaded profiles to be inherited unless specifically excluded or overridden.
* config options to include machine resources
* config options to be provided to the remote machine as its cloud-init defaults
* drivers to make best efforts to implement all appropriate options.
* Cloud-Machine to report as errors mandatory settings not supported by a driver
    * command line option to audit configuration and dump the synthesized configuration


```yml
#cloud-init
cloud-machine:
    resources:
        ram: 4 # GB, floating point, will be rounded up to next available unit from driver)
        cpus: 2
        disks:
            volume1:
                devicepath : /dev/sda
                allocation: 20GB
                filesystem: zfs
                    kernel-module: zfs
                overwrite:
                wipe-on-boot:
                boot-partition : true
            volume2:
               devicepath : /dev/sr1
               readonly : true
               filesystem : iso9660
        nic:
        cloud-init-url:
rancher:

docker:

...
```



# Machine Roadmap

Machine currently works really well for development and test environments. The
goal is to make it work better for provisioning and managing production
environments.

This is not a simple task -- production is inherently far more complex than
development -- but there are three things which are big steps towards that goal:
**client/server architecture**, **swarm integration** and **flexible
provisioning**.

(Note: this document is a high-level overview of where we are taking Machine.
For what is coming in specific releases, see our [upcoming
milestones](https://github.com/docker/machine/milestones).)

### Docker Engine / Swarm Configuration

Currently there are only a few things that can be configured in the Docker Engine and Swarm.  This will enable more operations such as Engine labels and Swarm strategies.

### Boot2Docker Migration Support

Currently both Machine and Boot2Docker provider similar functionality.  This will enable users to migrate from boot2docker to machine.

### Expand Provisioner

Machine currently supports running Boot2Docker for "local" providers and Ubuntu for "remote" providers.  This will expand the provisioning capabilities to include other base operating systems such as Red Hat-like distributions and possibly other "just enough" operating systems.

### Windows Experience

Currently, the Machine on Windows experience is not as good as the Mac / Linux.  There is no "recommended" path to use Machine and there are several inconsistencies on Windows such as logging and output formatting.

# Project Planning

An [Open-Source Planning Process](https://github.com/docker/machine/wiki/Open-Source-Planning-Process) is used to define the Roadmap. [Project Pages](https://github.com/docker/machine/wiki) define the goals for each Milestone and identify current progress.
