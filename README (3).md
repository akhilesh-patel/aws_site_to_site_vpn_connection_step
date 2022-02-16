
# Project Title
# aws site to side vpn connection steps

# step 1 
create to vpc one -in  mumbai and another is singapore(customer end)

# step 2 
create on linux machine in both the vpc but region instance disable public ip 
security group(ssh,all tcp ,all icmp  anywhere)

# step 3
now go to mumbai region create vittul private gateway

# step4 now create customergateway 
enter public ip of singapur ec2_instance



# step 5
create side to side vpn connection add subnet of customer-end

# step 6 
now go to route table(mumbai region) routepropogation and enaled =>save

# step 7 
site to site vpn => download configure select generic type

# step 8
now go to singapore region take access of ec2 using putty run the command 



###############################################################
1.  Commands for Installation of Openswan

    i. Change to root user: 

                $ sudo su

    ii. Install openswan:

                $ yum install openswan -y

    iii. In /etc/ipsec.conf uncomment following line if not already 
         
          uncommented:

                 include /etc/ipsec.d/*.conf

    iv. Update /etc/sysctl.conf to have following

 net.ipv4.ip_forward = 1

 net.ipv4.conf.all.accept_redirects = 0

 net.ipv4.conf.all.send_redirects = 0

    v. Restart network service:

                 $ service network restart

2. Command for /etc/ipsec.d/aws-vpn.conf

conn Tunnel1

        authby=secret
        auto=start
        left=%defaultroute
        leftid=Customer end Gateway VPN public IP
        right=AWS Virtual private gateway ID- public IP
        type=tunnel
        ikelifetime=8h
        keylife=1h
        phase2alg=aes128-sha1;modp1024
        ike=aes128-sha1;modp1024
        keyingtries=%forever
        keyexchange=ike
        leftsubnet=Customer end VPN CIDR
        rightsubnet=AWS end VPN CIDR
        dpddelay=10
        dpdtimeout=30
        dpdaction=restart_by_peer


3. Contents for  /etc/ipsec.d/aws-vpn.secrets

customer_public_ip aws_vgw_public_ip: PSK "shared secret"


4. Commands to enable/start ipsec service

           $ chkconfig ipsec on
           $ service ipsec start
           $ service ipsec status
           
           
    ## TUNNEL STATUS
           
    



![Screenshot (849)](https://user-images.githubusercontent.com/64592542/148192088-d61a1b38-c1d0-4a8e-af4d-d38d13482cff.png)
