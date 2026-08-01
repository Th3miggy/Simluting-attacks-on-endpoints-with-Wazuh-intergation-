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


