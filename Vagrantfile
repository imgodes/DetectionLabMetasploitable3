Vagrant.configure("2") do |config|
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.vm.define "ub1404" do |ub1404|
    ub1404.vm.box = "rapid7/metasploitable3-ub1404"
    ub1404.vm.hostname = "metasploitable3-ub1404"
    config.ssh.username = 'vagrant'
    config.ssh.password = 'vagrant'

    ub1404.vm.network "private_network", ip: "192.168.56.210"
    ub1404.vm.network "public_network", briedge: ""
    ub1404.vm.provider "virtualbox" do |v|
      v.name = "Metasploitable3-ub1404"
      v.memory = 2048
    end
  end
    # OSQuery config files are copied
    # Fleetdm server cert is pulled from 192.168.56.105 using curl and default creds
    config.vm.provision "file", source: "flagfile.txt", destination: "~/flagfile.txt"
    config.vm.provision "file", source: "secret.txt", destination: "~/secret.txt"
    config.vm.provision "file", source: "nxlog.conf", destination: "~/nxlog.conf"
    config.vm.provision "file", source: "nxlog-ce_2.10.2150_ubuntu_trusty_amd64.deb", destination: "~/nxlog-ce_2.10.2150_ubuntu_trusty_amd64.deb"
    config.vm.provision "shell", inline: <<-SHELL
      apt update
      apt install -y software-properties-common
      echo "=========== instalacao OSQUERY ==========="
      export OSQUERY_KEY=1484120AC4E9F8A1A577AEEE97A80C63C9D8B80B
      apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys $OSQUERY_KEY
      sudo add-apt-repository 'deb [arch=amd64] https://pkg.osquery.io/deb deb main'

      mkdir -p /etc/apt/keyrings
      curl -L https://pkg.osquery.io/deb/pubkey.gpg | sudo tee /etc/apt/keyrings/osquery.asc
      add-apt-repository 'deb [arch=amd64 signed-by=/etc/apt/keyrings/osquery.asc] https://pkg.osquery.io/deb deb main'      
      apt-get update
      apt-get install -y osquery sshpass curl python python3
      # Match SSL Cert Hostname (fleet)
      echo "192.168.56.105    fleet" >> /etc/hosts

      JWT=$(curl --insecure 'https://192.168.56.105:8412/api/v1/fleet/login' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:90.0) Gecko/20100101 Firefox/90.0' -H 'Accept: application/json' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Referer: https://192.168.56.105:8412/login' -H 'Content-Type: application/json' -H 'Origin: https://192.168.56.105:8412' -H 'Connection: keep-alive' -H 'Sec-Fetch-Dest: empty' -H 'Sec-Fetch-Mode: cors' -H 'Sec-Fetch-Site: same-origin' -H 'Pragma: no-cache' -H 'Cache-Control: no-cache' -H 'TE: trailers' --data '{"email":"admin@detectionlab.network","password":"Fl33tpassword!"}' | python3 -c "import sys, json; print(json.load(sys.stdin)['token'])")

      curl --insecure 'https://192.168.56.105:8412/api/v1/fleet/config/certificate' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:90.0) Gecko/20100101 Firefox/90.0' -H 'Accept: application/json' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Content-Type: application/json' -H "Authorization: Bearer $JWT" -H 'Connection: keep-alive' -H 'Sec-Fetch-Dest: empty' -H 'Sec-Fetch-Mode: cors' -H 'Sec-Fetch-Site: same-origin' -H 'Pragma: no-cache' -H 'Cache-Control: no-cache' | python3 -c "import sys, json, base64; f=open('/tmp/fleet.pem', 'wb'); f.write(base64.b64decode(json.load(sys.stdin)['certificate_chain']))"

      mv /tmp/fleet.pem /etc/osquery/
      mv flagfile.txt /etc/osquery/osquery.flags
      mv secret.txt /etc/osquery/
      echo "restart osqueryd"
      systemctl restart osqueryd
      /etc/init.d/osqueryd stop
      /etc/init.d/osqueryd start

      echo "=========== instalacao NXLOG ==========="
      apt -y install libapr1 libdbi1 libperl5.18
      dpkg -i nxlog-ce_2.10.2150_ubuntu_trusty_amd64.deb
      /etc/init.d/nxlog stop
      mv nxlog.conf /etc/nxlog/
      /etc/init.d/nxlog start
      
      # Add UDP syslog input to splunk and syslog_output to ossec.conf
      # sshpass allows input of password on the command line
      echo "enable splunk udp"
      sshpass -p "vagrant" ssh -o StrictHostKeyChecking=no vagrant@192.168.56.105 "sudo /opt/splunk/bin/splunk add udp 514 -sourcetype syslog -index syslog -auth admin:changeme"
    SHELL
end
