OpenDaylight ML2 MechanismDriver
================================
OpenDaylight is an Open Source SDN Controller developed by a plethora of
companies and hosted by the Linux Foundation. The OpenDaylight website
contains more information on the capabilities OpenDaylight provides:

    http://www.opendaylight.org

Theory of operation
===================
The OpenStack Neutron integration with OpenDaylight consists of the ML2
MechanismDriver which acts as a REST proxy and passess all Neutron API
calls into OpenDaylight. OpenDaylight contains a NB REST service (called
the NeutronAPIService) which caches data from these proxied API calls and
makes it available to other services inside of OpenDaylight. One current
user of the SB side of the NeutronAPIService is the OVSDB code in
OpenDaylight. OVSDB uses the neutron information to isolate tenant networks
using GRE or VXLAN tunnels.

How to use the OpenDaylight ML2 MechanismDriver
===============================================
To use the ML2 MechanismDriver, you need to ensure you have it configured
as one of the "mechanism_drivers" in ML2::

    mechanism_drivers=odl

The next step is to setup the "[odl_rest]" section in either the ml2_conf.ini
file or in a plugins/opendaylight/odl_rest.ini file. An example is shown below::

    [odl_rest]
    password = admin
    username = admin
    url = http://192.168.100.1:8080/controller/nb/v2/neutron

When starting OpenDaylight, ensure you have the SimpleForwarding application
disabled or remove the .jar file from the plugins directory. Also ensure you
start OpenDaylight before you start OpenStack Neutron.

There is devstack support for this which will automatically pull down OpenDaylight
and start it as part of devstack as well. The patch for this will likely merge
around the same time as this patch merges.
