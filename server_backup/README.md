## Ansible Notes

**adduser.yml:** Adds common ansible user and password with the credentials in inventory file.
**authorized_key.yml:** Creates ssh keys on a number of desired servers and fetches those public keys.
Then, fetched public keys are copied to authorized_keys of another server.
**server_backup:** Creates scripts and cron jobs on desired servers to backup specified
directories to a central determined server(to which public keys are copied to its authorized_keys)

> Utilized role template from https://galaxy.ansible.com/geerlingguy/backup

####authourized_key.yml:
```mermaid
sequenceDiagram
ansible ->> debian_servers: Create ssh keys
debian_servers ->> ansible: Fetch public keys
ansible ->> backup_server: Copy public keys
Note right of backup_server: Public keys of<br/>debian_servers are<br/>copied to authorized_key<br/>file of backup_server.

```

####server_backup.yml:
```mermaid
sequenceDiagram
ansible ->> debian_servers: Built scripts, add cron job
Note left of ansible: Bash scripts are created<br/>on debian_servers which</br>rysnc to remote, that is,</br>backup_server
debian_servers ->> backup_server: Cron Job: Rsync Directories

```

