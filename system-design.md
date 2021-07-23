* [OSI 7 layer model](#osi-7-layer-model)

### OSI 7 layer model
(Open Systems Interconnection)

1. Physical Layer (Layer 1):
The lowest layer of the OSI reference model is the physical layer. It is responsible for the actual physical connection between the devices. The physical layer contains information in the form of bits. It is responsible for transmitting individual bits from one node to the next. When receiving data, this layer will get the signal received and convert it into 0s and 1s and send them to the Data Link layer, which will put the frame back together.
Hub, Repeater, Modem, Cables are Physical Layer devices
2. Data Link Layer (DLL) (Layer 2):
The data link layer is responsible for the node to node delivery of the message. The main function of this layer is to make sure data transfer is error-free from one node to another, over the physical layer. When a packet arrives in a network, it is the responsibility of DLL to transmit it to the Host using its MAC address.
Switch & Bridge are Data Link Layer devices.
3. Network Layer (Layer 3):
Network layer works for the transmission of data from one host to the other located in different networks. It also takes care of packet routing i.e. selection of the shortest path to transmit the packet, from the number of routes available. The sender & receiver’s IP address are placed in the header by the network layer.
The functions of the Network layer are : Routing & Logical Addressing
4. Transport Layer:
Provides services to application layer and takes services from network layer. It is responsible for the End to End Delivery of the complete message. The transport layer also provides the acknowledgement of the successful data transmission and re-transmits the data if an error is found. Forward/read port number.
Transport layer is operated by the Operating System. It is a part of the OS and communicates with the Application Layer by making system calls.
5. Session Layer (Layer 5):
is responsible for establishment of connection, maintenance of sessions, authentication and also ensures security.
6. Presentation Layer (Layer 6):
aka the Translation layer. The data from the application layer is extracted here and manipulated as per the required format to transmit over the network.
The functions of the presentation layer are :
Translation : For example, ASCII to EBCDIC, Encryption/ Decryption, Compression
7. Application Layer (Layer 7)

<img src="images/computer-network-osi-model-layers.png" alt="OSI model" width="70%"/>  

<img src="images/osi-model.jpeg" alt="OSI model"/>