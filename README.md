# puppet_wiki
Puppet setup for deploying a wiki Webapp

Tutorial Notes: https://www.evernote.com/l/AkbKJ64YqfhNmoOB9WEHXns5d6AxgQP3eO0

## Vagrant Multiple-VM Creation and Configuration
Automatically provision multiple VMs with Vagrant and VirtualBox. Automatically install, configure, and test
Puppet Master and Puppet Agents on those VMs. All instructions can be found in the blog post:
[http://wp.me/p1RD28-1kX](http://wp.me/p1RD28-1kX)


#### JSON Configuration File
The Vagrantfile retrieves multiple VM configurations from a separate `nodes.json` file. All VM configuration is
contained in the JSON file. You can add additional VMs to the JSON file, following the existing pattern. The
Vagrantfile will loop through all nodes (VMs) in the `nodes.json` file and create the VMs. You can easily swap
configuration files for alternate environments since the Vagrantfile is designed to be generic and portable.

ssh in the VM and run bootstrap-master.sh (optional)
#### Instructions
```
vagrant up # brings up all VMs
vagrant ssh puppet.example.com

sudo service puppetmaster status # test that puppet master was installed
sudo service puppetmaster stop
sudo puppet master --verbose --no-daemonize
# Ctrl+C to kill puppet master
sudo service puppetmaster start
sudo puppet cert list --all # check for 'puppet' cert

# Open new tab for node1
vagrant ssh node01.example.com # ssh into agent node
sudo service puppet status # test that agent was installed
sudo puppet agent --test --waitforcert=60 # initiate certificate signing request (CSR)
```
Back on the Puppet Master server (puppet.example.com)
```
sudo puppet cert list # should see 'node01.example.com' cert waiting for signature
sudo puppet cert sign --all # sign the agent node(s) cert(s)
sudo puppet cert list --all # check for signed cert(s)
```

Pull config from master on nodes
```
sudo puppet agent -t
sudo puppet agent --verbose --no-daemonize
```



Refrence https://github.com/garystafford/multi-vagrant-puppet-vms
