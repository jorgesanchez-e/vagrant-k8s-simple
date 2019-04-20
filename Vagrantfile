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


Vagrant.configure("2") do |config|
  config.ssh.private_key_path = ["keys/kubernetes", "~/.vagrant.d/insecure_private_key"]
  config.ssh.insert_key = false
  config.vm.provision "file", source: "keys/kubernetes.pub", destination: "~/.ssh/authorized_keys"

  BOXES.each do |opts|
     config.vm.define opts[:name] do |box|

       box.vm.box = "centos/7"
       box.vm.network "private_network", ip: opts[:ip]
       box.vm.hostname = opts[:name] 

       box.vm.provider "virtualbox" do |vb| 
          opts[:vbox_cnf].each do |hash|
             hash.each do |key, value|
                vb.customize ['modifyvm', :id, key, value]
             end
          end
       end

       box.vm.provision "ansible_local" do |ansible|

          hosts = ""
          master_ip = ""
          BOXES.each do |x|
             if x[:type] == 'master'
                master_ip = x[:ip]
             end
             hosts = hosts + x[:ip] + " " + x[:name] + ","
          end

          ansible.extra_vars = {
             master_ip: master_ip,
             dns_nodes: hosts
          }

          if opts[:type] == 'worker'
             ansible.skip_tags = 'master'
          else
             ansible.skip_tags = 'worker'
          end

          ansible.version = "2.7.10"
          ansible.compatibility_mode = "2.0"
          ansible.playbook = 'ansible/playbook.yml'
       end
     end
  end
end
