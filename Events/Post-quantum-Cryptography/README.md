# Post-quantum Crypto (PQC) and F5

Quantum computing is one of today's most exciting technological advancements, poised to transform industries with its unparalleled processing power. Unlike classical computers, which encode data using binary bits (zeroes and ones), quantum computers leverage quantum bits, or qubits. Qubits can exist in a superposition of zero and one, meaning they can be a zero, a one, or a combination of both at the same time. This capability enables quantum computers to solve certain types of problems exponentially faster than today's classical computers.

While quantum computing won't necessarily benefit everyday tasks like web surfing, its true strength lies in addressing complex, data-intensive challenges, such as accelerating medical and pharmaceutical research, optimizing AI algorithms, improving network designs, and advancing cryptography.

F5 Post-quantum cryptography readiness: https://www.f5.com/pdf/solution-overview/f5-pqc-readiness.pdf

PQC key benefits:

Cryptography is embedded into the core of the F5® Application Delivery and Security Platform (ADSP) via the F5 BIG-IP® Traffic Management Microkernel (TMM), a foundational
element of BIG-IP solutions. 

NGINX provides PQC support using the Open Quantum Safe provider library for OpenSSL 3.x (oqs-provider). This library is available from the Open Quantum Safe (OQS) project. The oqs-provider library adds support for all post-quantum algorithms supported by the OQS project into network protocols like TLS in OpenSSL-3 reliant applications. All ciphers/algorithms provided by oqs-provider are supported by NGINX. 

- Centralizes management of cryptographic handshakes and encryption processes
- Enables crypto agility for easier adaptation to emerging cryptographic standards and strategies
- Supports quantum-resistant ciphers for both clients and servers, providing hybrid key encapsulation for robust PQC protection

Because of its pivotal role in network and application architectures—managing traffic between clients and servers—F5 ADSP is strategically positioned to facilitate and ensure PQC readiness while adapting seamlessly as encryption standards evolve.

## Lab Environment

This lab environment contains three parts:

- BIG-IP 17.5
- NGINX Open Source Server
- Windows-client

![lab](images/image00.png)

### Windows-client

From the Windows-client, we will be able to access the BIG-IP TMUI and the websites protected with PQC profiles/OpenSSL behind both BIG-IP and NGINX.

1. Navigate to the details button of the Windows-client and connect with RDP.

> Note: To run the RDP session in "windowed" mode, choose a screen size from the drop-down list

![rdp](images/image01.png)

2. Credentials for the Windows-client are also located on the details page

![password](images/image02.png)

3. Accept the RDP certificate error if prompted

![rdp-cert](images/image03.png)

4. Open the Chrome browser and skip the sign-in process

![chrome-login](images/image05.png)

5. Set Chrome as the default browser

![chrome-default](images/image06.png)

6. Set the system-level default to Chrome and close the system properties

![chrome-system](images/image07.png)

> Note: Kyber level PQC was an early access feature in Chrome; this has been removed from the current release of Chrome. We will be using an older version to demonstrate our capabilities; please refrain from updating the Chrome browser.

### BIG-IP Setup

BIG-IP has 17.5 installed. In 17.1, PQC for client-side SSL profiles was introduced, which we will explore in this lab. In 17.5.1, PQC for server-side SSL profiles was added, and both client and server-side ciphers were updated to the NIST standards at the time of publication.

BIG-IP supports both Kyber and ML-KEM, in this lab we will be showing Kyber, though ML-KEM is more widely adopted.

> Note: We will not be doing server-side SSL PQC setup; however, the environment supports it, for exploration.

Understanding PQC Standards and Timelines: ```https://www.f5.com/company/blog/understanding-pqc-standards-and-timelines```

1. Log in to the BIG-IP to verify access and configuration

From the Chrome browser, open the BIG-IP TMUI: ```https://10.1.1.6```

![tmui-warning](images/image08.png)
![tmui-accept](images/image09.png)

User: admin
Password admin

![tmui-login](images/image10.png)

2. Post-quantum crypto configuration

BIG-IP utilizes SSL Profiles for client and server-side TLS negotiations. Within the SSL Profile, attached cipher groups manage the cipher rules for negotiation.

Configuration has already been created on behalf of the lab. If you would like to configure and familiarize yourself with a new SSL profile, here is the public documentation for 17.5 and 17.5.1: ```https://my.f5.com/manage/s/article/K000149577```

