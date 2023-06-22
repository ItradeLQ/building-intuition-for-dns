<p align="center">
<a href="https://imgur.com/eULrHiA"><img src="https://i.imgur.com/eULrHiA.png" title="source: imgur.com" /></a>
</p>

<h1>Building Intuition for DNS(Azure)</h1>
In this tutorial we will observe A-Record, CNAME Record and Local DNS Cache.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- DNS

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)

<h2>List of Prerequisites</h2>

- Active Directory Installed
- Client and Domain Controller Connected

<a href="https://imgur.com/t80i1HB"><img src="https://i.imgur.com/t80i1HB.png" title="source: imgur.com" /></a>

The Domain Name System (DNS) is the phonebook of the Internet. DNS allows us to enter human-friendly domain names and be routed to websites such as Google's Public DNS  [https://dns.google/] with an IP address of [8.8.8.8]. We don't have to memorize alphanumeric IP addresses to visit this website thanks to DNS.

<h2>Actions and Observations</h2>
**A-Record Exercise**

![vivaldi_te0ncrjmC3](https://user-images.githubusercontent.com/109401839/213228476-10566ab6-eff5-467e-a836-76b21cc14b09.png)

1. **Connect/log into DC-1 as your domain admin account (mydomain.com\jane_admin)**
2. **Connect/log into Client-1 as an admin (mydomain\jane_admin)**
3. **From Client-1 try to ping “mainframe” notice that it fails**
4. **Nslookup “mainframe” notice that it fails (no DNS record)**
5. **Create a DNS A-record on DC-1 for “mainframe” and have it point to DC-1’s Private IP address


![2023-01-18 10 12 45 coursecareers com ef528124c90b](https://user-images.githubusercontent.com/109401839/213230206-6f8bb790-3ed4-4a81-b431-d84fd177b8b1.jpg)


> DNS Manager via Server Manager
> Forward Lookup
>Mydomain
>manualltyy create A record with ip of dc**
6. **Go back to Client-1 and try to ping it. Observe that it works**

![vivaldi_aRBUA6joTQ](https://user-images.githubusercontent.com/109401839/213231056-fb8de6ee-e1ca-4eba-8097-25dcf4268f60.png)

**Local DNS Cache Exercise**

1. **Go back to DC-1 and change mainframe’s record address to 8.8.8.8**

![vivaldi_kCL8ATV9xe](https://user-images.githubusercontent.com/109401839/213231797-93173e4c-eb96-4b2b-902e-090c37d38f2f.png)

2. Go back to Client-1 and ping “mainframe” again. Observe that it still pings the old address because it still exist in client cache so pings old ip address. The cache needs to flush in order to show the updated ip address

![vivaldi_b8guefs1IO](https://user-images.githubusercontent.com/109401839/213232169-7cbd4961-08e0-409c-acfb-bdb2c0c3904a.png)

3. **Observe the local dns cache (ipconfig /displaydns)**
4. **Flush the DNS cache (ipconfig /flushdns). Observe that the cache is empty**

![vivaldi_ngpZOpAny4](https://user-images.githubusercontent.com/109401839/213232520-8c9a7a92-b407-4b4f-89b7-844e25ff2e50.png)

5. **Attempt to ping “mainframe” again. Observe the address of the new record is showing up**

![vivaldi_mM61VFUhCE](https://user-images.githubusercontent.com/109401839/213232855-48f2d665-3370-4e88-a168-801d029033c9.png)

**CNAME Record Exercise**

![2023-01-18 10 20 47 camo githubusercontent com 352c35d7555a](https://user-images.githubusercontent.com/109401839/213233343-f7ff8421-db7d-4a62-a074-58e607ccada8.jpg)

1. Return to DC-1 and create a CNAME record that points the host “search” to “www.google.com”**
2. Return to Client-1 and attempt to ping “search”, observe the results of the CNAME record**

![vivaldi_8fNzswyh0V](https://user-images.githubusercontent.com/109401839/213233611-e5ed9231-42db-4b85-95d1-3f28f166416f.png)

3. On Client-1, nslookup “search”, observe the results of the CNAME record**

<a href="https://imgur.com/2m5vOsJ"><img src="https://i.imgur.com/2m5vOsJ.png" title="source: imgur.com" /></a>


**Root Hints**

What are Root Hints? Root hints are DNS data stored in a DNS server. The root hints provide a list of preliminary resource records that can be used by the DNS service to locate other DNS servers that are authoritative for the root of the DNS domain namespace tree. Simply put the omputer will use the list of Fully Qualified Domain Names to resolve names it doesn't know.

<a href="https://imgur.com/jSZVxL9"><img src="https://i.imgur.com/jSZVxL9.png" title="source: imgur.com" /></a>

Hope you enjoyed this turtorial.
