---
layout: post
title:  "Multicast UDP Sockets with examples"
date:   2020-04-13 22:29:06 -0400
categories: jekyll update
---
## Introduction
-------
Using multicast, we can send datagrams to a GROUP of interested listeners. The members may join or opt in to receive the multicast packets sent within the group. Members of this group may or may not know the other members of the multicast group. Lets look at a sample program of a sender and receiver where the sender does the multicast.

### The sender - send.c
-------
The send program is the simpler of the two. The only difference compared to a traditional UDP sockets program is that the `to address` will now be the multicast group. The following program sends a message string to the multicast address `239.0.0.1`. The message is transmitted every two seconds for a total of 10 times.

```cpp
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <errno.h>

#define PORT 1820
// The range 239.0.0.0 to 239.255.255.255 can be used locally for multicast
#define GROUP "239.0.0.1"

int main(int argc, char* argv[])
{
    int sd, rc, i;
    struct sockaddr_in mc_addr;
    socklen_t addr_len;
    unsigned char message[12] = "Hello World";

    // Set up the socket
    sd = socket(AF_INET, SOCK_DGRAM, 0);
    if (sd < 0) {
        perror("Error while setting up socket");
        exit(1);
    }

    // Set up the multicast address
    mc_addr.sin_family = AF_INET;
    mc_addr.sin_addr.s_addr = inet_addr(GROUP);
    mc_addr.sin_port = htons(PORT);
    addr_len = sizeof(mc_addr);

    // Send the message every 5 seconds for 5 times
    for ( i = 0; i < 10; i++) {
        printf("Sending Message.\n");
        rc = sendto(sd, message, sizeof(message), 0,
                (struct sockaddr *) &mc_addr, addr_len);
        if (rc < 0) {
            perror("Error while sending");
            exit(1);
        }
        sleep(2);
    }
}
```

&nbsp;

### The receiver - recv.c
-------
The difference compared to the traditional UDP sockets program is that we have to add socket options so that the socket is added to the multicast group. This is done using `setsockopt`. We need to tell the kernel which multicast groups we are interested in. If no process is interested in a group, packets destined to it that arrive to the host are discarded. In order to inform the kernel of our interests and, thus, become a member of that group, you should first fill an ip_mreq structure which is passed later to the kernel in the optval field of the setsockopt() system call. 

In the following program, the socket is a member of the multicast group `239.0.0.1`. It recevies any message that was sent to the group on an infinite loop.

```cpp
#include <stdio.h>
#include <string.h>
#include <strings.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/ioctl.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <netdb.h>

#define PORT 1820
#define GROUP "239.0.0.1"

int main() {
    int sd, rc;
    struct sockaddr_in mc_addr;
    struct ip_mreq mc_req;
    char message[12];
    socklen_t mc_len;

    // Set up the socket
    sd = socket(AF_INET, SOCK_DGRAM, 0);
    if (sd < 0) {
        perror("Error while setting up socket");
        exit(1);
    }

    // Set up the server address
    bzero(&mc_addr, sizeof(struct sockaddr_in));
    mc_addr.sin_family = AF_INET;
    mc_addr.sin_port = htons(PORT);
    mc_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    mc_len = sizeof(mc_addr);

    // Bind to an ephemeral port
    if (bind(sd, (struct sockaddr *) &mc_addr, mc_len) < 0) {
        perror("Error while binding socket");
        exit(1);
    }

    // Add multicast membership to socket options
    bzero(&mc_req, sizeof(struct ip_mreq));
    mc_req.imr_multiaddr.s_addr = inet_addr(GROUP);
    mc_req.imr_interface.s_addr = htonl(INADDR_ANY);
    if (setsockopt(sd, IPPROTO_IP, IP_ADD_MEMBERSHIP,
                &mc_req, sizeof(struct ip_mreq)) < 0) {
        perror("Error while adding membership");
        exit(1);
    }

    // Receive the messages
    while (1) {
        rc = recvfrom(sd, message, sizeof(message), 0,
                (struct sockaddr *) &mc_addr, &mc_len);
        if (rc < 0) {
            perror("Error while receiving");
            exit(1);
        }

        printf("Received message: %s\n", message);
    }
}
```
&nbsp;
