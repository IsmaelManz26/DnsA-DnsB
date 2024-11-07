Vagrant.configure("2") do |config|
    # Configuración de la máquina DNSA
    config.vm.define "DNSA" do |dnsa|
      dnsa.vm.box = "debian/bookworm64"
      dnsa.vm.hostname = "dnsa"
      dnsa.vm.network "private_network", ip: "192.168.57.10"
      # Provisión para instalar BIND y dnsutils
      dnsa.vm.provision "shell", inline: <<-SHELL
        sudo apt update
        sudo apt upgrade -y
        sudo apt install -y bind9 dnsutils
        # Copiar archivos de configuración
        sudo cp /vagrant/DNSA/named.conf.local /etc/bind/named.conf.local
        sudo cp /vagrant/DNSA/db.ies.test /etc/bind/db.ies.test
        sudo cp /vagrant/DNSA/db.192.168.57  /etc/bind/db.192.168.57
        sudo cp /vagrant/DNSA/db.aulas /etc/bind/db.aulas
        sudo cp /vagrant/DNSA/db.informatica /etc/bind/db.informatica
        sudo cp /vagrant/DNSA/db.departamentos /etc/bind/db.departamentos
        # Reiniciar el servicio BIND
        sudo systemctl restart bind9
      SHELL
    end
  
    # Configuración de la máquina DNSB
    config.vm.define "DNSB" do |dnsb|
      dnsb.vm.box = "debian/bookworm64"
      dnsb.vm.hostname = "dnsb"
      dnsb.vm.network "private_network", ip: "192.168.57.11"
      # Provisión para instalar BIND y dnsutils
      dnsb.vm.provision "shell", inline: <<-SHELL
        sudo apt update
        sudo apt upgrade -y
        sudo apt install -y bind9 dnsutils
        # Copiar archivos de configuración
        sudo cp /vagrant/DNSB/named.conf.local /etc/bind/named.conf.local
        # Reiniciar el servicio BIND
        sudo systemctl restart bind9
      SHELL
    end
  end
  