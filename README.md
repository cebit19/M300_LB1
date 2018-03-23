# M300_LB1 Vagrant File DHCP & Apache Webserver

## Prozess starten

Um den Prozess zu starten, muss man mit der Git Konsole das File aufsuchen und ausführen. Um das File zu "starten", muss man den command "vagrant up" eingeben.
Danach wird der Prozess angestossen und die Vm oder Vm's wird/werden erstellt.

![](https://github.com/Verdorion/M300_LB1/blob/master/vagrantup.PNG)

## VM Konfiguration

Die VM's werden wie folgt konfiguriert. Wir haben die erste VM als "Server" erstellt; auf dieser läuft der DHCP, sowie der Apache Webserver. Die zweite VM ist die test Maschine, mit welcher wir die Website erreichen wollen, sowie die automatische IP zuordnung des DHCPs testen. Als Provieder haben wir uns für Virtualbox entschieden und als OS benutzen wir <a href="https://app.vagrantup.com/ubuntu/boxes/trusty64">Ubuntu/trusty64</a> normal und mit GUI (<a href="https://app.vagrantup.com/ubuntu/boxes/ubuntu-trusty64-gui">Ubuntu/trusty64-gui</a>)

Code der VM1:
```
Vagrant.configure(2) do |config|  
  config.vm.define "dhcpapache" do |dhcpapache|
    dhcpapache.vm.box = "ubuntu/trusty64"
    dhcpapache.vm.hostname = "dhcpapache"
    dhcpapache.vm.network "public_network", type:"dhcp"
    dhcpapache.vm.network "private_network", ip:"10.100.10.1", virtualbox__intnet:true
    
    dhcpapache.vm.provider "virtualbox" do |vb|
	  vb.memory = "2048" 
    end
```
Code der VM2:
```
config.vm.define "dhcpclient" do |dhcpclient|
    dhcpclient.vm.box = "ubuntu/trusty64-gui"
    dhcpclient.vm.hostname = "dhcpclient"
    dhcpclient.vm.network "private_network", type:"dhcp", virtualbox__intnet:true

    dhcpclient.vm.provider "virtualbox" do |vb|
	  vb.memory = "2048" 
    end
```

## HTML

Zum test des Apache Webservers haben wir noch ein HTML File eingefügt, welches wir über die zweite VM erreichen wollen.

Code:
```
udo rm /var/www/html/index.html
        echo "<html lang="de">"| sudo tee -a /var/www/html/index.html
        echo "<head>"sudo tee -a /var/www/html/index.html
		    echo "<title>Vorlage_Home</title>" | sudo tee -a /var/www/html/index.html
	      echo "</head>" | sudo tee -a /var/www/html/index.html
        echo "<body>" | sudo tee -a /var/www/html/index.html
		    echo "<header>" | sudo tee -a /var/www/html/index.html
				echo "<hgroup>" | sudo tee -a /var/www/html/index.html
				echo "<h1>Willkommen auf meiner Webseite</h1>" | sudo tee -a /var/www/html/index.html
				echo "</hgroup>" | sudo tee -a /var/www/html/index.html
			  echo "</header>" | sudo tee -a /var/www/html/index.html
        echo "</body>" | sudo tee -a /var/www/html/index.html    
        echo "</html>" | sudo tee -a /var/www/html/vagindex.html
```

## DHCP Konfiguration

Den DHCP haben wir wie folgt konfiguriert. Wir haben ihm den DNS von Google gegeben ( 8.8.8.8 ), sowie das subnetz 10.100.10.0, die netmask 255.255.255.0 und die ip-range 10.100.10.20 bis 10.100.10.254
