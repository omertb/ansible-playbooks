### Notes

**update_host_ns_fw.yml:** Updates the A and PTR record of a _host_ on bind9 and restarts
the bind service to take effect. Then it updates the object on PANOS (paloalto fw)
which defined the IP address of this _host_. and is used in policy rules. By this means:
- no need to bother with rules on fw (if the object is used properly in all related rules)
- DNS definitions and access rules about new IP address of the host is in effect.


> Required Packages to install on host running ansible:
```
$ ansible-galaxy install paloaltonetworks.paloaltonetworks
$ sudo apt install python-pip
```

> Required pypi package on host running bind9:
```
$ pip install dnspython
```
