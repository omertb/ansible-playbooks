
## Notes

### Vault Commands:

* unvaulted_pass file content:
```
vault_ansible_password: PASSWORD
```

* then encrypt this file, 
```Shell

# ansible-vault encrypt unvaulted_pass 

New Vault password: 
Confirm New Vault password: 
Encryption successful

# cat unvaulted_pass 

$ANSIBLE_VAULT;1.1;AES256
61333432623436316266343766356538633638613435386533343439623466386433313564393137
6666666337653066363539383232376363353832306161390a303962346166346464356565633163
37383936633538313238613361303961646262386633386434333437383964346137306564393566
6632646134633865650a666334636236313339613437333637633966663362356665653737363865
39326330646338626236613464656366373862343735343164383964353937343737386464396630
6531306130643439613062346532303762646535333533623632

# mv unvaulted_pass vaulted_pass
```

* to view vaulted_pass file content in clear text:

```Shell
# ansible-vault view vaulted_pass 
Vault password: 
vault_ansible_password: PASSWORD

```

> vault_ansible_password definition is used in _hosts_ file as seen below:
```
[all_switches:vars]
ansible_user=USERNAME
ansible_password="{{ vault_ansible_password }}"
ansible_network_os=ios
###
###
[A_Cisco]
A_3KRK1_C3550_1 ansible_host=10.10.254.45
```

### Running Playbook
* Playbook (ios_ver_serials.yml) for displaying serial numbers and ios versions of cisco devices:


```
- name: GATHER INFORMATION FROM ROUTERS
  hosts: A_Cisco
  connection: network_cli
  gather_facts: no

  tasks:
    - name: GATHER ROUTER FACTS
      ios_facts:

    - name: DISPLAY VERSION
      debug:
        msg: "The IOS version is: {{ ansible_net_version }}"

    - name: DISPLAY SERIAL NUMBER
      debug:
        msg: "The serial number is:{{ ansible_net_serialnum }}"
```

```Shell
$ ansible-playbook ios_ver_serials.yml --extra-vars '@vaulted_pass' --vault-id @prompt

## OR
$ ansible-playbook ios_ver_serials.yml --extra-vars '@vaulted_pass' --ask-vault-pass

## not to be prompt for vault password but to get from a file named 'vault-pass' containing the password:
$ ansible-playbook ios_ver_serials.yml --extra-vars '@vaulted_pass' --vault-id vault-pass

## if group_vars available and there is vault file in it:
$ ansible-playbook ios_ver_serials.yml --ask-vault-pass

```