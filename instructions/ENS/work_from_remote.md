# Proxy Jump : Bastion

In case you want to acces the LPENS network from remote, you need to use the server `bastion`.
You can find the official instruction [here](https://it.phys.ens.fr/passerelle-ssh-bastionphysensfr), but the main steps are the following.

## Key generation 
From the home directory of your personal computer 

- generate your acces key via the command
```
ssh-keygen -t rsa -b 4096 -o  -C "<firstname>.<secondname>"
```
You will be asked to provide a password. It will be required everytime you acces through bastion. This command should have created a private key `id_rsa` and a public key `id_rsa.pub` inside a hidden folder `.ssh/`.

- access the link [https://aaa2.phys.ens.fr/](https://aaa2.phys.ens.fr/) with your LPENS credentials. IMPORTANT: you can access this link only within the ENS account, otherwise you will have to contact the service informatique (si@phys.ens.fr) to do it for you.

- acces the page "SSH", press the button "Éditer" (bottom right), upload the file `id_rsa.pub` and save.

## Remote Access
You can now access a "machine" from remote via the command
```
ssh -J user@bastion.phys.ens.fr <username>@<machine>.lpt.ens.fr
```
You can define a shortcut by creating a file `config` in the `.ssh` folder of your home directory (where your private key is stored) and 
```
Host bastion
  HostName bastion.phys.ens.fr
  IdentityFile ~/.ssh/id_rsa
  User <username>
  ControlPersist 10m
  PubkeyAuthentication yes
  
Host <machine>
  ProxyJump bastion
  HostName <machine>.phys.ens.fr
  IdentityFile ~/.ssh/id_rsa
  User <username>
  ProxyCommand ssh -X <username>@bastion.phys.ens.fr -W %h:%p
```
From now on you can just type simply
```
ssh <machine>
```
Note that the address of the machine ".phys.ens.fr" could change depending on the machine.

### Copy/Paste Files
To copy/paste a file from a remote machine to your local machine (and viceversa) you can use the following command
```
scp -o ProxyCommand="ssh -W %h:%p <username>@bastion.phys.ens.fr" <username>@<machine>.phys.ens.fr:~/path/to/remote path/to/local/
```
Too much convoluted? I agree, that's where you can take advantage of your `config` file: 
```
scp <machine>:~/path/to/remote path/to/local/
```
this command is equivalent to the previous!

If you are interested in synchronizing files across different devices you might want to consider using `rsync` instead, e. g.
```
rsync -avzh <machine>:~/path/to/remote path/to/local/
```
