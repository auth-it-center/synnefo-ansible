synnefo-ansible
===============

A (soon-to-be) set of Ansible modules to manage vm/storage/networks on Synnefo-based infrastructures.

Currently includes:

* **synnefo_compute**: Ansible module to interact with the compute service ('Cyclades')


synnefo_compute
---------------

An Annible module that lets you manage Virtual Machine instances on Synnefo-based infrastructures 

### installation
Module requires Kamaki client be installed.
Kamaki is the official ~okeanos command line client.

You can download kamaki from the [project homepage](http://www.synnefo.org/docs/kamaki/latest/index.html)

Download synnefo_compute source file from [here](https://github.com/auth-scc/synnefo-ansible/raw/master/synnefo_compute) and place it inside `ansible/library` directory


### a first example

Here's how creating a new VM using synnefo_compute module, looks like:

```
# ...Contents of ansible playbook
# Create a new VM using ansible module: synnefo_compute

   synnefo_compute:
      name=my_brand_new_vm
      state=present
      wait=yes
      flavor_id=4
      image='Fedora'
      auth_url='https://accounts.myexamplecloud.local/identity/v2.0'
      token='myApIaCcEssToKen'

```
Let's take a close look at each argument:


#####name

Refers to the VM's name. It can also refer to the desired name of a new VM to be created.


#####state
Refers to the desired state that the module execution must lead. As far as VMs are concerned this can mean:
+ `state=present` ensure that the specified VM will be present after execution. If not already present, create it.
+ `state=absent` ensure that the specified VM will be absent after execution. If not already absent, delete it.
+ `state=active` ensure that the specified VM will be active after execution. If not already active, start it.
+ `state=stopped` ensure that the specified VM will be stopped after execution. If not already stopped, shut it down.



#####wait
If set to true, module will pause & wait for the VM operation to be completed

#####flavor_id
integer referring to a specific flavor that's used to describe the VM (e.g. num of vcpus, ram etc.). 

#####image
name of a specific image used to boot the VM

#####auth_url
api authentication url of desired Synnefo infrastructure. The value defaults to [~okeanos](https://okeanos.grnet.gr/) service api authentication [url](https://accounts.okeanos.grnet.gr/identity/v2.0)

#####token
user's authentication token

###more arguments...

#####id
Refer to a specific VM by id, instead of using `name`

#####image_id
Refer to a specific image by id, instead of using `image`

#####flavor
Refer to a specific flavor by name, instead of using `flavor_id`
#####ssh_key
Filename of ssh key to be injected during VM's creation process

### query a flavor by specs

Instead of remembering a specific name or id for a flavor, user can provide the following specs as arguments during VM's creation process:
+ `vcpus` : number of virtual cores
+ `ram`: amount of ram
+ `disk`: amount of disk in GB
+ `dtype`: type of virtual disk to be used (default='drdb')

After all above arguments are specified, synnefo_compute will query infrastructure to find a matching flavor


### more examples....

```
# Shutdown a VM

   synnefo_compute:
      name=my_running_vm
      state=stopped
      wait=yes
      auth_url='https://accounts.myexamplecloud.local/identity/v2.0'
      token='myApIaCcEssToKen'

```

```
# Start a VM

   synnefo_compute:
      name=my_stopped_vm
      state=active
      wait=yes
      auth_url='https://accounts.myexamplecloud.local/identity/v2.0'
      token='myApIaCcEssToKen'

```

```
# Delete a VM

   synnefo_compute:
      name=my_loved_vm
      state=absent
      wait=yes
      auth_url='https://accounts.myexamplecloud.local/identity/v2.0'
      token='myApIaCcEssToKen'

```

```
# Create a VM and provide specs

   synnefo_compute:
      name=vm_with_specs
      state=present
      image='Ubuntu Server'
      vcpus=1
      ram=1024
      disk=20
      dtype=drdb
      wait=yes
      auth_url='https://accounts.myexamplecloud.local/identity/v2.0'
      token='myApIaCcEssToKen'

```


### dont forget to...
check out the exceptional documentation of [Synnefo](http://www.synnefo.org/docs/synnefo/latest/index.html) project and [Ansible](http://docs.ansible.com/)