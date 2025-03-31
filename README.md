# IoTKeyLogger With RSA Encryption

## Overview: 
IoTKeyLogger is an educational proof-of-concept project that demonstrates how to build a keylogger using an IoT device (Raspberry Pi) as a server and a client running on any PC. The project captures keystrokes on the client, encrypts them using RSA encryption, and transmits the encrypted data to the Raspberry Pi server via the MQTT protocol. The server decrypts the data and logs the keystrokes to a file. This project highlights the potential vulnerabilities in physical and network security while showcasing secure data transmission using public-key cryptography.

###### Note: This project is for educational purposes only. Unauthorized keylogging is illegal and unethical. Use this tool responsibly and only with explicit permission from the device owner.

### Project Structure
- **client.py**: Runs on the client PC, captures keystrokes using the pynput library, encrypts them with a public key, and publishes the encrypted data to the Raspberry Pi server via MQTT.
- **server.py:** Runs on the Raspberry Pi, subscribes to the MQTT topic, receives encrypted keystrokes, decrypts them using a private key, and logs them to keylogs.txt.
- **key_generation.py:** Generates a public-private key pair (public_key.pem and private_key.pem) for RSA encryption and decryption.
- **keylogs.txt:** Stores the decrypted keystrokes captured by the server.
- **public_key.pem:** The public key used by the client for encryption.
- **private_key.pem:** The private key used by the server for decryption.

### How It Works:
- **Key Generation:** The key_generation.py script generates a public-private key pair for RSA encryption.
- **Client (PC)**: The client.py script runs on the client PC, captures keystrokes using the pynput library, encrypts them with the public key, and publishes the encrypted data to an MQTT topic (keylogger/data).
- **Server (Raspberry Pi): **The server.py script runs on the Raspberry Pi, subscribes to the MQTT topic, receives the encrypted data, decrypts it using the private key, and logs the keystrokes to keylogs.txt.
- **Data Transmission:** The MQTT protocol is used for communication between the client and server, ensuring lightweight and efficient data transfer.
- **Optional AWS IoT Integration:** The client code includes commented-out sections for publishing data to AWS IoT Core, which can be enabled for cloud-based logging (requires AWS IoT certificates and configuration).

### Hardware Setup
- **Raspberry Pi:** Acts as the MQTT server. It requires:
- **A network connection **(Wi-Fi or Ethernet) to communicate with the client.
- **Client PC: **Any computer (Windows, macOS, or Linux) where the client.py script runs to capture keystrokes.
### Software Requirements
##### Raspberry Pi:
- Raspbian OS (or compatible OS).
- Python 3.x.
- MQTT broker (e.g., Mosquitto) installed on the Raspberry Pi:
```
sudo apt update
sudo apt install mosquitto mosquitto-clients
sudo systemctl enable mosquitto
```
##### Client PC:
Python 3.x.
Required Python libraries:
	- paho-mqtt: For MQTT communication.
	- pynput: For capturing keystrokes.
	- cryptography: For RSA encryption.
```
pip install paho-mqtt pynput cryptography
```

### Installation and Setup
**Clone the Repository:**

**Set Up the Raspberry Pi**:
	- Ensure the Raspberry Pi is connected to the same network as the client PC.
	- Install and start the Mosquitto MQTT broker on the Raspberry Pi (see Software 		Requirements).
	- Note the Raspberry Pi's IP address (e.g., 192.168.1.4).
**Generate RSA Keys:**
	Run the key_generation.py script to create the public-private key pair:
```
python key_generation.py
```
**Configure the Client:**
- 	Copy public_key.pem to the client PC.
- 	Update the LOCAL_BROKER_IP in client.py with the Raspberry Pi's IP address:

```
LOCAL_BROKER_IP = "192.168.1.4"  # Replace with your Raspberry Pi's IP
```

**Run the Server (Raspberry Pi):**
- 	Copy server.py, private_key.pem, and ensure keylogs.txt is writable on the Raspberry Pi.
- 	Start the server:
```
python server.py
```

**Run the Client (PC):**
- On the client PC, run the client script:
```
python client.py
```

**Monitor Logs:**
- On the Raspberry Pi, check keylogs.txt for the decrypted keystrokes.

### Security Features
- RSA Encryption: Keystrokes are encrypted using RSA with OAEP padding, ensuring secure transmission over the network.
- MQTT Protocol: Lightweight and efficient for IoT communication, with the option to add TLS for additional security (e.g., with AWS IoT Core).

### Disclaimer
This project is for educational purposes only. Do not use it to harm systems or networks without explicit permission. Unauthorized keylogging is illegal and unethical. The author is not responsible for any misuse of this tool.


