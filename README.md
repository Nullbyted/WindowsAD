<h1 align="center"> Active Directory Environment</h1>

<h2>Objective: </h2>
The purpose of this project is to demonstrate my skills in installing and configuring Windows Server and Windows Pro, as well as setting up an Active Directory and implementing various network services such as RAS NAT and DHCP. Through this project, I aim to provide clear and concise instructions on how to complete these tasks, making it easy for others to understand and follow. The goal is to inform and demonstrate my technical experience in the field of Windows server administration.

<br>
<br>
<br>
<h2> Table of Contents</h2>
  <ul style="font-size:10px">
      <li> Install Microsoft Server 2019 & Windows Pro (Virtual Box): *refer to InstallationGuide.md* </li>
      <li><a href="#Active"> Install Active Directory.</a> </li>
      <li><a href="#User"> User Creation Method 1 (GUI). </a> </li>
      <li><a href="#User2"> User Creation Method 2 (Powershell). </a></li>
      <li><a href="#User3"> User Creation Method 3 (Powershell Script).</a> </li>
      <li><a href="#Network"> Install DHCP and DNS Services- Private network.</a> </li>
      <li><a href="#Pro"> Windows Pro Client- Access Private Network @ Server.</a> </li>
      <li><a href="#File"> Install File and Printer Services. </a></li>
  </ul>
<br>
<br>
<h2 id="Active"> Installing Active Directory </h2>
<ul>
  <img  src="./pictures/assests/Virtualbox8.png" width=600 height=500>
  <li> Server Manager should automatically pop up. Click "Add roles and features"  </li>
  <li> at the Add roles window - click next until youu reach server roles and select "Active Directory Domain Services"  </li>
  <li> "Add Features"  </li>
  <img  src="./pictures/assests/Virtualbox9.png" width=600 height=500>
  <li>Click "Next" and finsih installation  </li>
  <li> Follow the next pictures to AD post Deployment  </li>
  <br>
  <img  src="./pictures/assests/Virtualbox10.png" width=600 height=500> | <img  src="./pictures/assests/Virtualbox11.png" width=600 height=500>
  <li> After you hit next you will be prompted to add a password, then click next all the way through until you reach install  </li>
  <li> Your VM should automatically restart when finsihed </li>
</ul>
<br>
<br>
<br>
<h2 id="User"> Active Directory User Creation Method 1 of 3</h2>
<ul>
  <li> Your Windows Server has now been promoted to a Domain Controller with the intallation of Active directory. </li>
  <img  src="./pictures/assests/Virtualbox12.png" width=800 height=500>
  <li> As you can see Active Directory (AD) has been allocated to Server manager. </li>
  <li> For user creation; proceed with Active Directory Users and Computers </li>
