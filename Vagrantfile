required_plugins = %w( vagrant-hostsupdater vagrant-berkshelf vagrant-omnibus)
required_plugins.each do |plugin|
  exec "vagrant plugin install #{plugin};vagrant #{ARGV.join(" ")}" unless Vagrant.has_plugin? plugin || ARGV[0] == 'plugin'
end


Vagrant.configure("2") do |config|
  config.vm.define "app" do |app|
    app.vm.box = "ubuntu/xenial64"
    app.vm.network "private_network", ip: "192.168.10.100"
    app.hostsupdater.aliases = ["development.local"]
    app.vm.synced_folder "app", "/home/ubuntu/app"
    app.vm.synced_folder "app/views", "/home/ubuntu/views"
    app.vm.provision "chef_solo" do |chef|
      chef.add_recipe "node::default"
      chef.version = '14.12.9'
    end
  end

  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/xenial64"
    db.vm.network "private_network", ip: "192.168.10.150"
    db.hostsupdater.aliases = ["database.local"]
    db.vm.provision "chef_solo" do |chef|
      chef.add_recipe "mongo::default"
      chef.version = '14.12.9'
    end
  end
end
