# Ansible playbook for provision servers with SSH public key authentication via Hydra Passport

This is a demo project for ODO-154 task. It uses Vagrant for VM provisioning. 

To succeed in testing valid Passport OpenID credentials `CLIENT_ID` and `CLIENT_SECRET` are required.
Credentails can be obtained from Oleg Adignalov.

The default Passport credentials are in `files/passport_creds` (encrypted by Ansible Vault).

A single SSH key (`~/.ssh/onix_ansible_ed25519`) is used for connecting to each started VM. It is known that if you start many VMs in a single Vagrant session, Vagrant will generate a separate key for every VM and ssh may refuse your login because of ssh-agent key 'pollution'.

The deployment process is described in `provisioning.yml` file.

There are two scripts used:
- `/usr/share/libpam-script/pam_script_auth` (used by PAM for initial user creation) and
- `/home/passport/bin/passport_ssh_auth.sh` (used by sshd for obtaining ssh public key for the particular user).

All connecting users are checked for presence in Passport and cached in sqlite database for `CACHETTL` seconds (usually, 10 minutes).

To have access the following requirements for the user must be met:
- Valid username (must match this re `^[a-z][-.a-z0-9]*$`).
- Passport attribute `isActive` set to `true`.
- Passport group membership is specified in `ALLOWEDGROUPS` (variable found in `/home/passport/.passport_subs`.
- Passport attribute `sshPublicKey` has a valid public key.

## Installation notes

A few external packets (installed by Ansible) for script operation are required: jq (JSON parsing), sqlite (caching database) and libpam-script (PAM scripting).

During the installation of `libpam-script` the system `/etc/pam.d/common-*` files will be overwritten. For the purpose of ODO-154 task the additional functionality is only required for sshd authentication phase. With this, the original `/etc/pam.d/common-*` must be backed up and then restored after `libpam-script` installation. This is done by Ansible during deploy.

## Operation

Check the `Vagrantfile` and `inventory` before you begin.
In the current version only Ubuntu 20.04 is uncommented (IP: 172.28.128.20).

It is assumed that Ansible uses this SSH key: `~/.ssh/onix_ansible_ed25519`. You can use any other key, just edit an appropriate sections within `Vagrantfile` and `ansible.cfg`.

```
$ ssh-keygen -R 172.28.128.20 # remove an old ssh key from known_hosts for 172.28.128.20

$ vagrant up

$ ansible-playbook provisioning.yml
```

If there were no errors during deployment, try to connect to the VM host using an arbitrary username:

```
$ ssh asdfqwer@172.28.128.20
```

Do not press ENTER if asked for a password, type in some random characters instead.
In this case should be no new user created in the VM /etc/passwd and /home.

Now use your Passport credentails for an SSH connection to VM:

```
$ ssh your.passport_username@172.28.128.20
```

Same as above, type in some random characters when asked for a password, press ENTER and then press CTRL-C.
The new user and a home directory should be now created within the VM.

Now supply your valid ssh key which is specified in the Passport:

```
$ ssh -i ~/.ssh/your_priv_key your.passport_username@172.28.128.20
```

Enter your passphrase when asked. You should now have user access to the VM. Now try the `sudo` command:

```
$ sudo id
```

This concludes the demo.

