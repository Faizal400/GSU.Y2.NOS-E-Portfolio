# 📌 Week 2 - Lab Work: IP Addressing, Subnetting & DHCP  

**__Objective:__** Create tools to manipulate and analyse IP addresses.

## Exercise 1: Extend the script to calculate:
- Broadcast address
- First and last usable host addresses
- Number of usable hosts
- Change the IP address to have it CIDR prefix ( e.g. /24)
and compare resulting networks

### My python solution:
```python
import ipaddress

def analyse_ip(ip_str):
    ip = ipaddress.ip_interface(ip_str)
    
    print(f"IP Address: {ip.ip}")
    print(f"Network: {ip.network}")
    print(f"Netmask: {ip.netmask}")
    print(f"Broadcast Address: {ip.network.broadcast_address}")
    print(f"First Usable Host: {list(ip.network.hosts())[0]}")
    print(f"Last Usable Host: {list(ip.network.hosts())[-1]}")
    print(f"Total Usable Hosts: {ip.network.num_addresses - 2}")
    print(f"Is Private: {ip.ip.is_private}")
    print(f"Is Global: {ip.ip.is_global}")
```
**Example Usage:**
```python
analyse_ip("192.168.1.1/24")
```

## Exercise 2: Analyse your IP address to know if it is private/public and the network details. 

```python
import socket

hostname = socket.gethostname()
IPAddr = socket.gethostbyname(hostname)
analyse_ip(f"{IPAddr})
print("Your Computer Name:", hostname)
print("Your Computer IP Address:", IPAddr)

```
## Exercise 3: Get the university website IP address and analyse it. Hint you can use seminar of week2 to know how to get the address. 
```python
import socket
import ipaddress

def analyse_website_ip(website):
    try:
        ip_address = socket.gethostbyname(website)
        ip_obj = ipaddress.ip_address(ip_address)
        print(f"Website: {website}, IP Address: {ip_address}")
        analyse_ip(f"{ip_address}/24")
        if ip_obj.is_private:
            print("PRIVATE IP address.")
        else:
            print("PUBLIC IP address.")
        
        print("\nDetailed Analysis:")
        analyse_ip(f"{ip_address}/24")

    except socket.gaierror:
        print(f"Cant retrieve IP address for {website}")
```
# Example Usage
```python
analyse_website_ip("www.gold.ac.uk/")
```

## Exercise 4: (Challenge ) Create a subnetting plan for a company with 4 departments requiring the following hosts:
> - Engineering: 30 hosts
> - Marketing: 15 hosts
> - Finance: 10 Hosts
> - HR: 5 Hosts
> - Network Address: 172.16.0.0/16
![Img](https://github.com/user-attachments/assets/d14b7a62-4b05-4487-9044-eb33b50407dd)
## Exercise 5: (Challenge ) use the client sever example from previous week to have a TCP server as DHCP server and create a client that would get the IP address from the server following the process above.
[1] and [2]
### [1]  TCP Based DHCP Server
```python
import socket

# Simulated DHCP IP pool
dhcp_pool = ["192.168.1.100", "192.168.1.101", "192.168.1.102"]
leases = {}

def dhcp_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind(("0.0.0.0", 67))  # DHCP typically uses port 67
    server_socket.listen(5)
    
    print(" DHCP Server is running...")

    while True:
        client_socket, client_addr = server_socket.accept()
        print(f"Connection from {client_addr}")

        request = client_socket.recv(1024).decode()

        if request == "DHCP DISCOVER":
            if dhcp_pool:
                offered_ip = dhcp_pool.pop(0)
                print(f"Offering IP: {offered_ip} to {client_addr}")
                client_socket.send(f"DHCP OFFER {offered_ip}".encode())
            else:
                print(" No available IPs in the DHCP pool!")
                client_socket.send("DHCP NAK".encode())

        elif "DHCP REQUEST" in request:
            requested_ip = request.split()[-1]
            if requested_ip not in leases.values():
                leases[client_addr] = requested_ip
                print(f"{client_addr} assigned {requested_ip}")
                client_socket.send(f"DHCP ACK {requested_ip}".encode())
            else:
                print(f"IP {requested_ip} is already assigned!")
                client_socket.send("DHCP NAK".encode())

        client_socket.close()

# Run the server
dhcp_server()
```
### [2] Client

```python
import socket

def dhcp_client():
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect(("127.0.0.1", 67))  # Connect to DHCP server
    
    print("Sending DHCP DISCOVER...")
    client_socket.send("DHCP DISCOVER".encode())
    response = client_socket.recv(1024).decode()
    print(f"Server Response: {response}")

    if "DHCP OFFER" in response:
        offered_ip = response.split()[-1]
        
        client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        client_socket.connect(("127.0.0.1", 67))
        
        print(f"Sending DHCP REQUEST for {offered_ip}...")
        client_socket.send(f"DHCP REQUEST {offered_ip}".encode())
        final_response = client_socket.recv(1024).decode()
        print(f"Server Response: {final_response}")

    client_socket.close()

# Run the client
dhcp_client()
```
