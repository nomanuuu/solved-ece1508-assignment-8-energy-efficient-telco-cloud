Download Link: https://assignmentchef.com/product/solved-ece1508-assignment-8-energy-efficient-telco-cloud
<br>



<strong> </strong>

<strong><u>Note:</u></strong> This a team work assignment. Each team (3 or 4 students) will work on a dedicated blade server. The students register their names (or emails) at following URL:

<a href="https://docs.google.com/spreadsheets/d/1WXikPNqUSScsOtX4oPX3Tup8LCUUA-DtYgxaXHld_D8">https://docs.google.com/spreadsheets/d/1WXikPNqUSScsOtX4oPX3Tup8LCUUA</a><a href="https://docs.google.com/spreadsheets/d/1WXikPNqUSScsOtX4oPX3Tup8LCUUA-DtYgxaXHld_D8">–</a>

<a href="https://docs.google.com/spreadsheets/d/1WXikPNqUSScsOtX4oPX3Tup8LCUUA-DtYgxaXHld_D8">DtYgxaXHld_D8</a>

<ol>

 <li><strong> <u>Connecting to an Ericsson blade server (EBS)</u> </strong></li>

</ol>

OpenVPN is required to connect to the experimental environment. Download and install OpenVPN client from:  <a href="https://openvpn.net/index.php/open-source/downloads.html">https://openvpn.net/index.php/open</a><a href="https://openvpn.net/index.php/open-source/downloads.html">–</a><a href="https://openvpn.net/index.php/open-source/downloads.html">source/downloads.html</a>

Each team will receive following information:

<ul>

 <li>OpenVPN key files (sde-xxx.ovpn) for each member to access the system</li>

 <li>Private key file “create-netsoft.ppk” (or “create-netsoft.pem” in case you use Linux) to open a SSH connection to an EBS</li>

 <li>IP and credential information of the EBS and shifted OpenStack platform.</li>

</ul>

In order to open a VPN connection, copy the provided keyfile to OpenVPN client config folder. In the Windows environment, it is:

C:Program FilesOpenVPNconfig

Launch OpenVPN to connect to the VPN network:




Launch a SSH client (e.g., putty) to connect to the Ericsson blade server. The putty software can be downloaded from:

<a href="https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html">https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html</a>




Import the SSH private key of the blade (“create-netsoft.ppk” file), as follows:




And then click “Open” to connect to the blade with provided EBS credential information:







When you are in the blade, create a folder to save your work.

#mkdir your_group_name

<ol start="2">

 <li><strong><u>Connecting to OpenStack</u> </strong></li>

</ol>

To connect to OpenStack on your blade server, simply open a browser, and point to: <a href="http://your_blade_ip/">http://your_blade_ip</a> <a href="http://your_blade_ip/">w</a>ith the provided credential information. In the previous example, it should be:  <a href="http://172.29.2.11/">http://172.29.2.11/</a>

Refer to the previous lab for the manual of OpenStack.

<ol start="3">

 <li><strong><u>Helper commands</u> </strong></li>

</ol>

The following commands may be required to achieve your assignement.

<table width="590">

 <tbody>

  <tr>

   <td width="253"><strong>Command </strong></td>

   <td width="337"><strong>Utility </strong></td>

  </tr>

  <tr>

   <td width="253">#sudo apt-get install cpufrequtils</td>

   <td width="337">Install the cpufrequtils package to change CPU frequency if it is not yet available in your blade</td>

  </tr>

  <tr>

   <td width="253">#cpufreq-info</td>

   <td width="337">Determine the current frequency of each CPU by the command</td>

  </tr>

  <tr>

   <td width="253">#sudo apt-get install sysstat</td>

   <td width="337">Install the systat package to collect CPU utilization</td>

  </tr>

  <tr>

   <td width="253">#mpstat -P ALL</td>

   <td width="337">Determine the load of the CPUs in the blade</td>

  </tr>

  <tr>

   <td width="253">#cat/sys/devices/system/cpu/cpuX/cpufreq/scaling_available_frequencies</td>

   <td width="337">Determine the available frequencies of the CPU X (0 ≤ X ≤ 11)</td>

  </tr>

  <tr>

   <td width="253">#sudo sh -c “echo -n “userspace” &gt;/sys/devices/system/cpu/cpuX/cpufreq/scaling_governor”#sudo sh -c “echo -n YYYY &gt;/sys/devices/system/cpu/cpuX/cpufreq/scaling_setspeed”</td>

   <td width="337">Changing the frequency of the CPU X(0 ≤ X ≤ 11)YYYY is an available frequency (see the command above)</td>

  </tr>

  <tr>

   <td width="253">#sudo sh -c “echo -n 0 &gt;/sys/devices/system/cpu/cpuX/online”</td>

   <td width="337">Disable the CPU X (1 ≤ X ≤ 11)<strong><u>Please do not disable the CPU 0</u></strong></td>

  </tr>

  <tr>

   <td width="253">#sudo sh -c “echo -n 1 &gt;/sys/devices/system/cpu/cpuX/online”</td>

   <td width="337">Activate the CPU X (1 ≤ X ≤ 11)</td>

  </tr>

  <tr>

   <td width="253">#sudo eri-ipmitool rns 0 4 5 68 69</td>

   <td width="337">Collect data from temperature, voltage, and current sensors</td>

  </tr>

  <tr>

   <td width="253"># sudo apt-get install python-heatclient</td>

   <td width="337">Install the command-line OpenStack Heat client</td>

  </tr>

 </tbody>

