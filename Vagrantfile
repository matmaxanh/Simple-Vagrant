# Config
hostname      = "minelamp"
server_ip     = "192.168.22.10"
server_cpus   = "1"   # Cores
server_memory = "1024" # MB
base_box      = "mine/ubuntu_lamp"

Vagrant.configure(2) do |config|

    # Specify the base box
    config.vm.box = base_box

    # Create a hostname, don't forget to put it to the `hosts` file
    # This will point to the server's default virtual host
    config.vm.hostname = hostname

    # Create a forwarded port mapping which allows access to a specific port
    # within the machine from a port on the host machine. In the example below,
    # accessing "localhost:8080" will access port 80 on the guest machine.
    config.vm.network :forwarded_port, guest: 80, host: 8080
    config.vm.network :forwarded_port, guest: 3306, host: 33060
    config.vm.network :forwarded_port, guest: 443, host: 44300
    config.vm.network :forwarded_port, guest: 5432, host: 54320

    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    config.vm.network :private_network, ip: server_ip

    # Create a public network, which generally matched to bridged network.
    # Bridged networks make the machine appear as another physical device on
    # your network.
    # config.vm.network :public_network

    # If true, then any SSH connections made will enable agent forwarding.
    # Default value: false
    config.ssh.forward_agent = true

    # Share an additional folder to the guest VM
    config.vm.synced_folder ".", "/var/www/html"
    
    # Replicate local .gitconfig file if it exists
    if File.file?(File.expand_path("~/.gitconfig"))
        config.vm.provision "file", source: "~/.gitconfig", destination: ".gitconfig"
    end

    # If using VirtualBox
    config.vm.provider :virtualbox do |vb|

        vb.name = hostname
    
        # Set server cpus
        vb.customize ["modifyvm", :id, "--cpus", server_cpus]

        # Set server memory
        vb.customize ["modifyvm", :id, "--memory", server_memory]
    end

    # Avoids 'stdin: is not a tty' error.
    config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
end
