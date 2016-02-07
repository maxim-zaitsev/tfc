<img align="right" src="https://cs.helsinki.fi/u/oottela/tfclogo.png" style="position: relative; top: 0; left: 0;">


###Tinfoil Chat NaCl


TFC-NaCl is a high assurance encryption plugin for Pidgin IM client that 
protects users from [passive eavesdropping](https://en.wikipedia.org/wiki/Upstream_collection), 
[active MITM attacks](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)
and remote CNE (aka equipment interference) of endpoints practiced by TLAs such 
as the [NSA](https://firstlook.org/theintercept/2014/03/12/nsa-plans-infect-millions-computers-malware/), 
 [GCHQ](http://www.wired.co.uk/news/archive/2014-03/13/nsa-turbine) and 
 [BKA](http://ccc.de/en/updates/2011/staatstrojaner).


###Overview

**Strong end-to-end encryption** with Curve25519 ECDHE / PSKs.
XSalsa20-Poly1305 AEAD with forward secrecy and deniability.


**Secure key generation** with /dev/urandom mixed with entropy from 
open circuit design GPIO HWRNG of RPi that acts either as TCB itself or as 
a sampling device over SSH.


**Endpoint security** obtained with hardware-based TCB separation:
Unidirectional data flow between end point's three computers prevents 
either injection of malware or exfiltration of keys and plaintexts from TCB
regardless of existing software zero-day vulnerabilities in software.


**Defeats metadata** about quantity and schedule of communication with 
trickle connection that outputs constant stream of encrypted noise data.


**Covert file transfer** that takes place in background during 
trickle connection.
 

**Multicasting** of messages and files to multiple recipients (no trickle 
supported).


###How it works

![](https://cs.helsinki.fi/u/oottela/tfc_graph2.png)

TFC uses three computers per endpoint. Alice enters her commands and messages to
program Tx.py running on her Transmitter computer (TxM), a [TCB](https://en.wikipedia.org/wiki/Trusted_computing_base)
separated from network. Tx.py encrypts and signs plaintext data and relays it
to receiver computers (RxM) via networked computer (NH) through RS-232 interface 
and a data diode.

Depending on packet type, the program NH.py running on Alice's NH forwards 
packets from TxM-side serial interface to Pidgin and local RxM (through another 
RS-232 interface and data diode). Local RxM authenticates and decrypts received
data before processing it.

Pidgin sends the packet either directly or through Tor network to IM server, 
that then forwards it directly (or again through Tor) to Bob.

NH.py on Bob's NH receives Alice's packet from Pidgin, and forwards it through 
RS-232 interface and data diode to Bob's RxM, where the ciphertext is 
authenticated, decrypted, and processed. When the Bob responds, he will send
the message/file using his TxM and in the end Alice reads the message from her RxM.


###Why keys can not be exfiltrated

1. Malware that exploits an unknown vulnerability in RxM can infiltrate to 
the system, but is unable to exfiltrate keys or plaintexts, as data diode prevents
all outbound traffic.

2. Malware can not breach TxM as data diode prevents all inbound traffic. The
only data input from RxM to TxM is the 72 char public key, manually typed by 
user.

3. The NH is assumed to be compromised, but unencrypted data never touches it.

![](https://cs.helsinki.fi/u/oottela/tfc_attacks2.png)

Optical repeater inside the optocoupler of the data diode (below) enforces direction of data transmission.

<img src="https://cs.helsinki.fi/u/oottela/data_diode.png" align="center" width="74%" height="74%"/>


###Installation
[![Installation](http://img.youtube.com/vi/D5pDoJZj2Uw/0.jpg)](http://www.youtube.com/watch?v=D5pDoJZj2Uw)


###How to use
[![Use](http://img.youtube.com/vi/tH8qbl1USoo/0.jpg)](http://www.youtube.com/watch?v=tH8qbl1USoo)


###More information

White paper and manual for previous versions are listed below. TFC-NaCl specific
updates are listed in the updatelog. Updated white paper and documentation are
under work.

White paper: https://cs.helsinki.fi/u/oottela/tfc.pdf

Manual: https://cs.helsinki.fi/u/oottela/tfc-manual.pdf
