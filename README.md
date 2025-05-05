# Installation Configuration of Suricata IDS / IPS

<h2>Overview</h2>
This project focuses on installing and configuring Suricata, a powerful open-source network threat detection engine, on my Linux machine. The goal is to go through the installation and configuration process step by step<!--more-->, ensuring a smooth setup for real-time network monitoring and intrusion detection. By completing this project, I will gain hands-on experience with Suricata’s core functionalities, including traffic analysis, rule configuration, and log management. The final setup will enable efficient detection of malicious traffic patterns and potential security threats. This project aligns with cybersecurity best practices, enhancing my ability to secure and monitor network environments effectively
<h2>Introduction</h2>
Suricata is an advanced Intrusion Detection System <strong>(IDS)</strong>, Intrusion Prevention System <strong>(IPS)</strong>, and Network Security Monitoring <strong>(NSM)</strong> tool that analyzes network traffic for potential threats. It is designed for high performance, leveraging multi-threading and deep packet inspection to detect malicious activities in real time. Suricata supports industry-standard rule sets such as Emerging Threats and Snort rules, allowing for flexible and customizable security policies.
<h3>Step 1</h3>
1. To upgrade my repositories - sudo apt-get update &amp;&amp; sudo apt-getupgrade -y

2. To install the prerequisite for suricata - <strong>Sudo apt -y install libnetfilter-queue-dev libnetfilter-queue1 libnfnetlink-dev libnfnetlink0 jq</strong>

<img class="alignnone wp-image-50 size-full" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-208.png" alt="" width="643" height="286" />

3. To install suricata - <strong>sudo apt install suricata</strong>

<img class="alignnone wp-image-51 size-full" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-209.png" alt="" width="805" height="344" />
<h3>Step 2 - <strong>To enable Suricata service</strong></h3>
sudo systemctl enable suricata.service

<img class="alignnone wp-image-52 size-full" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-210.png" alt="" width="662" height="304" />

<strong>To check the status of suricata</strong> - sudo systemctl status suricata.service

<img class="alignnone size-full wp-image-53" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-211.png" alt="" width="693" height="313" />

To fully use suricata, first edit Suricata file. To do this, enter this directory by entering: <strong>sudo nano /etc/suricata/suricata.yaml</strong>

<img class="alignnone size-full wp-image-54" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-213.png" alt="" width="646" height="462" />

To check the interface I want to monitor, I hold ctrl+w and type interface. By default, suricata is monitoring eth0.

<img class="alignnone size-full wp-image-55" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-215.png" alt="" width="653" height="457" />

To find out the interface my network is on, I exit and return to my terminal and i enter - <strong>ip a or ifconfig
</strong>

<img class="alignnone size-full wp-image-56" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-214.png" alt="" width="656" height="301" />

It turns out the Interface is already monitoring "eth0" which is the interface my IP address is so this means that suricate will be automatically be monitoring that interface
<h3><strong>Step 3 - Surircata Rules</strong></h3>
<strong>Sudo suricata-update list-sources</strong>

<img class="alignnone size-full wp-image-57" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-217.png" alt="" width="672" height="444" />

To add the first rule I highlighted, (Emerging Threats Open Ruleset) I need to get it into my instance

To add this ruleset:
<strong>sudo suricata-update enable-source et/open</strong>

<img class="alignnone size-full wp-image-59" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-218-1.png" alt="" width="635" height="385" />

To update suricata to include the rule that was just added:
<strong>sudo suricata-update</strong>

<img class="alignnone size-full wp-image-60" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-221.png" alt="" width="651" height="400" />

Once Suricata has finished updating, you can find suricata rule under the following directory var/lib/suricata. I navigated to this directory and listed the files in the directory.

<img class="alignnone size-full wp-image-61" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-222.png" alt="" width="629" height="247" />

This rules directory is owned by root user

<img class="alignnone size-full wp-image-62" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-223.png" alt="" width="673" height="187" />

So I need to change into the root user, to go into the rules directory

<strong>sudo su</strong>
then password

<img class="alignnone size-full wp-image-63" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-224.png" alt="" width="589" height="192" />

Now i can changed into the directory of rules:

<strong>cd rules</strong>
<strong>ls -la</strong>

<img class="alignnone size-full wp-image-65" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-227.png" alt="" width="664" height="189" />

I cat the <strong>Suricata.rules</strong> file, this is a large file, so i will pipe it to less

<img class="alignnone size-full wp-image-64" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-226.png" alt="" width="646" height="445" />
<h3>Step 4 - Testing Suricata</h3>
Exit from the root user and enter:
<strong>sudo suricata -T -c /etc/suricata/suricata.yaml -v</strong>

What this command is doing is to run suricata with -T flag which means to run suricata in test mode, the -c is telling suricata to use the Configuration file while -v is Verbose mode

<img class="alignnone size-full wp-image-67" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-229.png" alt="" width="651" height="413" />

The <em>Configuration provided was successfully loaded</em> indicated that suricata is running as expected
<h3><strong>Step 5</strong> -  Run Sucicata by starting its service</h3>
<strong>Sudo systemctl start suricata.service</strong>

<img class="alignnone size-full wp-image-68" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-230.png" alt="" width="664" height="200" />

<strong>sudo systemctl status suricata.service</strong> (From the screenshot below it is active and running)

<img class="alignnone size-full wp-image-69" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-231.png" alt="" width="644" height="352" />

Before testing whether suricata is working, I changed the directory to <strong>cd /var/log/suricata</strong>

<img class="alignnone size-full wp-image-70" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-232.png" alt="" width="675" height="228" />

After navigating to this directory, I listed the files -<strong> ls</strong>

<img class="alignnone size-full wp-image-71" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-233.png" alt="" width="639" height="179" />

I cat the fast.log file but nothing displayed because Suricata has not generated any alert yet

To see whether the Suricata instance is working without using the test mode, i navigated to a test site to generate a telemetry;

<strong>curl http://testmynids.org/uid/index.html</strong>
(This would generate an alert for suricata)

<img class="alignnone size-full wp-image-72" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-234.png" alt="" width="651" height="223" />

To monitor suricata log in real time
<strong>cat fast.log</strong>

<img class="alignnone size-full wp-image-73" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-237.png" alt="" width="677" height="237" />

To get more details
<strong>cat eve.json</strong>

<img class="alignnone size-full wp-image-74" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/03/Screenshot-236.png" alt="" width="658" height="479" />

&nbsp;
