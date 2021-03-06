.. _devices_module:


devices -- Configure/Modify/Unconfigure devices
===============================================

.. contents::
   :local:
   :depth: 1


Synopsis
--------

This module facilitates a. Configuration of a defined specified device or all devices b. Modification of attributes of a device in 'Defined/Available' state c. Unconfigure/stop of a device in 'Available' state



Requirements
------------
The below requirements are needed on the host that executes this module.

- AIX



Parameters
----------

  force (optional, bool, False)
    Forces the change/unconfigure operation to take place on a locked device.


  recursive (optional, bool, False)
    Specifies to unconfigure the device and its children recursively.


  chtype (optional, str, both)
    Specifies the change type. *reboot* - Changes are applied to the device when the system is rebooted *current* - Changes the current state of the device temporarily *both* - Changes are applied both to the current state of the device and the device database


  state (optional, str, available)
    Specifies the action to be performed on the device. *available* - If the device is in 'defined' state, configure device else change device attributes when in 'available' state *defined* - If the device is in 'available state', Unconfigure/Stop device else change device attributes when in ''defined' state


  parent_device (optional, str, None)
    While modifying device, specifies the parent device of the device to be updated. For unconfigure/stop operation, specifies the parent device whose children needs to be unconfigured recursively.


  rmtype (optional, str, unconfigure)
    Specifies whether to unconfigure/stop the device. *unconfigure* - Changes are applied to the device when the system is rebooted *stop* - Changes the current state of the device temporarily


  device (optional, str, all)
    Specifies the device logical name in the Customized Devices object class. *all* - specifies all devices to be configured when *state=available*


  attributes (optional, dict, None)
    When *state=available*, specifies the device attribute-value pairs used for changing specific attribute values.









Examples
--------

.. code-block:: yaml+jinja

    
    - name: Configure a device
      devices:
        device: proc0
        state: available

    - name: Modify an attribute of a device
      devices:
        device: en1
        state: available
        attributes:
          mtu: 900
          arp: off

    - name: Unconfigure a device
      devices:
        device: proc0
        state: defined



Return Values
-------------

msg (always, str, )
  The execution message.


stderr' (If the command failed., str, )
  The standard error.


rc' (If the command failed., int, )
  The return code.


stdout' (If the command failed., str, )
  The standard output.





Status
------




- This module is not guaranteed to have a backwards compatible interface. *[preview]*


- This module is maintained by community.



Authors
~~~~~~~

- AIX Development Team (@pbfinley1911)

