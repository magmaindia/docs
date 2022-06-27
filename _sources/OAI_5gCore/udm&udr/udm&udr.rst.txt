Unified Data Management and Unified Data Repository 
*********

.. image:: photos/udm1.png
 :align: center
 :alt: Alternative text

UDM `[1] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm>`_
=======

UDM(Unified Data Management) is a front-end part of the UDR which makes subscription-related data available to the different network functions at their request. It supplies the information to the network functions such as AMF, AUSF, SMF, etc. It is similar to HSS in 4G. The functions of HSS in the 5G system are split into AUSF and UDM. UDM doesn’t serve policy-related context to the PCF as PCF communicates directly to the UDR for its data.

UDR `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr>`_
=======

UDR(Unified Data Repository) is the network function that acts as a database for the 5GS(5G System). It mainly stores 4 different subscription-related information: subscription data, policy data, structured data for exposure, and application data. The subscription data is available via UDM(Unified Data Management) which acts as a front-end and serves essential services to the different network functions on request. All the network functions use UDR to store their relative data and retrieve them through request messages.

Data Management
=======

Register Context
-------

* Before the registration procedure is carried out by the UE, all the network functions need to be registered with the NRF to get located by the network functions which require their services. After the registration of NFs, each NF has to register all data with the UDR. But not all the functions directly communicate with the UDR, some main NFs interact with the UDM for their data registration. UDM acts as an intermediary between NFs and UDR.

* UDM and UDR play an important role in authentication and authorization. It stores and generates different authentication parameters and creates authentication status which describes at what stage the authentication procedure resides. Then, the AUSF subscribed to the Authentication updates so that it does not have to request each time for any change in the parameters or any data add-ups.

* AMF then requests for the Access Management(AM) subscription data from the UDM, which requests to the UDR. AMF also requests the Subscriber data for the identification of the subscriber in the network and subscribed for further updates. And also send update requests of SDM(Subscriber Data Management) on modification.

.. image:: photos/udm2.png
  :alt: Alternative text
 
- `[4] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/udm_app/udm_config.cpp>`_ UDM config support features
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/udm_app/udm_app.cpp>`_ Handle AMF registration 
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle create AMF context
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle query AMF context
 
- `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/udm_app/udm_app.cpp>`_ Handle generate authentication data request
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/5gaka/authentication_algorithms_with_5gaka.cpp>`_ Generate AUTN
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/5gaka/authentication_algorithms_with_5gaka.cpp>`_ Generate KDF
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/5gaka/authentication_algorithms_with_5gaka.cpp>`_ Derive Kseaf
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/5gaka/authentication_algorithms_with_5gaka.cpp>`_ Derive Kausf
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/5gaka/authentication_algorithms_with_5gaka.cpp>`_ Derive Kamf
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/5gaka/authentication_algorithms_with_5gaka.cpp>`_ Derive Knas
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/5gaka/authentication_algorithms_with_5gaka.cpp>`_ Derive Kgnb
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/5gaka/authentication_algorithms_with_5gaka.cpp>`_ Derive Kasme (Key Access Security Management Entity)
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/5gaka/authentication_algorithms_with_5gaka.cpp>`_ Generate vector
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/5gaka/authentication_algorithms_with_5gaka.cpp>`_ Derive SQN MS 
- `[2] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/5gaka/authentication_algorithms_with_5gaka.cpp>`_ `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/udm_app/udm_app.cpp>`_ Derive Random
- `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/udm_app/udm_app.cpp>`_ Response data(ausf)
- `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/udm_app/udm_app.cpp>`_ Handle confirm Authentication(udr)
 
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle create authentication status
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle query authentication status 
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ handle modify authentication subscription
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ handle read authentication subscription 
 
- `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/udm_app/udm_app.cpp>`_ Handle Access Mobility subscription data request
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle query SDM subscription
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle create SDM subscription
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle query SD subscription
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle Update SDM subscription
 
Deregistration of functions
-------

* NSSF retrieves network slicing subscription data from the UDM via SBI(Service-based Interface). As AMF registered with the NRF before storing its data inside the UDR, similarly SMF carried out the same procedure and registered the data along with its non-3gpp context with the network. And on the requirement, SMF retrieves all of its data.

* On the deregistration procedure, all the session context and the subscription-related data are removed from the UDR. The authentication-related parameters and status update algorithm are also deleted in this process.

.. image:: photos/udm3.png
  :alt: Alternative text

- `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/udm_app/udm_app.cpp>`_ Handle slice selection subscription data retrieval 
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle query SMF registration
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle query SMF registration list
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle create SMF context non-3gpp

- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle query SM data
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle query SMF select data

- `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/udm_app/udm_app.cpp>`_ Handle session management subscription data retrieval
- `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/udm_app/udm_app.cpp>`_ Handle subscription creation

- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle delete SMF context
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle remove SDM subscription
- `[3] <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm/-/blob/master/src/udm_app/udm_app.cpp>`_ Handle delete authentication
- `[1]  <https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr/-/blob/master/src/udr_app/udr_app.cpp>`_ Handle delete authentication status 
