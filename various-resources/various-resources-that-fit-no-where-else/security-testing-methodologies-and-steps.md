---
description: >-
  This is mostly a list for myself that others may too find useful. I took the
  basis of this from the OSSTMM version 2.1. You can find a PDF below.
---

# Security Testing Methodologies and Steps

[OSSTMMv2.1.pdf](http://tigerteam.se/dl/standards/osstmm.en.2.1.pdf)

### Logistics and Controls

#### Error Checking

1. Examine the route to the target network for packet loss using TCP.
2. Examine the route to the target network for packet loss using UDP.
3. Examine the route to the target network for packet loss using ICMP.
4. Measure the rate of packet round-trip time using TCP.
5. Measure TCP latency through TCP connections.
6. Measure the rate of packet acceptance and response on the target network.
7. Measure the amount of packet loss or connection denials at the target network.

#### Routing

1. Examine the routing path to the targets from the attack system.
2. Examine the routing path for the target’s ISPs.
3. Examine the routing path for the target ISPs primary Transit Sellers.
4. Examine the use of IPv6 to each live host in the network.

### Network Surveying

#### Name Server Responses

1. Examine Domain registry information for servers. 
2. Find IP block owned. 
3. Question the primary, secondary, and ISP name servers for hosts and sub domains.
4. Find IPv6 IP blocks in use though DNS queries.

#### Examine Outer Wall of Network

1. Use multiple traces to the gateway to define the outer network layer and routers.

#### Examine Tracks from the Target Organization

1. Search web logs and intrusion logs for system trails from the target network.
2. Search board and newsgroup postings for server trails back to the target network.

#### Information Leaks

1. Examine target web server source code and scripts for application servers and internal links.
2. Examine e-mail headers, bounced mails, and read receipts for the server trails.
3. Search newsgroups for posted information from the target.
4. Search job databases and newspapers for IT positions within the organization relating to hardware and software.
5. Search P2P services for connections into the target network and data concerning the organization.

### System Services Identification

####  Enumerate Systems

1. Collect broadcast responses from the network
2. Probe past the firewall with strategically set packet TTLs \(Firewalking\) for all IP addresses.
3. Use ICMP and reverse name lookups to determine the existence of all the machines in a network.
4. Use a TCP source port 80 and ACK on ports 3100-3150, 10001-10050, 33500-33550, and 50 random ports above 35000 for all hosts in the network.
5. Use TCP fragments in reverse order with FIN, NULL, and XMAS scans on ports 21, 22, 25, 80, and 443 for all hosts in the network.
6. Use a TCP SYN on ports 21, 22, 25, 80, and 443 for all hosts in the network.
7. Use DNS connect attempts on all hosts in the network.
8. Use FTP and Proxies to bounce scans to the inside of the DMZ for ports 22, 81, 111, 132, 137, and 161 for all hosts on the network.

#### Enumerating Ports

1. Use TCP SYN \(Half-Open\) scans to enumerate ports as being open, closed, or filtered on the default TCP testing ports for all the hosts in the network.
2. Use TCP full connect scans to scan all ports up to 1024 on all hosts in the network.
3. Use TCP fragments in reverse order to enumerate ports and services for the subset of ports on the default Packet Fragment testing ports in Appendix B for all hosts in the network.
4. Use UDP scans to enumerate ports as being open or closed on the default UDP testing ports if UDP is NOT being filtered already. \[Recommended: first test the packet filtering with a small subset of UDP ports.\]

#### Verifying Various Protocol Responses

1. Verify and examine the use of traffic and routing protocols.
2. Verify and examine the use of non-standard protocols.
3. Verify and examine the use of encrypted protocols.
4. Verify and examine the use of TCP and ICMP over IPv6.

#### Verifying Packet Level Responses

1. Identify TCP sequence predictability.
2. Identify TCP ISN sequence numbers predictability.
3. Identify IPID Sequence Generation predictability.
4. Identify system up-time.

#### Identifying Service

1. Match each open port to a service and protocol.
2. Identify server uptime to latest patch releases.
3. Identify the application behind the service and the patch level using banners or fingerprinting.
4. Verify the application to the system and the version.
5. Locate and identify service remapping or system redirects.
6. Identify the components of the listening service.
7. Use UDP-based service and Trojan requests to all the systems in the network.

#### Identifying Systems

1. Examine system responses to determine operating system type and patch level.
2. Examine application responses to determine operating system type and patch level.
3. Verify the TCP sequence number prediction for each live host on the network.
4. Search job postings for server and application information from the target.
5. Search tech bulletin boards and newsgroups for server and application information from the target.
6. Match information gathered to system responses for more accurate results.

### Competitive Intelligence Review

#### Business Intelligence

1. Map and measure the directory structure of the web servers.
2. Map the measure the directory structure of the FTP servers.
3. Examine the WHOIS database for business services relating to registered host names.
4. Determine the IT cost of the Internet infrastructure based on OS, Applications, and Hardware.
5. Determine the cost of support infrastructure based on regional salary requirements for IT professionals, job postings, number of personnel, published resumes, and responsibilities.
6. Measure the buzz \(feedback\) of the organization based on newsgroups, web boards, and industry feedback sites
7. Record the number of products being sold electronically \(for download.\)
8. Record the number of products found in P2P sources, wares sites, available cracks up to specific versions, and documentation both internal and third party about the products.
9. Identify the business partners.
10. Identify the customers from organizations to industry sectors.
11. Verify the clarity and ease of use of the merchandise purchasing process.
12. Verify the clarity and ease of use for merchandise return policy and process.
13. Verify that all agreements made over the Internet from digital signature to pressing a button which signifies acceptance of an end-user agreement can be repudiated immediately and for up to seven days.

### Privacy Review

* _List any disclosures_ 
* _List compliance failures between public policy and actual practice_ 
* _List systems involved in data gathering_ 
* _List data gathering techniques List data gathered_

#### Policy

1. Identify public privacy policy.
2. Identify web-based forms.
3. Identify database type and location for storing data.
4. Identify data collected by the organization.
5. Identify storage location of data.
6. Identify cookie types.
7. Identify cookie expiration times.
8. Identify information stored in cookie.
9. Verify cookie encryption methods.
10. Identify the clarity of opt-out information.
11. Identify the ease of use for opt-out.
12. Identify beacon gifs \(web bugs\) in web services and e-mails.
13. Identify server location of beacon gifs.
14. Identify web bug data gathered and returned to server.

#### False Light and Defamation

1. Identify fictionalized persons, organizations, institutions with real persons.
2. Identify persons or organizations portrayed in a negative manner.

#### Appropriation

1. Identify persons, organizations, or materials which as themselves or of a likeness thereof which is used for commercial reasons as in web sites or advertisements.

#### Disclosure of Private Facts

1. Identify information about employees persons, organizations, or materials which contain private information.

### Document Grinding

#### _Expected Results_

* _A profile of the organization._
* _A profile of the employees._
* _A profile of the organization's network._
* _A profile of the organization’s technologies._
* _A profile of the organization’s partners, alliances, and strategies._

1. Examine web databases and caches concerning the target organization and key people.
2. 3. Compile e-mail addresses from within the organization and personal e-mail addresses from key people.
4. Search job databases for skill sets technology hires need to possess in the target organization.
5. Search newsgroups for references to and submissions from within the organization and key people.
6. Search documents for hidden codes or revision data.
7. Examine P2P networks for references to and submissions from within the organization and key people.

### Vulnerability Research and Verification

#### _Expected Results_

* _Type of application or service by vulnerabilities_
* _Patch levels of systems and applications_
* _List of possible denial of service vulnerabilities_
* _List of areas secured by obscurity or visible access_
* _List of actual vulnerabilities minus false positives_
* _List of Internal or DMZ systems_
* _List of mail, server, and other naming conventions_
* _Network map_

1. Integrate the currently popular scanners, hacking tools, and exploits into the tests.
2. Measure the target organization against the currently popular scanning tools.
3. Attempt to determine vulnerability by system and application type.
4. Attempt to match vulnerabilities to services.
5. Attempt to determine application type and service by vulnerability.
6. Perform redundant testing with at least 2 automated vulnerability scanners.
7. Identify all vulnerabilities according to applications.
8. Identify all vulnerabilities according to operating systems.
9. Identify all vulnerabilities from similar or like systems that may also affect the target systems.
10. Verify all vulnerabilities found during the exploit research phase for false positives and false negatives.
11. Verify all positives \(be aware of your contract if you are attempting to intrude or might cause a denial of service\).

### Internet Application Testing

#### _Expected Results_

* _List of applications_
* _List of application components_
* _List of application vulnerabilities_
* _List of application system trust_

#### Re-Engineering 

1. Decompose or deconstruct the binary codes, if accessible.
2. Determines the protocol specification of the server/client application.
3. Guess program logic from the error/debug messages in the application outputs and program behaviors/performance.

#### Authentication 

1. Find possible brute force password guessing access points in the applications.
2. Find a valid login credentials with password grinding, if possible.
3. Bypass authentication system with spoofed tokens.
4. Bypass authentication system with replay authentication information.
5. Determine the application logic to maintain the authentication sessions - number of \(consecutive\) failure logins allowed, login timeout, etc.
6. Determine the limitations of access control in the applications - access permissions, login session duration, idle duration.

#### Session Management 

1. Determine the session management information - number of concurrent sessions, IP-based authentication, role-based authentication, identity-based authentication, cookie usage, session ID in URL encoding string, session ID in hidden HTML field variables, etc.
2. Guess the session ID sequence and format
3. Determine the session ID is maintained with IP address information; check if the same session information can be retried and reused in another machine.
4. Determine the session management limitations - bandwidth usages, file download/upload limitations, transaction limitations, etc.
5. Gather excessive information with direct URL, direct instruction, action sequence jumping and/or pages skipping.
6. Gather sensitive information with Man-In-the-Middle attacks.
7. Inject excess/bogus information with Session-Hijacking techniques.
8. Replay gathered information to fool the applications.

#### Input Manipulation

1. Find the limitations of the defined variables and protocol payload - data length, data type, construct format, etc.
2. Use exceptionally long character-strings to find buffer overflows vulnerability in the applications.
3. Concatenate commands in the input strings of the applications.
4. Inject SQL language in the input strings of database-tired web applications.
5. Examine "Cross-Site Scripting" in the web applications of the system.
6. Examine unauthorized directory/file access with path/directory traversal in the input strings of the applications.
7. Use specific URL-encoded strings and/or Unicode-encoded strings to bypass input validation mechanisms of the applications.
8. Execute remote commands through "Server Side Include".
9. Manipulate the session/persistent cookies to fool or modify the logic in the server-side web applications.
10. Manipulate the \(hidden\) field variable in the HTML forms to fool or modify the logic in the server-side web applications.
11. Manipulate the "Referrer", "Host", etc. HTTP Protocol variables to fool or modify the logic in the server-side web applications.
12. Use illogical/illegal input to test the application error-handling routines and to find useful debug/error messages from the applications.

#### Output Manipulation

1. Retrieve valuable information stored in the cookies.
2. Retrieve valuable information from the client application cache.
3. Retrieve valuable information stored in the serialized objects.
4. Retrieve valuable information stored in the temporary files and objects. Information Leakage.
5. Find useful information in hidden field variables of the HTML forms and comments in the HTML documents.
6. Examine the information contained in the application banners, usage instructions, welcome messages, farewell messages, application help messages, debug/error messages, etc.

### 

