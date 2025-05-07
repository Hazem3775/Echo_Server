# Echo_Server
simple Echo server and Client server using Python 
Echo Server Implementation 
1. Objective The objective of this lab is to implement a basic TCP-based echo server using Python. The server processes messages received from a client based on the first character of each message and responds accordingly. Additionally, Wireshark is used to verify message exchange between the client and server.
2. Server Logic The server expects each message from the client to begin with a single-character command, followed by a string of letters. The command determines how the string is processed:
- A → Sort the string in descending alphabetical order.
- C → Sort the string in ascending alphabetical order.
- D → Convert all letters to uppercase, preserving the original order.
- Any other character → Echo the original message as-is.
3. Implementation
Server Code (Python)
import socket
Host = '127.0.0.1' Port = 65432
def process_message(message):
message = message.strip()
if not message:
return ""
command = message[0] content = message[1:] if command == 'A':
return ''.join(sorted(content, reverse=True)) elif command == 'C':
return ''.join(sorted(content)) elif command == 'D':
return content.upper() else:
return message
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM) s.bind((Host, Port))
s.listen(1) print(f"Server is listening on {Host}:{Port}...") conn, addr = s.accept()
print(f"Connection from: {addr}") while True:
data = conn.recv(1024) if not data:
break decoded = data.decode('utf-8') response = process_message(decoded) conn.sendall(response.encode())
conn.close() s.close()
Client Code (Python)
import socket
Host = '127.0.0.1' Port = 65432
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM) s.connect((Host, Port))
while True:
message = input("Enter your message: ").strip() if not message:
break s.sendall(message.encode()) data = s.recv(1024)
print(f"Received from server: {data.decode('utf-8')}")
4. Sample Output
Enter your message: Dhello Received from server: HELLO Enter your message: Apple Received from server: pple Enter your message: Cat Received from server: at Enter your message: Car Received from server: ar
5. Wireshark Verification Wireshark was used to capture and inspect traffic between the client and the server. The following observations were made:
- TCP packets were successfully exchanged between localhost ports.
- Data payloads matched the processed messages as expected.
- The filter `tcp.port == 65432` confirmed active communication on the assigned port.
Wireshark Screenshot Samples:
![image](https://github.com/user-attachments/assets/7fd9e9d5-52d1-4124-a09b-16d7b1fc0ca6)
![image](https://github.com/user-attachments/assets/761b716b-c3a3-4d1c-a158-a1e8983849c1)


