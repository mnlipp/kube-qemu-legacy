*You should move on to the [final solution](https://jdrupes.org/vm-operator/).*

# Run libvirt-VMs Qemu in Kubernetes

This is an intermediate step towards a Kubernetes operator
for running qemu based VMs.

If you have come here, you're probably aware that RedHat has decided to
[drop Spice](https://bugzilla.redhat.com/show_bug.cgi?id=2030592) from 
the packages shipped with RHEL. As discussed in various places (e.g. 
[here](https://www.reddit.com/r/redhat/comments/ux38f3/why_was_spice_qxl_removed_from_rhel_9/)), 
this is bad news for anybody who does serious work with VMs that run 
on RHEL 9.1 (in whatever flavour). Without support for cut & paste, 
performant access over the internet and USB redirection the VMs aren't 
usable any more for many use cases.

The goal of this porject is (as an intermediate step) to move the
existing VMs that you have created with virt-manager or virsh into a 
pod. This allows you to upgrade your host OS to a RHEL 
(or AlmaLinux/Rocky Linux) version without Spice support.

Of course, you loose the easy manageability of your VMs and that's 
why an operator based solution will be the next step.

The pod template is based on a `virsh domxml-to-native qemu-argv` invocation
for one of my more recent Linux VMs. More sophisticated possibilities
for configuring the VM will be part of the future operator based 
solution. For e.g. Windows-VMs I recommend to generate a base k8s
configuration with something like 

```
helm install -n qemu-legacy winvm -f winvm-values.yaml ./legacy-qemu-vm --dry-run > winvm-all.yaml
```

and merge the information from your machine's `domxml-to-native qemu argv` 
with the generated configuration.
