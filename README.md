# Analyzing Network Layer Communication

## Objective  
Analyze DNS and ICMP traffic in transit using data from a network protocol analyzer tool.

## Scenario
Several customers of clients reported that they were not able to access the client company website www.yummyrecipesforme.com, and saw the error “destination port unreachable” after waiting for the page to load.
Tasked with analyzing the situation to determine which network protocol was affected during this incident. Attempt to visit the website and received the error “destination port unreachable.” To troubleshoot the issue, tcpdump was utilized to attempt to load the webpage again. To load the webpage, the browser sends a query to a DNS server through the UDP protocol to retrieve the IP address for the website's domain name. The browser then uses the IP address as the destination IP for sending an HTTPS request to the web server to display the webpage. The analyzer shows that when UDP packets are sent to the DNS server, ICMP packets are received containing the error message: “udp port 53 unreachable.”
![LKXsnNIhT0e1mAz5AEvxog_d363c94e0a4f4a8b90b0be403f6ee1f1_mMBaLWLyXG2omYBcSdjuR8y5_S59zow1ZEPYdjNyJzA1B0r55nI9KmDosI8QHXcEwE51NxM3N5gNtMgSOyVDHyJVLZvZA7_jJtkzUKfxuqFUJPHs57vVVES-LbG5teR8eir4idaqsxFaYJhhVJZn-a_S-txb7zQNIZq07XESgSkqDHuzfvALfYk3lipGVBY](https://github.com/user-attachments/assets/71e39346-4bcc-4239-bcd8-a53b57f3fdb7)

## Following Information From tcpdump Log
- The first two lines of the log file show the initial outgoing request from computer to the DNS server requesting the IP address of yummyrecipesforme.com. This request is sent in a UDP packet.
- The third and fourth lines of the log show the response to the UDP packet. The ICMP 203.0.113.2 line is the start of the error message indicating that the UDP packet was undeliverable to port 53 of the DNS server.
- In front of each request and response, the timestamps indicate when the incident happened. In the log, this is the first sequence of numbers displayed: 13:24:32.192571. Time is 1:24 p.m., 32.192571 seconds.
- After the timestamps, the source and destination IP addresses are present. In the first line, the UDP packet travels from the browser to the DNS server, this information is displayed as: 192.51.100.15 > 203.0.113.2.domain. The IP address to the left of the greater than (>) symbol is the source address, which in this example is the computer’s IP address. The IP address to the right of the greater than (>) symbol is the destination IP address. In this case, it is the IP address for the DNS server: 203.0.113.2.domain. For the ICMP error response, the source address is 203.0.113.2 and the destination is the computers IP address 192.51.100.15.
- After the source and destination IP addresses, there can be a number of additional details like the protocol, port number of the source, and flags. In the first line of the error log, the query identification number appears as: 35084. The plus sign after the query identification number indicates there are flags associated with the UDP message. The "A?" indicates a flag associated with the DNS request for an A record, where an A record maps a domain name to an IP address. The third line displays the protocol of the response message to the browser: "ICMP," which is followed by an ICMP error message.
- The error message, "udp port 53 unreachable" is mentioned in the last line. Port 53 is a port for DNS service. The word "unreachable" in the message indicates the UDP message requesting an IP address for the domain "www.yummyrecipesforme.com" did not go through to the DNS server because no service was listening on the receiving DNS port.
- The remaining lines in the log indicate that ICMP packets were sent two more times, but the same delivery error was received both times. 

## Incident Report: Network Traffic Analysis
- As part of the DNS protocol, the UDP protocol was used to contact the DNS server to retrieve the IP address for the domain name of yummyrecipesforme.com. The ICMP protocol was used to respond with an error message, indicating issues contacting the DNS server.
- The UDP message going from the browser to the DNS server is shown in the first two lines of every log event. The ICMP error response from the DNS server to the browser is displayed in the third and fourth lines of every log event with the error message, “udp port 53 unreachable.” Since port 53 is associated with DNS protocol traffic, this is an issue with the DNS server. Issues with performing the DNS protocol are further evident because the plus sign after the query identification number 35084 indicates flags with the UDP message and the “A?” symbol indicates flags with performing DNS protocol operations.
- Due to the ICMP error response message about port 53, it is highly likely that the DNS server is not responding. This assumption is further supported by the flags associated with the outgoing UDP message and domain name retrieval.
- The incident occurred today at 1:24 p.m.
- Customers notified the organization that they received the message “destination port unreachable” when they attempted to visit the website yummyrecipesforme.com.
- The cybersecurity team providing IT services to their client organization are currently investigating the issue so customers can access the website again.
- Conducted packet sniffing tests using tcpdump during investigation. In the resulting log file, DNS port 53 was unreachable.
- The next step is to identify whether the DNS server is down or traffic to port 53 is blocked by the firewall.
- DNS server might be down due to a successful Denial of Service attack or a misconfiguration.

### Skills Learned

- Identified and utilized network protocol in assessment of cybersecurity incident.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- tcpdump
