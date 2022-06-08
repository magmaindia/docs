Session Management Function
*********

| 
Overview
=======

SMF is a Control Plane(CP) function that manages all the session-related context with the UPF. It creates, updates, and removes sessions. It also allocates the IP addresses to each PDU session. SMF provides all the session parameters and supports the functions of UPF. SMF functions are collectively performed by MME, SGW-U, and PGW-U in the 4G System. This NF enables CUPS(Control and User Plane Separation) which decentralized the control plane and data forwarding components of the 4G System. It obtained all the rules and policies from the PCF through the N7 interface and route them to UPF to execute them in the respective sessions.


Session Configuration
=======

User Plane Setup
-------

* UE first needs to set up a User Plane connection with the 5G network during the registration process to access the 5G facilities. AMF sends a request to SMF to create a session management context to manage the PDU session of the UE. Then, the SMF selects the UPF that serves the best user plane experience to the UE. 


* SMF also establishes a default non-GBR QoS Flow without any packet filters to carry the UE’s traffic to the DN and vice versa. The default QoS Flow of the PDU Session allows exchanging the best-effort traffic with the Data Network(DN).

.. image:: photos/smf1.png
  :alt: Alternative text

- `[16] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ Create session context
- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Session context response
- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Set UPF node state 
- `[5] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalSessionManagerHandler.cpp>`_ `[16] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ Make create session request
- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Initiate default and static rules(mapping static PDR, FAR, URR, BAR)

Session Establishment
-------

* To create dedicated QoS flows for a UE, a UE initiated or network-initiated PDU Session Modification request is sent to the UPF. SMF schedules the static and dynamic rules for the session and sets the session-related rules and policies for the UPF, AMF, and UE. The SMF handles the session charging characteristics specific to a UE or a DNN. And the charging-related context is created based on per PDU session configuration. It also grants a fixed no. of units(bytes, seconds, etc) for the exchange of data packets.        


* The SMF also specifies the session parameters for the gNB and UE but instead of sending them directly, it sends them via AMF as no other Network Function communicates with them. SMF sends the session context to the AMF to transfer them to the respective gNB and UE.

.. image:: photos/smf2.png
  :alt: Alternative text

- `[12] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionReporter.cpp>`_ Report create session
- `[16] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ Session modification request 
- `[11] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionProxyResponderHandler.cpp>`_ Abort session request
- `[5] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalSessionManagerHandler.cpp>`_ Initialize session
- `[7] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/PipelinedClient.cpp>`_ `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Send session request to UPF
- `[1] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/AAAClient.cpp>`_ Create add sessions request

- `[4] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalEnforcer.cpp>`_ Handle rule scheduling
- `[4] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalEnforcer.cpp>`_ `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Scheduling static and dynamic rule 
- `[4] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalEnforcer.cpp>`_ Handle set session rules          
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Set static rules
- `[5] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalSessionManagerHandler.cpp>`_ Set session rules
- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Set PDR attributes
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Bind policy to bearer   
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Initiate session credit
- `[7] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/PipelinedClient.cpp>`_ Create subscriber quota state request 
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Set subscriber quota management
- `[10] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionCredit.cpp>`_ Get initial requested credits units
- `[6] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/MeteringReporter.cpp>`_ Initialize monitoring usage 
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Initialize charging credits
- `[18] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/UpfMsgManageHandler.cpp>`_ Set UPF session configuration

- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Receive charging credits
- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Process session response from UPF
- `[2] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/AmfServiceClient.cpp>`_ Handling SM session response and notification from SMF
- `[5] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalSessionManagerHandler.cpp>`_ Add session to directory record 
- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Handle state update to AMF
- `[16] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ Set SMF notification 
- `[16] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ Set AMF session context

Session Modification 
-------                                      

* SMF keeps on updating the rules, policies, charging criteria, and modified sessions based on requirements, network capabilities, and subscription policies. This modified context is sent again over the session modification request to UPF by SMF. The UPF either add up the new one with the existing rules and regulations or replaces them with the new ones. SMF also sends the session modification request due to session failure.


* It handles all the session-related context and also requests the session termination when there is no use. It sets reauthentication procedures where it authenticates each policy and charging parameters again. It also updates bearers at the gNB level.

.. image:: photos/smf3.png
  :alt: Alternative text

- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Update session context 
- `[8] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/RestartHandler.cpp>`_ Terminate previous session
- `[8] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/RestartHandler.cpp>`_ Cleanup previous sessions
- `[4] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalEnforcer.cpp>`_ Report session update/ failure event
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Handle update failure                       
                                                                      
- `[15] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStore.cpp>`_ Get default session update
- `[17] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/StoredState.cpp>`_ Get default update criteria
- `[4] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalEnforcer.cpp>`_ Update session request
- `[4] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalEnforcer.cpp>`_ `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Update session rules
- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Change PDR rules and update UPF
- `[7] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/PipelinedClient.cpp>`_ Activate flows for rules 
- `[4] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalEnforcer.cpp>`_ Handle activate UE flows callback   
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Update session with policy
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ classify policy activation 
- `[7] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/PipelinedClient.cpp>`_ Setup policy 
- `[7] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/PipelinedClient.cpp>`_ Update subscriber quota state
- `[3] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/ChargingGrant.cpp>`_ Get session credit update 
- `[4] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalEnforcer.cpp>`_ `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Update charging credits
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Set and increment data metrics
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Insert PDR rules to the rules of session list

- `[3] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/ChargingGrant.cpp>`_ Set reauthentication state
- `[4] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalEnforcer.cpp>`_ Initiate charging reauthentication 
- `[4] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalEnforcer.cpp>`_ `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Initiate policy re-authentication for session
- `[11] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionProxyResponderHandler.cpp>`_ Policy reauthentication 

- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Get dedicated bearer updates
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Update bearer deletion/creation request
- `[4] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalEnforcer.cpp>`_ Handle session update response

Deactivation of Session
-------                            

* SMF supports activation and deactivation of the User Plane connection of a PDU session. The limitation of SMF is it only supports UE-initiated deactivation. SMF deactivates and releases the session-related context from the UPF. First, it removes rules and then terminates the session at the end. There can many reasons for session deactivation such as unspecified failure, user inactivity, etc.


* SMF tracks User Plane traffic updates and manages the state of the PDU sessions.

.. image:: photos/smf4.png
  :alt: Alternative text

- `[6] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/MeteringReporter.cpp>`_ Report traffic
- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Move to active/inactive state
- `[16] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ PDU session inactive 

- `[1] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/AAAClient.cpp>`_ Create deactivate session request
- `[7] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/PipelinedClient.cpp>`_ Deactivate flows for rules
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Deactivate static flows

- `[16] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ Initiate release session  
- `[4] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalEnforcer.cpp>`_ Schedule termination
- `[9] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/RuleStore.cpp>`_ `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Remove all PDR, FDR rules
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Remove scheduled dynamic rules
- `[13] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Clear session metrics
- `[12] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionReporter.cpp>`_ Report terminate session
- `[5] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/LocalSessionManagerHandler.cpp>`_ End session request
