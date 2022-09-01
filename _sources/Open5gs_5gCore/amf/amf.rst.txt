Access and Mobility Management Function `[0] <https://github.com/open5gs/open5gs/tree/main/src/amf>`_
*********

.. image:: photos/amf.jpeg
 :align: center
 :alt: Alternative text

AMF(Access and Mobility Management Function) is a Control Plane(CU) function in the 5G Core Network. 5G AMF is the evolution of 4G MME. It supports the Termination of NAS signaling, NAS ciphering & integrity protection, registration management, connection management, mobility management, access authentication and authorization, and security context management.  gNodeB first needs to connect with AMF to access any 5G services. 

Interface
=======

N1 
-------

N1 is the interface between UE and AMF. AMF retrieves all the connection and session-related information from the UE over the N1 interface.

N2 
-------

UE-associated and non-UE-associated communication between AMF and gNodeB take place over this interface. 

N8 
-------

N8 is the interface between AMF and UDM. When AMF needs subscriber data like session-related subscription data, policy rules for users and particular UEs, and any other information, at that time this interface is used to access data from UDM to AMF. 

N11 
-------

Messages received over the N11 interface represent a trigger to add, modify or delete a PDU session across the user plane.

N12 
-------

N12 emulates AUSF within the 5G Core offering services to the AMF  via the AUSF service-based N12 interfaces. The 5G network represents the service-based interface, with a focus on the AUSF and AMF.

N22 
-------

Interface between AMF and NSSF. AMF selects the best Network Functions (NF) across the network with the help of NSSF. NSSF provides the network functions location to the AMF over the N22 interface.


SBI 
-------

Service-Based Interface is the API-based communication between network functions.

Protocols
=======

NAS `[9] <https://github.com/open5gs/open5gs/blob/main/src/amf/nas-path.c>`_
-------

The 5G NAS(Non-Access Stratum) is a control plane protocol that is present at the radio interface(N1 interface) between UE and AMF. This manages the mobility and session-related context within 5GS(5G System).

NGAP `[12] <https://github.com/open5gs/open5gs/blob/main/src/amf/ngap-build.c>`_ `[13] <https://github.com/open5gs/open5gs/blob/main/src/amf/ngap-handler.c>`_
-------

The Next-Generation Application Protocol(NGAP) is a Control Plane(CP) protocol signaling between gNB and the AMF. It handles the UE-associated and non-UE-associated services.

SCTP `[15] <https://github.com/open5gs/open5gs/blob/main/src/amf/ngap-sctp.c>`_
-------

Stream Control Transmission Protocol(SCTP) ensures the transport of signaling messages among AMF and 5G-AN nodes (N2 interface). 



Call Flow
=======

Registration and Deregistration
-------



.. image:: photos/registration.jpeg
  :alt: Alternative text

- UE and gNB registration request to AMF
- AMF selection
- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ Registration reject response.
- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ Initialize registration request.
- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ The AMF shall assign a new 5G-GUTI for particular UE

- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ AMF new GUTI assign: guami + tmsi
- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ AMF UE confirms GUTI 
- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ If the new GUTI is valid, then we need to remove previous GUTI in hash table.
- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ Finding AMF-UE: 1. By teid 2. By SUPI 3. By SUCI 4. By message 5. By GUTI

- `[8] <https://github.com/open5gs/open5gs/blob/main/src/amf/namf-handler.c>`_ NAMF communication UE Context transfer

- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ Authentication and security
- `[8] <https://github.com/open5gs/open5gs/blob/main/src/amf/namf-handler.c>`_ UE context transfer complete

- `[23] <https://github.com/open5gs/open5gs/blob/main/src/amf/nudm-build.c>`_ Nudm uecm context transfer

- `[7] <https://github.com/open5gs/open5gs/blob/main/src/amf/init.c>`_ AMF context parse config 
- `[7] <https://github.com/open5gs/open5gs/blob/main/src/amf/init.c>`_ AMF tmsi pool generate
- `[7] <https://github.com/open5gs/open5gs/blob/main/src/amf/init.c>`_ NGAP open 
- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ Deregistration request
- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ Delete all the AMF-session context in the AMF-UE Context

- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ SM deregistration request 
- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ Free Sbi object memory

- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ UE context release

- `[23] <https://github.com/open5gs/open5gs/blob/main/src/amf/nudm-build.c>`_ AMF Nudm uecm deregistration
- `[8] <https://github.com/open5gs/open5gs/blob/main/src/amf/namf-handler.c>`_ AMF release all sm context
- `[7] <https://github.com/open5gs/open5gs/blob/main/src/amf/init.c>`_ AMF event terminated
- `[7] <https://github.com/open5gs/open5gs/blob/main/src/amf/init.c>`_ AMF sbi close

- `[2] <https://github.com/open5gs/open5gs/blob/main/src/amf/context.c>`_ Delete gNB context


Mobility Management
-------

.. image:: photos/mobility.jpeg
  :alt: Alternative text

- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_ Handle registration request 
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_ UE send initial NAS message to AMF
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_  Set 5gs registration type:KSI , TSC
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_ AMF new GUTI
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_  Copy Stream number , NR-TAI, NR-CGI from ran_ue
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_  Check TAI
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_  AMF selected algorithm should be equal to NAS security algorithm
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_   5GMM Request accepted
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_  5GMM handle registration update
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_  5GMM handle service request
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_  Initial NAS service request message should contain security header type,ngKSI,TMSI,security header type
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_  5GMM handle service update
- `[17] <https://github.com/open5gs/open5gs/blob/main/src/amf/nnrf-handler.c>`_ NRF discovers AUSF
 
