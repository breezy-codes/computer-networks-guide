---
layout: post
title: "Building a Simple Server-Client Database in Python"
subtitle: "Developed for SIT202 - How to build a simple python script to simulate a server client process"
background: '/img/banner.png'
tags: tutorials
---

In the world of programming, networking plays a crucial role in enabling communication between different devices. In this article, we'll walk through the process of building a basic server-client Python program that demonstrates the concept of socket programming. We'll create an animal database where the client can request animal names, and the server responds with information about those animals.

You can view the code for this here:
![Readme Card](https://github-readme-stats.vercel.app/api/pin/?username=breezy-codes&repo=Server-Client-Program&show_owner=true&)

### **Prerequisites:**
- Basic understanding of Python programming.
- Familiarity with sockets and fundamental networking concepts.
### **Getting Started: Server Side**
1. **Importing Modules:** Begin by importing the `socket` module, which provides the necessary functions for socket communication.
2. **Setting Up Server:** Define the server's IP address and port number where it will listen for incoming requests.
3. **Creating a Socket:** Utilise the `socket.socket(socket.AF_INET, socket.SOCK_DGRAM)` function to create a socket for communication. `AF_INET` indicates IPv4, and `SOCK_DGRAM` signifies the use of UDP sockets.
4. **Binding Socket:** Bind the socket to the previously defined IP and port using `server_socket.bind((server_ip, server_port))`.
5. **Handling Requests:** Implement a loop to continuously handle incoming requests:
    - Receive data and client address using `data, client_address = server_socket.recvfrom(1024)`.
    - Process the received animal name by converting it to lowercase and removing whitespace.    
    - Check if the animal name exists in the `our_animals` dictionary. If found, prepare a response by joining the animal names and send it back to the client. If not found, send an appropriate error message.
6. **Closing Socket**: Close the server socket using `server_socket.close()`.
### **Client Side:**
1. Importing Modules: As with the server, start by importing the `socket` module.
2. Setting Up Client: Define the server's IP address and port number.
3. Creating a Socket: Create a socket for communication using `socket.socket(socket.AF_INET, socket.SOCK_DGRAM)`.
4. Client Interaction Loop: Create a `while True` loop to keep the client running until the user decides to exit.
5. Sending Requests: Inside the loop.
    - Prompt the user to enter an animal name using `input()`.    
    - Send the entered animal name to the server using `client_socket.sendto(animal.encode(), (server_ip, server_port))`.    
6. Receiving Responses: Receive the response from the server using `response, _ = client_socket.recvfrom(1024)` and print the animal names.
7. Continuation Prompt: Ask the user if they want to continue. If input is not 'y', exit the loop.  
8. Closing Socket: Close the client socket using `client_socket.close()`.
### **Usage:**
1. Run the `server.py` file to start the animal database server.
2. Run the `client.py` file to initiate the client.
3. Enter an animal type (e.g., "lion") in the client prompt.
4. The server responds with names and ages of animals of that type.
5. Decide whether to continue querying or exit.
### **Conclusion:**
Socket programming is a fundamental skill in networking, enabling communication between devices over networks. In this article, we've demonstrated the basics of creating a server-client program in Python, building a simple animal database as an example. This serves as a foundation for more advanced network applications. Keep in mind real-world considerations like error handling, security, and scalability as you delve into more complex networking projects.

<div style="text-align: center;">  
  <div style="position: relative; height: 315px; width: 560px; margin: 0 auto;">  
    <iframe src="https://www.youtube.com/embed/S4oAhXF7hcM?si=9QtJxVhkxyW5XRUe" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>  
  </div>  
</div>

