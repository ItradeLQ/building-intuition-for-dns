<p align="center">
<a href="https://imgur.com/eULrHiA"><img src="https://i.imgur.com/eULrHiA.png" title="source: imgur.com" /></a>
</p>

<h1>Building Intuition for DNS(Azure)</h1>
In this tutorial, we will observe A-Record, CNAME Record and Local DNS Cache.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- DNS

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Windows Server 2022

<h2>List of Prerequisites</h2>

- Active Directory Installed
- Client and Domain Controller Connected

<a href="https://imgur.com/t80i1HB"><img src="https://i.imgur.com/t80i1HB.png" title="source: imgur.com" /></a>

The Domain Name System (DNS) is the phonebook of the Internet. DNS allows us to enter human-friendly domain names and be routed to websites such as Google's Public DNS  [https://dns.google/] with an IP address of [8.8.8.8]. We don't have to memorize alphanumeric IP addresses to visit this website thanks to DNS.

<h2>Actions and Observations</h2>
<p>A-Record Exercise</p>

![vivaldi_te0ncrjmC3](https://user-images.githubusercontent.com/109401839/213228476-10566ab6-eff5-467e-a836-76b21cc14b09.png)

1. **Connect/log into DC-1 as your domain admin account (mydomain.com\jane_admin)**
2. **Connect/log into Client-1 as an admin (mydomain\jane_admin)**
3. **From Client-1 try to ping “mainframe” notice that it fails**
4. **Nslookup “mainframe” notice that it fails (no DNS record)**
5. **Create a DNS A-record on DC-1 for “mainframe” and have it point to DC-1’s Private IP address**


<a href="https://imgur.com/vNTP30P"><img src="https://i.imgur.com/vNTP30P.png" title="source: imgur.com" /></a>


> DNS Manager via Server Manager
> Forward Lookup
> Mydomain
> Manually create A record with IP of DC-1
6. **Go back to Client-1 and try to ping it. Observe that it works**

<a href="https://imgur.com/poXy43P"><img src="https://i.imgur.com/poXy43P.png" title="source: imgur.com" /></a>

**Local DNS Cache Exercise**

1. **Go back to DC-1 and change mainframe’s record address to 8.8.8.8**

<a href="https://imgur.com/sBNbona"><img src="https://i.imgur.com/sBNbona.png" title="source: imgur.com" /></a>

2. Go back to Client-1 and ping “mainframe” again. Observe that it still pings the old address because it still exists in Client-1 cache so it pings old IP address. The cache needs to flush in order to show the updated IP address

![image 7](https://github.com/ItradeLQ/building-intuition-for-dns/assets/112427894/fba21707-e65b-4f27-bf38-a4ba286ac84d)

3. **Observe the local dns cache (ipconfig /displaydns)**

4. **Flush the DNS cache (ipconfig /flushdns). Observe that the cache is empty**

![image 8](https://github.com/ItradeLQ/building-intuition-for-dns/assets/112427894/a83547fb-a457-4272-adc4-888c55f796d1)

5. **Attempt to ping “mainframe” again. Observe the address of the new record is showing up**

![image 9](https://github.com/ItradeLQ/building-intuition-for-dns/assets/112427894/d039d119-70a3-4e28-9f31-bdf1973512b9)


**CNAME Record Exercise**

![image 10](https://github.com/ItradeLQ/building-intuition-for-dns/assets/112427894/db20e8e7-983f-4631-a214-aedad39628f4)

1. Return to DC-1 and create a CNAME record that points the host “search” to "www.google.com”
2. Return to Client-1 and attempt to ping “search”, observe the results of the CNAME record**

![image 11](https://github.com/ItradeLQ/building-intuition-for-dns/assets/112427894/dd3a1663-d882-49dd-a65c-1eccad7e9c2d)

3. On Client-1, nslookup “search”, observe the results of the CNAME record**

<a href="https://imgur.com/2m5vOsJ"><img src="https://i.imgur.com/2m5vOsJ.png" title="source: imgur.com" /></a>

In addition, if you try to browse https://search.mydomain.com it will give an error because it does not match www.google.com certificate.

![12 combined](https://github.com/ItradeLQ/building-intuition-for-dns/assets/112427894/1b78784a-4ed2-4610-b2ca-9fb32721f2f5)

**Root Hints**

What are Root Hints? Root hints are DNS data stored in a DNS server. The root hints provide a list of preliminary resource records that can be used by the DNS service to locate other DNS servers that are authoritative for the root of the DNS domain namespace tree. 
Simply put: from the tutorial, Client-1 relies on the records stored in DC-1 to resolve all name queries. The records that exist in DC-1 for this tutorial are inadequate to resolve all domain queries. However, when you search for a random address such as www.Twitter.com, it is able to use Root Hints to figure out the domain.

<a href="https://imgur.com/jSZVxL9"><img src="https://i.imgur.com/jSZVxL9.png" title="source: imgur.com" /></a>

Hope you enjoyed this tutorial.
