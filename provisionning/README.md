Provisionning of the infrastructure for DockerEE
------------------------------------------------

Several quick ways to create the nodes:

### Vagrant

A simple Vagrant file is provided in this repo.

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: "echo Hello"

  config.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 1
  end

  nodes_number = 3
  (1..nodes_number).each do |i|
    hostname = "ucp#{i}"
    config.vm.define hostname do |ucp|
      ucp.vm.hostname = hostname
      ucp.vm.box = "ubuntu/trusty64"
      ucp.vm.network "private_network", type: "dhcp"
    end
  end
end
```

Change the nodes_number and run ```vagrant up```
Once the machine created, get their IP address and add them in the inventory file.

### Deploy on DigitalOcean with Terraform

Create a *variables.tf* from the *variables.tf.example* file.

Modification that needs to be done:

- set the value of do_token with your own 
- change the *count* to specify the number of managers and workers node needed
- change the ssh_keys so it uses the list of the fingerprints of your keys (eg: ssh_keys = ["e9:39:8c:f0:ae:6e:c6:e2:61:31:12:83:17:6e:fd:f3"]) 
- you can of course modify the other arguments to better match your needs.

Once the changes are done, run ```terraform plan``` to check everything is right, this is a dry run to ensure no error are detected before the launch.

Then run ```terraform apply``` to create the resources.

The Ansible inventory file will be automatically created in *configuration/invntory/inventory.tpl* and could b used as-is in the configuration step

### Your way

The option above are only 2 simple and very limited ways to get you started, of course you can select other options and cloud providers.

License
-------

The MIT License (MIT)

Copyright (c) [2018]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
