SACI plugin for [zabbix] [1]
==============================

If you want Zabbix opening and closing tickets through [OTRS] [2] api, you want
install and test SACI plugin. 

**SACI** is resulted from need of integration between these great open source tools.

To make a multi scenario plugin, I'm using AppConfig perl module. 
If you can't or dont want install AppConfig perl module, , you can hack the SACI 
to make options hardcoded. 

Well, to make this magic works:

1. Install `SOAP::LITE`, `Data::Dumper` and `AppConfig` perl modules (distribution packages or by CPAN, your call here! )

2. Configure all saci variables and test using follow commands:

    *PROBLEM* and *OK* are your trigger subject given by zabbix {TRIGGER.STATUS) macro.

    The body of trigger must be *{HOSTNAME}:{TRIGGER.NAME}*.

    ```` bash
    $ perl saci saci.conf PROBLEM hostname%trigger_name 
    $ perl saci saci.conf OK hostname%trigger_name
    ````
ex:


perl saci saci.conf PROBLEM Teste host%Teste ping High%Disaster%192.168.1.1%yes%12:00:00%02/04/2016



3. Configure Zabbix action to use saci script.

4.Set your condition My config setted:


Type : Or

A   Value The Trigger = Problem
B   Value The Trigger = ok

4. In Command Remote set server your server Otrs

5. custom Script execute with agente mode (install agent mode in server otrs)

My custom Script in server 

perl /opt/saci/saci /etc/saci.conf {TRIGGER.STATUS} {HOST.NAME}%{TRIGGER.NAME}%{TRIGGER.SEVERITY}%{IPADDRESS}%{EVENT.ACK.STATUS}%{EVENT.TIME}%{EVENT.DATE} 


5. Configure the actions and be happy. =)


Work in OTRS 5 


[1]: http://zabbix.com "Zabbix"
[2]: http://otrs.org "OTRS"
