# 3b.CREATION FOR CHAT USING TCP SOCKETS
## AIM
To write a python program for creating Chat using TCP Sockets Links.
## ALGORITHM:
1. Import the necessary modules in python
2. Create a socket connection to using the socket module.
3. Send message to the client and receive the message from the client using the Socket module in
 server
4. Send and receive the message using the send function in socket.
## PROGRAM
```
#SERVER
import socket
import os

HOST = '127.0.0.1'  
PORT = 65432  

def send_file(filename, conn):
   if os.path.isfile(filename):
       conn.sendall(b'EXISTS')
       file_size = os.path.getsize(filename)
       conn.sendall(str(file_size).encode('utf-8'))
       client_response = conn.recv(1024).decode('utf-8')
       if client_response == 'READY':
           with open(filename, 'rb') as f:
               chunk = f.read(1024)
               while chunk:
                   conn.sendall(chunk)
                   chunk = f.read(1024)
           print(f"File '{filename}' sent successfully.")
   else:
       conn.sendall(b'NOT_EXISTS')

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as server_socket:
   server_socket.bind((HOST, PORT))
   server_socket.listen()

   print(f"File server is listening on {HOST}:{PORT}")
   while True:
       conn, addr = server_socket.accept()
       with conn:
           print(f"Connected by {addr}")

           filename = conn.recv(1024).decode('utf-8')
           print(f"Client requested file: {filename}")

           send_file(filename, conn)
```
```
#CLIENT
import socket

HOST = '127.0.0.1'  
PORT = 65432  


def receive_file(filename, conn):
   response = conn.recv(1024).decode('utf-8')

   if response == 'EXISTS':
       file_size = int(conn.recv(1024).decode('utf-8'))
       print(f"File '{filename}' exists on server, size: {file_size} bytes.")

       conn.sendall(b'READY')

       with open('received_' + filename, 'wb') as f:
           total_received = 0
           while total_received < file_size:
               data = conn.recv(1024)
               f.write(data)
               total_received += len(data)
               print(f"Received {total_received} of {file_size} bytes")

       print(f"File '{filename}' received and saved as 'received_{filename}'")
   else:
       print("File does not exist on the server.")

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as client_socket:
   client_socket.connect((HOST, PORT))

   filename = input("Enter the filename you want to download: ")
   client_socket.sendall(filename.encode('utf-8'))

   receive_file(filename, client_socket)
```
## OUPUT
![WhatsApp Image 2024-10-14 at 14 20 31_4f10571b](https://github.com/user-attachments/assets/ed627343-03af-4649-b2bb-8f69c5f67f0d)






![WhatsApp Image 2024-10-14 at 14 20 43_a8deb4aa](https://github.com/user-attachments/assets/e0f519e7-4c9f-49f9-a564-fefad45cade3)


## RESULT
Thus, the python program for creating Chat using TCP Sockets Links was successfully 
created and executed.
