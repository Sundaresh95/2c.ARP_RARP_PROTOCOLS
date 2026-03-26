# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM - ARP
```
SERVER.PY:
import socket

# IP -> MAC table (for ARP)
arp_table = {
    "192.168.1.1": "AA:BB:CC:DD:EE:01",
    "192.168.1.2": "AA:BB:CC:DD:EE:02",
    "192.168.1.3": "AA:BB:CC:DD:EE:03"
}

# MAC -> IP table (for RARP)
rarp_table = {v: k for k, v in arp_table.items()}

host = "127.0.0.1"
port = 5000

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind((host, port))
server.listen(1)

print("Server is waiting for connection...")

conn, addr = server.accept()
print("Connected with", addr)

while True:
    data = conn.recv(1024).decode()

    if not data:
        break

    protocol, value = data.split(",")

    if protocol == "ARP":
        result = arp_table.get(value, "MAC Address Not Found")

    elif protocol == "RARP":
        result = rarp_table.get(value, "IP Address Not Found")

    else:
        result = "Invalid Request"

    conn.send(result.encode())

conn.close()
```
## OUPUT - ARP:

<img width="952" height="179" alt="image" src="https://github.com/user-attachments/assets/b3bbdef8-16b2-4b71-bf16-d4f351ff0342" />

## PROGRAM - RARP
```
CLINENT.PY:
import socket

host = "127.0.0.1"
port = 5000

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect((host, port))

print("1. ARP (IP → MAC)")
print("2. RARP (MAC → IP)")

choice = input("Enter choice: ")

if choice == "1":
    ip = input("Enter IP Address: ")
    message = "ARP," + ip

elif choice == "2":
    mac = input("Enter MAC Address: ")
    message = "RARP," + mac

else:
    print("Invalid choice")
    exit()

client.send(message.encode())

result = client.recv(1024).decode()

print("Result:", result)

client.close()
```
## OUPUT -RARP
<img width="681" height="257" alt="image" src="https://github.com/user-attachments/assets/d5f9a6f0-2a7e-4665-a768-e7430b66c19f" />

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
