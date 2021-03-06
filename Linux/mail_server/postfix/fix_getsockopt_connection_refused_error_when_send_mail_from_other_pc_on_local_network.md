
# Fix Getsockopt Connection Refused Error When Send Mail from Other PC on Local Network

#### Root Cause

In Postfix config file -- `etc/postfix/main.cf`:  

`inet_interfaces = localhost`

#### Solution

* Run `ifconfig` to get the local IP address(Ex: 15.15.1.1) of the PC runs `Postfix`

* Modify `/etc/postfix/main.cf`  

  * Set `inet_interfaces = IP of PC runs postfix, 127.0.0.1`
        
        `Ex: inet_interfaces = 15.15.1.1, 127.0.0.1`

  * Set `mynetworks = IP of another PC, 127.0.0.1`

        `Ex: mynetworks = 15.15.1.2, 127.0.0.1`

* Restart `Postfix`

        sudo systemctl stop postfix.service
        sudo systemctl start postfix.service
        sudo systemctl status postfix.service    

#### Another Solution
You can set it to ALL to allow Postfix to receive the mails from all interfaces.
It's NOT recommended because it may cause security problems.

        inet_interfaces = all

#### References
* [Allow PC on Local Network Send Mail to Postfix Server](https://github.com/northbright/Notes/blob/master/Linux/mail_server/postfix/allow_pc_on_local_network_send_mail_to_postfix_server.md)
* [Setup Postfix to Send Mail on Centos 7 Minimal](https://github.com/northbright/Notes/blob/master/Linux/mail_server/postfix/setup_postfix_to_send_mail_on_centos_minimal.md)