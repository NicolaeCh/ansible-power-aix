.. _suma_module:


suma -- Download/Install fixes, SP or TL on an AIX server.
==========================================================

.. contents::
   :local:
   :depth: 1


Synopsis
--------

Creates a task to automate the download and installation of technology level (TL) and service packs (SP) from a fix server using the Service Update Management Assistant (SUMA).



Requirements
------------
The below requirements are needed on the host that executes this module.

- AIX >= 7.1 TL3
- Python >= 2.7



Parameters
----------

  save_task (optional, bool, False)
    Saves the SUMA task. The task is saved, allowing scheduling information to be added later.

    Can be used if *action=download* or ``action=preview``.

    If *oslevel* is a TL and *last_sp=yes* the task is saved with the last SP available at the saving time.


  description (optional, str, None)
    Display name for SUMA task.

    If not set the will be labelled '*action* request for oslevel *oslevel*'

    Can be used for *action=download* or *action=preview*.


  task_id (optional, str, None)
    SUMA task identification number.

    Can be used if *action=list* or *action=edit* or *action=delete* or *action=run* or *action=unschedule*.

    Required when *action=edit* or *action=delete* or *action=run* or *action=unschedule*.


  download_dir (optional, path, /usr/sys/inst.images)
    Directory where updates are downloaded.

    Can be used if *action=download* or ``action=preview``.


  extend_fs (optional, bool, True)
    Specifies to automatically extends the filesystem if needed. If no is specified and additional space is required for the download, no download occurs.

    Can be used if *action=download* or ``action=preview``.


  sched_time (optional, str, None)
    Schedule time. Specifying an empty or space filled string results in unscheduling the task. If not set, it saves the task.

    Can be used if *action=edit*.


  oslevel (optional, str, Latest)
    Specifies the Operating System level to update to;

    ``Latest`` indicates the latest SP suma can update the target to (for the current TL).

    ``xxxx-xx(-00-0000``) sepcifies a TL.

    ``xxxx-xx-xx-xxxx`` or ``xxxx-xx-xx`` specifies a SP.

    Required when *action=download* or *action=preview*.


  download_only (optional, bool, False)
    Download only. Do not perform installation of updates.

    Can be used if *action=download* or ``action=preview``.


  action (optional, str, preview)
    Controls the action to be performed.

    ``download`` to download and install all fixes.

    ``preview`` to execute all the checks without downloading the fixes.

    ``list`` to list all SUMA tasks.

    ``edit`` to edit an exiting SUMA task.

    ``run`` to run an exiting SUMA task.

    ``unschedule`` to remove any scheduling information for the specified SUMA task.

    ``delete`` to delete a SUMA task and remove any schedules for this task.

    ``config`` to list global SUMA configuration settings.

    ``default`` to list default SUMA tasks.


  metadata_dir (optional, path, /var/adm/ansible/metadata)
    Directory where metadata files are downloaded.

    Can be used if *action=download* or ``action=preview`` when *last_sp=yes* or *oslevel* is not exact, for example *oslevel=Latest*.


  last_sp (optional, bool, False)
    Specifies to download the last SP of the TL specified in *oslevel*. If no is specified only the TL is downloaded.

    Can be used if *action=download* or ``action=preview``.









Examples
--------

.. code-block:: yaml+jinja

    
    - name: Check, download and install system updates for the current oslevel of the system
      suma:
        action: download
        oslevel: Latest
        download_dir: /usr/sys/inst.images

    - name: Check and download required to update to SP 7.2.3.2
      suma:
        action: download
        oslevel: '7200-03-02'
        download_only: yes
        download_dir: /tmp/dl_updt_7200-03-02
      when: ansible_distribution == 'AIX'

    - name: Check, download and install to latest SP of TL 7.2.4
      suma:
        action: download
        oslevel: '7200-04'
        last_sp: yes
        extend_fs: no

    - name: Check, download and install to TL 7.2.3
      suma:
        action: download
        oslevel: '7200-03'



Return Values
-------------

msg (always, str, Suma preview completed successfully)
  The execution message.


meta (always, dict, {'meta': {'messages': ['Parameter last_sp=yes is ignored when oslevel is a TL ', 'Suma metadata: 7200-02-01-1732 is the latest SP of TL 7200-02', '...']}})
  Detailed information on the module execution.


  messages (always, list, Parameter last_sp=yes is ignored when oslevel is a TL 7200-02-00)
    Details on errors/warnings/inforamtion



stderr (always, str, )
  The standard error.


stdout (always, str, )
  The standard output.





Status
------




- This module is not guaranteed to have a backwards compatible interface. *[preview]*


- This module is maintained by community.



Authors
~~~~~~~

- AIX Development Team (@pbfinley1911)