3. Navigate to BIG-IP cipher rules

![cipher-rules](images/image11.png)

4. Like the example documentation, there are two created PQC profiles, one made from the TMUI and the other from TMSH

![pqc-tmsh](images/image12.png)

5. Explore the TMSH_PQC rule, and verify the setup

![pqc-tmsh-rule](images/image13.png)

6. Navigate to BIG-IP cipher groups

![cipher-groups](images/image14.png)

7. Explore the TMSH_PQC group, and verify the setup

![pqc-tmsh-group](images/image15.png)

8. Navigate to SSL Client profiles

![client-profiles](images/image16.png)

9. Explore the TMSH_PQC client SSL profile, and verify the setup

![pqc-client-ssl](images/image17.png)

![pqc-client-ssl-settings](images/image18.png)

10. Navigate to the BIG-IP virtual servers

![virtual-servers](images/image19.png)

11. Explore the pqc_vs virtual server, and verify the setup

![pqc-virtual-server](images/image20.png)

### BIG-IP Chrome PQC settings

The Chrome browser has experimental features that enable Kyber and ML-KEM. However, as mentioned earlier, these features have been removed from the current version of Chrome due to a security gap.

Enable the security features in Chrome to use the Kyber settings and disable the ML-KEM settings

1. Open the Chrome browser and browse to chrome://flags/

2. Change the experimental settings to enable "TLS 1.3 post-quantum key agreement", and disable "Use ML-KEM in TLS 1.3", and relaunch the browser

![pqc-big-ip-browser-settings](images/image21.png)

### BIG-IP PQC Virtual Server Validation

With Chrome, check the version of TLS negotiation and the ciphers used.

1. Open Chrome and browse to ```https://10.1.10.100``` the virtual server address on the BIG-IP with the PQC SSL Client profile attached

![ssl-error](images/image22.png)

2. Proceed to the website

![ssl-proceed](images/image23.png)

3. The loaded page is the NGINX default page

![nginx-homepage](images/image24.png)

4. Open the Chrome browser developer tools 

![developer-tools](images/image25.png)

5. Scroll the developer tools to the left, exposing Privacy and security to show the TLS negotiation

![tls-kyber](images/image26.png)

### NGINX Setup

NGINX utilizes the OpenSSL package installed on the host operating system for the SSL library. The OpenSSL 3.5 package, SSL certificate, SSL Key, and NGINX configuration are already complete. Review the settings for understanding.

1. From the UDF lab page, use the access Web Shell

![webshell-access](images/image28.png)

2. View the NGINX configuration file from the prompt with: 

```cat /opt/nginx/nginx.conf```

![nginx-conf](images/image29.png)

3. As a best practice in the nginx.The conf file includes another location for the PQC listener. From the prompt, view the included PQC listener: 

```cat /opt/nginx/conf.d/pqc.conf```

![nginx-pqc-conf](images/image30.png)

In this configuration, we can see the listening port, the certificate and key used, and the SSL options for exchange, ML-KEM.

### NGINX Chrome PQC settings

The Chrome browser has experimental features that enable Kyber and ML-KEM. However, as mentioned earlier, these features have been removed from the current version of Chrome due to a security gap.

Enable the security features in Chrome to use the Kyber settings, but prefer ML-KEM settings

1. Open the Chrome browser and browse to ```chrome://flags/```

2. Change the experimental settings to enable "TLS 1.3 post-quantum key agreement", and enable "Use ML-KEM in TLS 1.3", and relaunch the browser

![pqc-nginx-browser-settings](images/image27.png)

### NGINX PQC Listener Validation

With Chrome, check the version of TLS negotiation and the ciphers used.

1. Open Chrome and browse to ```https://10.1.10.30``` the listener address on NGINX with the PQC SSL options

![nginx-ssl-error](images/image32.png)

2. Proceed to the website

![nginx-ssl-proceed](images/image31.png)

3. NGINX is hosting a page with the negotiated SSL curve version listed

![nginx-cipher](images/image33.png)

4. Open the Chrome browser developer tools 

![developer-tools](images/image34.png)

5. Scroll the developer tools to the left, exposing Privacy and security to show the TLS negotiation

![tls-kyber](images/image35.png)

## Lab Complete