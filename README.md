# Provision simple kubernetes  cluster

###Very simple and flat **Vagrantfile** + **Ansible** playbook to get **Kubernetes** multinode cluster up and running.



This scripts will help you installing and running a simple multinode **Kubernetes cluster**, one master node and one or several worker nodes into **Centos 7 virtual machines** managed by **Virtual Box Hypervisor**, it use vagrantfile to define all cluster nodes and use **Ansible** as Vagrant local provisioner, loca provisioner means you **don't** have to install Ansible on your host, Vagrant will install it on each virtual machine and will copy Ansible playbook into vm's to execute it locally.

Keep in mind this scripts are for educational purpouses only, Ansible playbook hasn't been optimized to be used on production environments this has just thought as a *fast* and easy way to get the cluster rebuilded after broken events because of my learning process about kubernetes.



1. [Install Vagrant](https://www.vagrantup.com/docs/installation/)
   1. **NOTE** You have to use **Virtual Box** as your hypervisor

2. Clone this repo
3. Edit **Vagrantfile** to adjust cluster boxes hardware specification as shown below

```ruby
BOXES = [
   {
     :name => 'kubemaster',
     :ip   => '192.168.50.120',
     :type => 'master',
     :vbox_cnf => [
         {
            '--memory' => '4096',
            '--cpus'   => '2'
         }
     ]
   },
   {
     :name => 'worker-1',
     :ip   => '192.168.50.121',
     :type => 'worker',
     :vbox_cnf => [
         {
            '--memory' => '2048',
            '--cpus'   => '1'
         }
     ]
   },
   {
     :name => 'worker-2',
     :ip   => '192.168.50.122',
     :type => 'worker',
     :vbox_cnf => [
         {
            '--memory' => '2048',
            '--cpus'   => '1'
         }
     ]
   }
]
```

Here some **highlights** has to be mentioned:

* The first box definition has to be for **master node**.
* `:type` defines ansible tags and it has to be **master** for kubernetes master nodes and **worker** for workers nodes.
* Master node has to have at least **4 gigabytes of RAM** and at least **2 CPUs**.
* You have to have at least one **worker node** definition.



4. Launch vagrant to build the cluster.

   ```bash
   vagrant up
   ```

5. Go out to get a coffe.
6. Enjoy your cluster :smiley:.