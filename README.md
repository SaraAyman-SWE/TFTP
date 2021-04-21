# The Trivial File Transfer Protocol

# Requirements

You’ll implement one of a TFTP client or TFTP server using the UDP sockets. (not both). Note that no project is harder than the other, feel free to pick randomly.
You can assume that there will be only one connection to the server (if you’re implementing the server, so you save the part about handling lots of threads).
The TFTP allows a client to upload or download files to a server. You can find more details in the RFC here (PDF), note that many pages in the RFC are introductory, so don’t panic if you find lots of text. Remember that the RFC MUST be accessible to anyone so that everyone understands everything the same way. That’s why it’s verbose. Reading the RFC is required. Little protocol explanation will be provided in this document.
The following answers some of the questions you might find when reading the RFC (read the RFC and follow these points in order when you have a question)
   * We’ll be transferring files. Not emails.
   * Use the octet mode of transfer. Not netascii or others.
   * The transfer identifiers (TID's) are simply the UDP port number, the TID notation is used across the RFC
   * The server runs on port 69 by default
   * 8-bit format means “byte”
If things aren’t still clear, feel free to watch this video (or any other video that doesn’t contain code ✌️)
Protol summary
The protocol operation occurs in two steps, which I will hint about here. 1) Connection establishment 2) Data transfer
> Connection establishment
Connections are initiated by clients by sending either a read request (RRQ) to download files from the server or sending a write request (WRQ) to upload files to the server. Those requests will be sent to the default server port which is 69.
After receiving a request, the server makes a new UDP socket for each client. Then checks if the request can be fulfilled and handles the required errors. Then using this new socket, the server will send the reply (ACK/ERR) to the client.
The positive reply for an RRQ is the first data block in the requested file (blk #1).
For a WRQ, the positive reply from the server in an ACK message with a block number 0, (blk #0).
Negative replies (if an error occurs), will be an ERR packet.
You're asked to handle only two error cases 
   1. File existence on server
   2. The client sending data to the main socket on port 69
Here are two examples of connection initiation
> Host  A  sends  a  "RRQ"  to  SERVER  with  source= A's PORT, destination= MAIN_SERVER_PORT.
< SERVER sends a "DATA" (with block number= 1) to host A with source= SERVER's NEW_SOCKET_PORT, destination= A's PORT.
>> Host A sends  a  "WRQ"  to  SERVER  with  source=  A's  PORT, destination= MAIN_SERVER_PORT.
<< SERVER  sends  a "ACK" (with block number= 0) to host A with source= SERVERS's NEW_SOCKET_PORT, destination= A's PORT.

# Data Transfer

Data transfer happens between two entities, one of them sends data and the other sends an ACK for each data packet sent. 
You're not required to handle timeouts or so.
________________
# Program arguments

Your program will be run from the command line interface. (i.e. I won't run anything in an IDE), this is not a problem for Python anyways, so don't worry.
If you're not familiar with the command line, checkout this article. 
PyCharm has options for adding command line arguments to programs also, google them.
Note don't take any input from the user using input(), this will make the graders fail. The code accepts only command line arguments.
> Client

The client program must accept some command line arguments which are [ push | pull ] file_name server_address server_port
For example, if your code file name is 3148_client_lab1.py.
For example, I want to "push" (upload) a file called "hello.txt" from the server at IP: "192.168.1.12" and port number 8777, We'll type something like this.
Note the IP address and port number of the server are optional arguments and the skeleton will substitute them for default values.
This is how you can run your code in the terminal.
python 4148_client_lab1.py push hello.txt 192.168.1.12 8777 # To upload a file
	Or to upload a file.
python 4148_client_lab1.py pull hello.txt 192.168.1.12 8777 # To download a file
	Note you'll mostly need to supply the operation and file name.
> Server

For the server, the only arguments are the IP address and the server port address and they're optional.
This is how you can run your code in the terminal.
python 4314_server_lab1.py 127.0.0.1 32048 # To run the server on port 32048

# Testing Code 

Run the tests

To run the tests python sample_tests.py
Build your code according to the RFC.

# Wireshark

Wireshark is the simplest method to make sure you're at least emitting TFTP packets, the packet type will appear as TFTP if your packets are correctly formatted. This is the first good sign you're on the right track.
TFTP Software
