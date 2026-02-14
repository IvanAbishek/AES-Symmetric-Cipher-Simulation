# AES-Symmetric-Cipher-Simulation
Built a Python client–server project demonstrating secure communication using AES symmetric encryption. The server generates a key, the client encrypts messages, and the server decrypts them successfully. Tested between two devices on the same network.

## Description:
This project demonstrates secure communication using AES symmetric encryption in a Python client–server model.
The server generates a random AES key and waits for a client connection. The client receives the key, encrypts a message, and sends the encrypted data back. The server decrypts the message to verify secure transmission.

Key Learning Outcomes:
- Understanding AES encryption process
- Working with Python socket programming
- Implementing symmetric key cryptography
- Handling padding and byte conversion


Technologies Used:
- Python
- PyCryptodome
- Socket Programming


## Features
- AES Symmetric Encryption
- Client–Server Communication
- Tested between two devices

## How to Run
1. Run aes_server.py
2. Run aes_client.py on another device
3. Enter a message
4. Server decrypts the message received from the client


## AES-Server Program:
```
import socket
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import unpad

# Generate AES key (16 bytes)
secret_key = get_random_bytes(16)
print("Generated AES Key:", secret_key)

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(("0.0.0.0", 12345))
server.listen(1)

print("Server waiting for connection...")
conn, addr = server.accept()
print("Connected to:", addr)

# Send AES key to client
conn.send(secret_key)

# Receive encrypted message
encrypted_msg = conn.recv(1024)

cipher = AES.new(secret_key, AES.MODE_ECB)
decrypted_msg = unpad(cipher.decrypt(encrypted_msg), 16)

print("Encrypted Message:", encrypted_msg)
print("Decrypted Message:", decrypted_msg.decode())

conn.close()
```
## AES-Client Program:

```
import socket
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(("<Server's IP Address>", 12345))

# Receive AES key from server
key = client.recv(1024)
print("Received AES Key:", key)

message = input("Enter message to send: ").encode()

cipher = AES.new(key, AES.MODE_ECB)
encrypted_msg = cipher.encrypt(pad(message, 16))

client.send(encrypted_msg)
print("Encrypted Message Sent:", encrypted_msg)

client.close()

```
## AES-Server:

<img width="544" height="93" alt="image" src="https://github.com/user-attachments/assets/2ce823e9-5ef9-4df1-baf3-02db42635d53" />

## AES-Client:

<img width="324" height="44" alt="image" src="https://github.com/user-attachments/assets/78bdabb1-87cc-40a1-9e8a-584dc59a0390" />

## Result:
The project successfully simulated secure client-server communication between two devices connected to the same network. Messages were encrypted using AES on the client side and decrypted on the server side using the shared key, confirming correct encryption and decryption.
