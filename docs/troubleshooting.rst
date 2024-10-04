===============
Troubleshooting
===============

.. important::

    This guide assumes that the latest version of the software packages is used.
    Before proceeding, please check that they are up to date.

X-Series Arms
=============

See the guides on `the X-Series Arms documentation site`_.

.. _`the X-Series Arms documentation site`: https://docs.trossenrobotics.com/interbotix_xsarms_docs/troubleshooting.html

SLATE Base
==========

See the guides on `the SLATE Base documentation site`_.

.. _`the SLATE Base documentation site`: https://docs.trossenrobotics.com/slate_docs/troubleshooting.html

Device Connection Issues
========================

USB Hubs
--------

If using an ALOHA version with the Sabrent 7-Port USB Hubs and fail to see all connected devices when running bringup, check the following steps:

*   Check that all cables are fully inserted into their ports on both ends.
*   Check that all buttons corresponding to occupied ports are pressed.
    The LEDs should be lit and the button should be depressed.
*   We have found that, occasionally, switching the port that a cable is plugged into can resolve connection issues.
    Make sure that the button of the now empty port is pressed again so that it is released, and that the button of the now occupied port has been depressed.
