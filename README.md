# simulating attacks on endpoints with Wazuh integration

## Objective

The plan for this lab is to target our victim machine with several different attacks and see how the attack trigger different events in our Wazuh manager. though the cource of this project we will se a brute force, shell shock, and a SQL injection attack. (notes: two things to remember if you are following with these projects number 1 these VM's are being tested on a isolated environment and should never be test on a productive device number two this project is starting under the impression that the Wazuh manager is already setup as well as the two VM's we are using)

### Skills Learned

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.
- Kail Linux and ubuntu machine for our attacker and victim VM's

## Steps

Ref 1: To get this project start up we need to make sure that our Victim machine is add as a agent on our manager. (again if need information on how to do that we have gone over it in several other projects) 

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2019-40-24.png?raw=true)

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2019-40-31.png?raw=true)

Ref 2: Next we will to good over to our victim machine and make sure we have an SSH running on port 22 for that will the following commands to get it installed and running

    $ sudo apt install openssh-server -y

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2019-58-23.png?raw=true)

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2019-59-09.png?raw=true)

Ref 3: Now for the next part we are going to slide over to our Kali machine (attacker) and we are going to run an Nmap scan on our target to look for open ports and what do you know we find SSH talking on port 22

    $ sudo nmap -sV -O 192.168.1.126
    
![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2020-05-52.png?raw=true)

Ref 4: Now that we have found the open port we can try to preform a brute force attack against the target using Hydra

        $ sudo hydra -l badguy -P /usr/share/wordlists/wfuzz/other/common.pass.txt ssh://192.168.1.126

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2020-06-08.png?raw=true)

Ref 5: Great! we can see above that the attack failed to find a password that worked but that's OK the point of the lab is to visualize the attack in Wazuh manager and now below we can see the number of failed login attempted that Wazuh recorded and we can also see that it was able to identify it has a brute force attack instead of just several failed login attempted.

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2020-09-01.png?raw=true)

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2020-09-25.png?raw=true)

Ref 6: Alright lets try another one. for the next attack we are going to do a SQl injection attack but first we need to return to our victim machine and install Apache2. make sure it allow though enabled firewall and making sure Apache is running

        S sudo apt update 
        $ sudo apt install apache2

        S sudo ufw app list
        $ sudo ufw allow 'apache'
        $ sudo ufw status

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2020-18-21.png?raw=true)

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2020-21-32.png?raw=true)

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2020-22-14.png?raw=true)

Ref 7: Once we are good to go there make sure to confirm your apache2 is up and running which you can do by using the curl command or just by type your IP address in a browser

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2021-11-50.png?raw=true)

Ref 8: Now that it is up. we need to edit /var/ossec/etc/ossec.conf file so that we can monitor the access log of our Apache server from Wazuh manager

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2020-32-32.png?raw=true)

Ref 9: Lets try our SQL attack and Once again we can see that attack of was not successful but once again Wazuh detected the attack and type of attack 

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2021-15-35.png?raw=true)

Ref 10: for our final attack lets try one that is super similar but different attack and see if Wazuh can detected attack. Using the command below we can launch the attack against our victim machine and of course it was unsuccessful but we can see that Wazuh once again detected the attack and determine the type of attack as a shell shock attack. 

        $ sudo curl -H "User-Agent: () { :; }; /bin/cat /etc/passwd" 192.168.1.126

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2021-27-39.png?raw=true)

![not-working](https://github.com/Th3miggy/Simluting-attacks-on-endpoints-with-Wazuh-intergation-/blob/main/Screenshot%20from%202026-07-29%2021-27-09.png?raw=true)

## Conclusion

This is only a few of proof of concept guide that Wazuh has available that being said it is definitely some of the more fun ones to do as well as being some of the best for seeing how Wazuh identify and intercept different attack and turns them in to events to view.

