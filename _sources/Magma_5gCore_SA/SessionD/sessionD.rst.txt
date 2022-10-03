Magma SessionD Service `[0] <https://github.com/magma/magma/tree/master/lte/gateway/c/session_manager>`_
*********

.. image:: photos/SessionD.jpg
 :align: center
 :alt: Alternative text

SessionD is the main service responsible for managing and enforcing session configurations. It also coordinates the lifecycle of a session. AccessD calls Sessiond for the session's creation, modification, and termination. Compared with network functions, it can be considered as Session Management Function(SMF). 

SessionD's Interface with other Services
=======

SessionD <——> AccessD
-------

SM Context Request from AccessD for the establishment of a session.


SessionD <——> PipelineD
-------

Responsible for policy and QoS enforcement. Pipelined receives any relevant policy and QoS configuration from SessionD and periodically reports usage accordingly.

SessionD <——> PolicyDB
-------

Responsible for propagating any session or policy configuration that sessioD must enforce.


Architecture of SessionD
=======

SessionD is created by combining different components such as:

- Message Manager Handler `[29] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_
- M5g-Enforcer `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_
- Session State `[26] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_
- Session Store `[28] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStore.cpp>`_
- Redis Store (when stateless feature is enabled) `[16] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/RedisStoreClient.cpp>`_
- Memory Store (when stateless feature is disabled) `[10] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/MemoryStoreClient.cpp>`_
- Session Proxy Responder Handler `[24] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionProxyResponderHandler.cpp>`_
- Session Credit `[20] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionCredit.cpp>`_



.. image:: photos/Architecture.jpg
  :alt: Alternative text

Call Flow
=======

PDU Session Establishment
-------



.. image:: photos/Establishment.jpg
  :alt: Alternative text

- `[29] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ Handling set message from AMF
- `[29] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ Send create sesion
- `[29] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ Set Session ID
- `[29] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ M5g enforcer initialisation
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ New Session state object 
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Creating sesion state object  with ‘Creating’ state
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Writing session in session map
- `[28] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStore.cpp>`_ Write session
- `[28] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStore.cpp>`_ Set current version of session
- `[28] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStore.cpp>`_ Set fsm state
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Process static and dynamic rules to install
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Session state change
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ `[26] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Charging Grant Intialised
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ `[26] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Monitoring Grant Initialised
- `[28] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStore.cpp>`_ Rule map addition, charging and monitoring criteria
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ M5g send session request to UPF
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Set subscribed qos,
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Set priority level
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Set preemption capability
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Set PDR, FAR, URR and BAR mapping
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Set UPF node
- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/PipelinedClient.cpp>`_ PipelinedD create session request
- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/PipelinedClient.cpp>`_ Set subscriber ID, Teid
- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/PipelinedClient.cpp>`_ QOS flow setup done
- `[2] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/AmfServiceClient.cpp>`_ Handle response to AccessD
- `[2] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/AmfServiceClient.cpp>`_ Handle notification to AccessD




Session Release/Termination
-------

.. image:: photos/Termination.jpg
  :alt: Alternative text

- `[29] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ Initiate Session Release
- `[29] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_  M5g Enforcer proceeds the  release session
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ `[28] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStore.cpp>`_ Session store find session 
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Get session ID
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ M5g start session termination
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ `[26] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Set FSM state  in Session State 
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ `[26] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Set current version of session
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ PDR Map Erase
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Remove all rules for termination  

If there is termination because of no required report from UPF then follow the below steps

- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ M5g complete termination
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Find session from Session Store
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ `[25] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionReporter.cpp>`_ Termination in controller
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ `[26] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionState.cpp>`_ Remove all PDR, FAR rules from session state
- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Remove session from session map

Final Steps for both the cases

- `[27] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionStateEnforcer.cpp>`_ Update session in session store
- `[29] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SetMessageManagerHandler.cpp>`_ Successfully released and updated session store of subscriber
- `[14] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/PipelinedClient.cpp>`_ PipelineD deactivate flow for rules



Paging Request
-------


.. image:: photos/Paging.jpg
  :alt: Alternative text

- `[33] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/UpfMsgManageHandler.cpp>`_ PipelineD sends paging request to sessionD
- `[33] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/UpfMsgManageHandler.cpp>`_ Local F-TEID
- `[33] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/UpfMsgManageHandler.cpp>`_ UE IP address
- `[24] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionManagerServer.cpp>`_ SessionD receives the message
- `[24] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionManagerServer.cpp>`_ Get the subscriber ID from MobilityD w.r.t UE IP address
- `[24] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/SessionManagerServer.cpp>`_ SessionD reads the session map from session store.
- `[2] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/AmfServiceClient.cpp>`_ SessionD sends the notification to AccessD
- `[2] <https://github.com/magma/magma/blob/master/lte/gateway/c/session_manager/AmfServiceClient.cpp>`_ Handle Notification to AccessD





