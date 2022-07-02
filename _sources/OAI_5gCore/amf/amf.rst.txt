Access and Mobility Management Function `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf>`_
*********

.. image:: photos/OAI_amf.png
 :align: center
 :alt: Alternative text

AMF(Access and Mobility Management Function) is a Control Plane(CU) function in the 5G Core Network(CN). gNodeB first needs to connect with AMF to access any 5G services. AMF is the only Network Function(NF) through which gNB communicates with the 5G Core(excluding interaction with the UPF(User Plane Function) during the PDU Session Establishment). 

Interface
=======

N1 `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_
-------

AMF retrieves all the connection and session-related information from the UE over the N1 interface.

N2 `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_
-------

UE-associated and non-UE-associated communication between AMF and gNodeB take place over this interface. 

N8 
-------

Policy rules both for all users and for particular UEs, session-related subscription data, subscriber data, and any other information(e.g. data exposed to the third-party application) is stored in UDM which AMF retrieves over the N8 interface.

N11 `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_
-------

The N11 interface represents a trigger to add, modify or delete a PDU session by AMF across the User Plane. 

N12 
-------

N12 emulates AUSF within the 5G Core offering services to the AMF  via the AUSF service-based N12 interfaces. The 5G network represents the service-based interface, with a focus on the AUSF and AMF.

N22 
-------

AMF selects the best Network Functions (NF) across the network with the help of NSSF. NSSF provides the network functions location to the AMF over the N22 interface.

SBI `[8] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/tree/master/src/sbi>`_
-------

Service-Based Interface is the API-based communication between network functions.

Protocols
=======

NAS `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/tree/master/src/nas>`_
-------

The 5G NAS(Non-Access Stratum) is a control plane protocol that is present at the radio interface(N1 interface) between UE and AMF. This manages the mobility and session-related context within 5GS(5G System).

NGAP `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/tree/master/src/ngap>`_
-------

The Next-Generation Application Protocol(NGAP) is a Control Plane(CP) protocol signaling between gNB and the AMF. It handles the UE-associated and non-UE-associated services.

SCTP `[7] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/tree/master/src/sctp>`_
-------

Stream Control Transmission Protocol(SCTP) ensures the transport of signaling messages among AMF and 5G-AN nodes (N2 interface). 

ITTI Message `[9] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/tree/master/src/itti>`_
-------

Inter Task Interface is used for sending messages between tasks.

Call Flow
=======

Registration and Deregistration
-------

* AMF first needs to register with the NRF to identify the Network Function locations and communicate with them. When the UE powers ON, it undergoes a registration process. AMF handles the registration procedure, then accepts the initial NAS UE message and the registration request. This message is needed to create an AMF identity for the UE. 

* Then, it checks for the last AMF with which the UE is registered. And if it succeeds to find the old AMF address, the new AMF retrieves all the UE context, and a deregistration procedure is initiated against the old AMF. The old AMF requests for the release of SM context from the SMF and also requests for the release of UE context from the gNB.

.. image:: photos/OAI_AMF2.png
  :alt: Alternative text

- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ Process data to obtain NRF info
- `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_app.cpp>`_ Registering with NRF 
- `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_app.cpp>`_ Handle an NF status notification from NRF

- `[10] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/nas/ies/5GSRegistrationType.cpp>`_ Registration Type
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Run mobility/periodic registration update procedure 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Decode registration request
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle registration request

- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Registration reject response message

- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Initialize registration accept
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Run registration procedure

- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle UE registration state change

- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Received Initial UE message 
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handle Initial UE message
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Get NAS context 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Received integrity and ciphered NAS messages
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Store NAS information in the NAS context

- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Find UE context 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Checking for the NAS context with GUTI, AMF UE NGAP ID

- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle UE-initiated deregistration process

- `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_app.cpp>`_ Delete event subscription
- `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_app.cpp>`_ Handle event exposure delete/unsubscribe
- `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_app.cpp>`_ Trigger N11 Deregistration Request
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handle PDU session resource release

- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handle UE context release request
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ handling UE context release command
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Send UE release command to source gNB
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handle UE context release complete 
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Delete gNB context 
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Remove UE context 

Authentication and Authorization
-------

* If the new AMF didn’t get any traces of the old AMF, then it starts an authorization and authentication procedure with the UE. It handles the identification process and asks for the authentication vectors from the AUSF. Then, it sends an authentication request to the UE to setup security keys and select security algorithms in the channels to make them secure for the transfer of the data.

* AMF controls all the NAS DL/UL transport channels for communication.

.. image:: photos/OAI_AMF3.png
  :alt: Alternative text

- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Check existing identification procedure 
- `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_app.cpp>`_ Generate 5g GUTI
- `[9] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/nas/ies/5GSMobilityIdentity.cpp>`_ Set 5G GUTI
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_config.cpp>`_ Configure GUAMI
- `[9] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/nas/ies/5GSMobilityIdentity.cpp>`_ Set SUCI with SUPI IMSI
- `[9] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/nas/ies/5GSMobilityIdentity.cpp>`_ Set IMEISV
- `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_app.cpp>`_ Check UE context with 5g-S-TMSI, if not available create a new one with RAN AMF ID
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_config.cpp>`_ Configure PLMN list
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Set PLMN support list
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Verify PLMN
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Set GUTI, AMF UE NGAP ID to NAS context
- `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_app.cpp>`_ Set SUPI, AMF UE NGAP ID, and RAN AMF ID to UE context
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle Identity response
 
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_config.cpp>`_ Configure AUSF
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Check ongoing authentication procedure 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Start authentication procedure
 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle authentication failure
 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ AKA confirmation from AUSF
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_config.cpp>`_ Configure integrity/ciphering algorithm
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Get Authentication vectors from AUSF or generate them  locally 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Authentication vector generator in UDM
 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle authentication response 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle authentication vector successful result
 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Encode NAS message protected 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle NAS UL and DL Transport messages
 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Check security header type
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Start security mode control procedure
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Select security algorithm
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Security context 
 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle security mode reject 
 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle Security mode complete
 
User Plane Setup
-------

* AMF retrieves all the AM subscription data from the UDM and requests the NSI(Network Slice Instances) information from the  NSSF to get the information about the SMF. Then, it sends an SM context setup request to the SMF to establish a default PDU session. After that, it authenticates the session and bearers to avoid data leakage. On the completion of the user plane setup procedure, AMF generates an AMF UE identity for the UE.

.. image:: photos/OAI_AMF4.png
  :alt: Alternative text

- `[12] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/udm_app/udm_app.cpp>`_ Handle Access Mobility subscription data retrieval
 
- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ Get NSI information from NSSF
- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ Discover SMF from NSI info
 
- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ Discover SMF
- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ SMF selection from configuration
 
- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ Handle PDU Session Create SM Context
- `[11] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/contexts/ue_context.cpp>`_ Add/Get/find PDU session context 
- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ Create PDU session context if not available
- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ Send notification for the subscribed events 
 
- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ Handle PDU session initial request
- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ Handle PDU session establishment request
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ send DL NAS buffer task N2
 
- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ Send UE authentication request
 
- `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_app.cpp>`_ Generate/Update AMF UE NGAP ID
- `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_app.cpp>`_ Generate AMF profile
 
Context Setup and Session Modification
-------

* After creating an AMF UE identity for the UE, AMF shares security capabilities, SMC messages, and registration acceptance messages under the initial context setup request with the gNB and UE.

* In the session modification request, AMF sends the updated session management resources to the SMF. The AMF also initiated a  session establishment request at the gNB level.  

.. image:: photos/OAI_AMF5.png
  :alt: Alternative text

- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handle initial context setup request
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Get UE security capability 
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Encode initial context setup request  
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Send Initial Context Setup Request 
 
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handle initial context setup response
 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle SMC messages
 
- `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_event.cpp>`_ Subscribe to UE Registration Status
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Registration complete handle
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle registration response
 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle Service Request
- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ Send PDU session update SM context request
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Send PDU session Update SM context request to SMF 
 
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Get NSSAI from the PDU session context 
 
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Received modification of PDU Session Resource request 
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handle PDU session resource modify
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Encode PDU session resource setup request 
 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Get the PDU session to be activated
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handle PDU Session resource procedure
- `[5] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n11.cpp>`_ Store corresponding info in the PDU session context 
 
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Encode DL NAS transport message
 
Handover Procedure
------- 

* AMF sends a paging request to the gNB.  During the handover procedure, gNB sends a handover notification to the AMF as UE changes the RAN area.  It sends the handover messages to the source gNB to direct the remaining resources from the other gNB. AMF also sends the handover request message to the target gNB for the continuation of data flow.

.. image:: photos/OAI_AMF6.png
  :alt: Alternative text

- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handle paging message
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle NAS signaling establishment request(create or update NAS context)
- `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_app.cpp>`_ Handle NAS signaling establishment message
 
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ handle UL/DL NAS transport message
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handling Handover required
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handle handover Notify 
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handover request message to be sent to target gNB
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Send handover command message to source gNB
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Handling handover requests ACK
 
- `[6] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n2.cpp>`_ Send handover preparation failure
 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Set 5G MM state 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Set 5G CM state 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle UE location change
- `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_event.cpp>`_ Subscribe UE location report
- `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_event.cpp>`_ UE reachability status
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle UE reachability status change
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_n1.cpp>`_ Handle UE connectivity state change
 
- `[7] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_statistics.cpp>`_ Update UE info
- `[7] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf/-/blob/master/src/amf-app/amf_statistics.cpp>`_ Update 5g MM state 
