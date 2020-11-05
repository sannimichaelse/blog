## The Phonebook of the Internet - DNS

![dns-lookup.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604128454353/6i55H8U97.png)

# Introduction

Have you ever wondered what happens behind the scene after typing a URL on your web browser? how does my browser locate a website even if it's in the farthest country?  Well, you are not alone. In this article, we will be dishing some of those myths and understand what's really happening behind the scenes

Wait a minute, did you remember what you did the last time you wanted to call your mum, or siblings or girlfriend? You simply went to your phonebook/contact and search for the name, then you were able to access the number to call. Just like your phone contact maps your friend's name to their number, DNS maps domain name to an IP Address. 

# What is DNS

**The Domain Name System (DNS) is the phonebook of the Internet**. Humans access websites online by typing the URL, but web browsers interact through Internet Protocol (IP) addresses. Each device connected to the internet has a unique IP Address which is used to identify the device.

DNS translates domain names(e.g google.com) to IP addresses(e.g 192.168.1.1). The Domain Name System eliminates the need for humans to memorize IP addresses. You just type the name and it handles the rest

# How does DNS work under the hood

When you type a URL on your web browser, it goes through several systems(DNS Servers) to identify the location of the resource on the internet. It is important to understand the systems a DNS lookup from the browser goes through to be able to understand what happens under the hood. 

NB: All the communication between the web browser and the DNS Servers happens behind the scenes and has no need for human interaction. The DNS Lookup usually happen in a split second

![lookup.png](https://www.appneta.com/images/network-monitoring/dns-example.png)

# DNS Servers

### DNS Resolver 
A recursive resolver is the first stop in a DNS Lookup. 

The DNS Resolver contacted by your computer is usually chosen by your ISP (Internet service provider). However, you can configure your network to use a different DNS resolver, if you choose. This configuration can be modified in your operating system's network settings, or in the administration interface of your home network router

The DNS Resolver acts as the intermediary between the client and the other DNS Servers. It receives requests from the client and forwards it to the Root Name Server. When a resolver receives the DNS query, it will first check its own cache memory, to find an IP address for **example.com**. If it can't find it, it will send the query to the next level, which is the Root Name Server. 

### Root Name Server

The Root Name Server or Root Server is at the top of the DNS Hierarchy. There are 13 sets of these root servers, and they are strategically placed around the world and they are operated by 12 different organizations. You can check them out [here](https://www.iana.org/domains/root/servers#:~:text=The%20authoritative%20name%20servers%20that,13%20named%20authorities%2C%20as%20follows.). 

Each set of these root servers has its own unique IP address. The Root Server does not usually know where the IP address is, but knows where to direct the Recursive resolver. Just like you would ask someone for direction to a particular location. Sometimes, they might not know the exact location but can direct you to where to go next to find your destination. 

The Root Name Server will respond with the IP address of the Top Level Domain

### Top Level Domain Name Server
The Top-level domain name server stores the address information for top-level domains such as **.com**, **.net**, **.org** etc.  It also doesn't store IP addresses also but has information about the top-level domain for the particular URL. Because the TLD server doesn't know the IP address of **example.com**, it will respond with the Authoritative Name Server for **example.com**

### Authoritave Name Servers
The authoritative name servers are responsible for knowing everything about the domain, which includes the IP address. They are the final authority in the DNS hierarchy. So when the authoritative name server receives a query from the DNS resolver, the authoritative name server will respond with the IP address of **example.com**. And finally, the resolver will tell your computer(browser) the IP address of **example.com**, then your computer can now retrieve the web page for **example.com**

It is important to know that once the DNS resolver receives the IP address from the Authoritative name server, it will store it in its cache memory in case it receives another query for **example.com** so it doesn't have to go through all the steps again

# Conclusion

These are the steps a typical DNS Lookup or DNS query goes through to be able to load a web page or identify a resource on the internet. These operations and processes usually happen under the hood without any human interaction in a split second. You can check this [interactive explanation](https://www.verisign.com/en_US/website-presence/online/how-dns-works/index.xhtml) for more information about how DNS works. 

If you enjoyed and learned something from this article, don't forget to like and share it with your friends. 