</table>




<ol start="4">

 <li><strong><u>Assignment (50 points)</u> </strong></li>

</ol>

You are required to implement five small scripts as follows. The scripts would be written in Python or in shell (e.g., bash) languages. You may also combine the scripts into a single file if it is necessary.

<ol>

 <li>(15 points) Write a script <strong>CollectE</strong> to collect data from the blade server in every second. Data can be saved into a .csv file, including the following fields (fields are separated by “;”):

  <ul>

   <li>Timestamp</li>

   <li>Temperature</li>

   <li>Power1</li>

   <li>Power2</li>

   <li>Total power</li>

   <li>CPU<sub>0</sub> utilisation (%)  CPU<sub>0</sub> frequency</li>

   <li>CPU<sub>1</sub> utilisation (%)</li>

   <li>CPU<sub>1</sub> frequency</li>

   <li>…</li>

   <li>CPU<sub>11 </sub>utilisation (%)</li>

   <li>CPU<sub>11 </sub>frequency</li>

  </ul></li>

</ol>

The script should be run in the background to collect data in real-time.

<strong><u>Recall</u></strong>: Power1 = Voltage1 x Current1; Power2= Voltage2 x Current2; Total power = Power1 + Power2.




<ol start="2">

 <li>(5 points) Write a script <strong>CreateE</strong> to create a <strong><u>random</u></strong> number N of VMs (you may freely choose the bounds of N, for example: 1 ≤ N ≤ 8). All VMs are created from the same flavour (for example: tempest2, 1 vCPU, 512M RAM, 1G root disk). It is recommended to use OpenStack HEAT to create the VMs.</li>

</ol>




<ol start="3">

 <li>(5 points) Write a script <strong>ArrivalE</strong> that launches the script in (2) in a <strong><u>random</u></strong> interval of time λ (you may freely choose the bounds of λ, for example: 1min ≤ λ ≤ 5min). Run this script within a time T (T = 30 minutes).</li>

</ol>




<ol start="4">

 <li>(5 points) Write a script <strong>ServE</strong> that destroys the VMs created in (3) in after random interval of time μ (you may freely choose the bounds of μ, for example: 1min ≤ μ ≤ 5min). Because of the randomness in (3) and (4), there could be multiple VMs coexisting in the system in the same time. The script in (4) will end when <strong>ArrivalE</strong> stopped and all the VMs have been destroyed.</li>

</ol>




<ol start="5">

 <li>(10 points) Write a script <strong>AdaptE</strong> that adapts the CPU frequency and active/disabled state to save energy consumption. The script may check the CPU utilization, and change it to an optimal frequency according to the requests of creating VMs.</li>

</ol>




<ol start="6">

 <li>(10 points) You are required to discuss the following items in a report. Please use graphical charts to illustrate your statement.</li>

 <li>What is the average creation time (λ)?</li>

 <li>What is the average running time of a VM (μ)? In average, how many VMs are there in the system?</li>

 <li>In average, how much energy does a VM consume? How much energy does it consume for each phase: creation, running, destruction? What is the average temperature in each phase?</li>

 <li>How does your energy adaptation strategy work?</li>

 <li>What is your saving energy (compared to a case in which no adaptation is implemented)?</li>

 <li>Propose a model of energy consumption and temperature according to the number of VMs for the: i) regular case (no adaptation), and ii) your proposed adaptation strategy.</li>

</ol>