- `[25] <https://github.com/open5gs/open5gs/blob/main/src/amf/sbi-path.c>`_ Initialize SCP NF instance
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_ `[6] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-sm.c>`_ AMF nausf authentication response and then confirm 
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_ Identity response SUCI
 
- `[6] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-sm.c>`_ 5GMM state registered
 
- `[13] <https://github.com/open5gs/open5gs/blob/main/src/amf/ngap-handler.c>`_ NGAP handle path switch request
- `[13] <https://github.com/open5gs/open5gs/blob/main/src/amf/ngap-handler.c>`_ NGAP handle handover required 
- `[13] <https://github.com/open5gs/open5gs/blob/main/src/amf/ngap-handler.c>`_ NGAP handle handover notification
- `[13] <https://github.com/open5gs/open5gs/blob/main/src/amf/ngap-handler.c>`_ NGAP handle ran configuration update
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_ `[6] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-sm.c>`_ 5GMM handle UL nas transport
 
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_ 5GMM handle deregistration request 
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_ Set 5gs deregistration type
 
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_ AMF sbi release all sessions 
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_ Clear paging info
 
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_ Clear SM context
- `[5] <https://github.com/open5gs/open5gs/blob/main/src/amf/gmm-handler.c>`_ Deassociate NG with NAS

 
Authentication
-------


.. image:: photos/authentication.jpeg
  :alt: Alternative text

- `[11] <https://github.com/open5gs/open5gs/blob/main/src/amf/nausf-handler.c>`_ UE Authentication request
 
- `[11] <https://github.com/open5gs/open5gs/blob/main/src/amf/nausf-handler.c>`_ UE response
- `[17] <https://github.com/open5gs/open5gs/blob/main/src/amf/nnrf-handler.c>`_ NRF discovers AUSF
 
- `[25] <https://github.com/open5gs/open5gs/blob/main/src/amf/sbi-path.c>`_ Initialize SCP NF instance
- `[11] <https://github.com/open5gs/open5gs/blob/main/src/amf/nausf-handler.c>`_ NAMF Nausf authentication request
- `[11] <https://github.com/open5gs/open5gs/blob/main/src/amf/nausf-handler.c>`_ 5gAKA
- `[11] <https://github.com/open5gs/open5gs/blob/main/src/amf/nausf-handler.c>`_ Av5gAka contains authentication vector for 5gAKA method 
- `[11] <https://github.com/open5gs/open5gs/blob/main/src/amf/nausf-handler.c>`_ Amf_ue -> SUCI 
- `[11] <https://github.com/open5gs/open5gs/blob/main/src/amf/nausf-handler.c>`_ Confirmation URL for 5g AKA 
 
- `[11] <https://github.com/open5gs/open5gs/blob/main/src/amf/nausf-handler.c>`_ SEAF starts authentication procedure
- `[11] <https://github.com/open5gs/open5gs/blob/main/src/amf/nausf-handler.c>`_ SUPI and Kseaf
- `[11] <https://github.com/open5gs/open5gs/blob/main/src/amf/nausf-handler.c>`_ Authentication confirmed
- `[11] <https://github.com/open5gs/open5gs/blob/main/src/amf/nausf-handler.c>`_ OR Authentication failed
 
 
Context Setup and Session Modification
-------
  

.. image:: photos/PDU.jpeg
  :alt: Alternative text

- `[22] <https://github.com/open5gs/open5gs/blob/main/src/amf/nsmf-handler.c>`_ PDU session establishment request
- `[22] <https://github.com/open5gs/open5gs/blob/main/src/amf/nsmf-handler.c>`_ Nsmf create SM context 
- `[22] <https://github.com/open5gs/open5gs/blob/main/src/amf/nsmf-handler.c>`_ SBI HTTP status created  
- `[24] <https://github.com/open5gs/open5gs/blob/main/src/amf/nudm-handler.c>`_ Nudm sdm get SDM 
 
- `[24] <https://github.com/open5gs/open5gs/blob/main/src/amf/nudm-handler.c>`_ Nudm SDM handle provisioned
 
- `[19] <https://github.com/open5gs/open5gs/blob/main/src/amf/nnssf-handler.c>`_ NS selection for pdu session
 
- `[22] <https://github.com/open5gs/open5gs/blob/main/src/amf/nsmf-handler.c>`_ AMF nsmf update sm context
- `[20] <https://github.com/open5gs/open5gs/blob/main/src/amf/npcf-handler.c>`_ Npcf AM policy create
- `[22] <https://github.com/open5gs/open5gs/blob/main/src/amf/nsmf-handler.c>`_ Sm context response
 
- `[22] <https://github.com/open5gs/open5gs/blob/main/src/amf/nsmf-handler.c>`_ N1N2 message transfer
- `[22] <https://github.com/open5gs/open5gs/blob/main/src/amf/nsmf-handler.c>`_ Nsmf context updated

 
AMF Timer
------- 



.. image:: photos/timer.jpeg
  :alt: Alternative text

 