<br>  
  <li> We will start m=by making two Organizational Units (OU's) "adadmin" & "adusers" </li>
  <img  src="./pictures/assests/Virtualbox13.png" width=600 height=500> | <img  src="./pictures/assests/Virtualbox14.png" width=600 height=500> | <img  src="./pictures/assests/Virtualbox15.png" width=600 height=500>
<br>  
  <li> We will start by adding a user to "ADadmin". </li>
  <li> At the ADamin OU Right-click=>New=>User </li>
  <img  src="./pictures/assests/Virtualbox16.png" width=600 height=500>
 
  <li> The same process applied can be used for the Users OU </li>
  <li> ***NOTE: The Password policy is set to never expires, in production you would want to add a strong password policy*** </li>
<br>
  <li> Now lets give the user in our ADadmin OU administrator priveleges </li>
  <li> at user Right-click=>Properties=>Member-of=>Add </li>
  <img  src="./pictures/assests/Virtualbox17.png" width=600 height=500>
  <li> Now you sign out of the administrator account and login with the admin account you created </li>
</ul>
<br>
<br>
<br>
<h2 id="User2"> Active Directory User Creation Method 2 of 3</h2>
<ul>
  <li>In this method we will use Power shell to create a user </li>
  <img  src="./pictures/assests/Virtualbox18.png" width=600 height=500> | <img  src="./pictures/assests/Virtualbox19.png" width=900 height=300> | <img  src="./pictures/assests/Virtualbox20.png" width=900 height=300>
</ul>
<br>
<br>
<br>
<h2 id="User3"> Active Directory User Creation Method 3 of 3</h2>
<br>
</ul>
  <li> The last method is Automation with a Powershell Script </li>
  <li> This is geared towards an onboarding batch of more that 60 agents. Single agent creation will most likely be done through AD users and computers
  <li> Anywyas i will provide two script: First one is a non-secured script based of Jogn Matador. The second one is one that i made that is rudimental in nature but with no regard a more secure method of user creation.</li>
<br>
<h4> - Unsecure script </h4>
<pre>
$PASSWORD_FOR_USERS   = "Password1"
$USER_FIRST_LAST_LIST = Get-Content .\names.txt


$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
New-ADOrganizationalUnit -Name ADUSERS -ProtectedFromAccidentalDeletion $false

foreach ($n in $USER_FIRST_LAST_LIST) {
    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()
    $title = $n.split(" ")[2].ToLower()
    $email = $n.split(" ")]3].ToLower()
    $username = "$($first.Substring(0,1))$($last)".ToLower()
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor red
    
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -Email $email `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
}
</pre>
<h5>Analysis: </h5>
As we can see this string is not a secure method for user creation, but it works for this lab enviroment. Some of the flaws we can observe are:
<ol>
  <li> The variable "PASSWORD_FOR_USERS" is using the easiest password possible </li>
  <li> On the same password we are forcing this password as plain text </li>
  <li> The script created a new OU, but it was assigned the boolean False </li>
  <li> The script fills in all the attributes but all of them are using the username </li>
  <li> Password policy is set to never expire </li>
</ol>
<br>
All of these features are easily exploitable in production and should only be used to prove automation with powershell

<h4> Rudimental Secure script </h4>
<br>
<pre>
$filePath = Read-Host "Enter the path of the input file"
$defaultPassword = ConvertTo-SecureString "Password1" -AsPlainText -Force

$users = Get-Content $filePath

foreach ($user in $users) {
  $username = $user.Split(",")[0]
  $firstName = $user.Split(",")[1]
  $lastName = $user.Split(",")[2]
  $email = "$username@yourdomain.com"

  New-ADUser -Name "$firstName $lastName" -GivenName $firstName -Surname $lastName -UserPrincipalName $email -EmailAddress $email 
  -AccountPassword $defaultPassword -Enabled $true -ChangePasswordAtLogon $true -PasswordNeverExpires $false 
  -MaxPasswordAge "90.00:00:00" -Path "OU=Users,DC=yourdomain,DC=com"
}

Write-Host "User accounts created successfully."

</pre>
<h5>Analysis: </h5>
This script is rudimental in nature but it has some features worth metioning:
<ol>
  <li> The Password is set to "Password1" however the script forces the user to create a new one after firest login </li>
  <li> Password policy, as mentioned, makes the user change it after login and has a 90 day expire with no password recycling </li>
  <li> Script ensures that all attributes are distinct and must appear in the file where the names reside </li>
</ol>

Overall execcution will yeild these results:

  <img  src="./pictures/assests/Screenshot 2022-12-14 133618.png" width=600 height=500>
  
  
<h2 id="Network"> Install &c Configure DHCP, DNS, & RAS- Private Network </h2>

In this section, we will install the services mentioned above to provide a way for remote clients to access the server. The archtecture has the server act as a "router" to provide internet connectivity. If you recall, on the installation page we created the server image with two internet interfaces; one for the server to connect to my ISP and the other interface is for the client to connect to the server. However we are first adding an IP address to the private interface network with a Class B/C IPv4 Address * In my case I used a Class B Private IP address just to show that the subnet has enough addresses to support the amount of users we generated***, then we will install and configure the latter services.
<br>
<h3> Services BreakDown: </h3>
<br>
<ul>
  <li> DHCP- will allow for automatic IPv4 assignment to any valid connecting clients </li>
  <li> DNS- allows proper domain resolution when client access WAN </li>
  <li> RAS- actually allows the client to connect to the server </li>
</ul>

<br>
<h2> Private Network Configuration </h2>
<ul>
  <li>We need to add an asdress to the private network which will be part of the DHCP scope. You can choose between 172.16.0.1 (Class B Private) or (192.168.1.1) as valid address and a subnet of 255.255.255.0.</li>
</ul>
<br>
<br>
<img  src="./pictures/assests/Virtualbox23.png" width=600 height=500>
<img  src="./pictures/assests/Virtualbox24.png" width=600 height=500>
<img  src="./pictures/assests/Virtualbox25.png" width=600 height=500>
<br>
<ul>
  <li> For DNS you can either use the ip address of the external IP (the one that connects to ISP) or use the loopback address (127.0.0.1)
</ul>
<br>
<br>
<h3> Services and Configuration </h3>
<ul>
<li>The steps taken to install the services are the same as the ones we used to install Active Directory
</ul>
<br>
<ul>
  <img  src="./pictures/assests/Virtualbox21.png" width=600 height=500>
  <li> **You may want to install theese services seprately **</li>
  <img  src="./pictures/assests/Virtualbox21.png" width=600 height=500>
  <li> Make sure you add these features </li>
</ul>
<br>
<br>
<h4> DHCP Configuration</h4>
<ul> 
  <li> Once installed we can head to DCP Server Manager -->Tasks-->Add Roles and Features-->
</ul>
<br>
<br>

  
  






  
 
  
  
  












