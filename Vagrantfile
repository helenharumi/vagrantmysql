
# Helen Harumi Adachi RA:1904918

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"

  config.vm.provider "virtualbox" do |vb|
  vb.name = "mysqlserver"
  vb.memory = 1024
  vb.cpus = 1
  config.vm.network "private_network", type: "dhcp"
  config.vm.network "forwarded_port", guest: 3306, host: 3306
end

  $script_mysql = <<-SCRIPT
  apt-get update 
  debconf-set-selections <<< 'mysql-server mysql-server/root_password password root'
  debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'
  apt-get install -y mysql-server-5.7 
  mysql -u root -proot -e "CREATE DATABASE testeV" 
  mysql -u root -proot -e "GRANT ALL PRIVILEGES ON *.* TO 'root' IDENTIFIED BY 'root'" 
  mysql -u root -proot -e "flush privileges" 
  sed -i "s/.*bind-address.*/bind-address = 0.0.0.0/" /etc/mysql/mysql.conf.d/mysqld.cnf 
  service mysql restart
SCRIPT

config.vm.provision "Install mysql", privileged: true, type: "shell", inline: $script_mysql
end