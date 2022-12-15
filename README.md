<h3> SCOPE: </h3> 
<br />
-Active Directory User: 900+
<br />
      -Used a Powershell script to automate user creation and organizational unit at a User level access
      <br />
     -Created a separate organizational unit at an Admin level access
     <br />
     -The script fulfilled all import fields like
     <br />
          -Password: password policy
          <br />
          -First & Last name 
          <br />
          -Display name: using the naming convention of the first letter of the First Name and all the Last Name
          <br />
          -Employee ID: used Username of the sake of the project
-RAS/NAT
     -Remote Access Server was used to connect clients to the private networks using projectdomain.com
     -NAT is configured to provide Internet connectivity through a server (essentially an Internal network and an External network)
-DHCP:
     -Class B Private IP address to support more than the 255 private IP addresses that a Class C could support
     -The server uses a reserved IP address of 172.1*.*.1
     -All other clients will use a 172.1*.*.100-200 as a sample scope with a lease of 8 days 
Client Machine:
     -Client machine was created to prove proper functions of active directory domain, DHCP, and internet connectivity.
