.. title:: Nutanix New Hire Training

.. toctree::
  :maxdepth: 2
  :caption: NHT Labs
  :name: _labs
  :hidden:

  diyfoundation/diyfoundation
  
  xray/xray
  
  files_deploy/files_deploy



.. _getting_started:

Getting Started
===============

.. raw:: html

  <strong><font color="red">Do not start any labs before being told to do so by your
  instructor.</font></strong><br><br>

Welcome to Nutanix New Hire Training! Carefully review the **Overview** section of each lab before proceeding with the exercise.

Cluster Details
+++++++++++++++

Using the spreadsheet below, locate your **Cluster ID** and corresponding details for your group's assigned cluster.

.. raw:: html

  <iframe src=https://docs.google.com/spreadsheets/d/1JAXYpRsNkzYUys_Sny4jRUtY8AeJymqMVEPwY_si3yo/edit#gid=847779028gid=0&amp; single=false&amp;widget=false&amp;chrome=false&amp;headers=false&amp;range=a1:m41 style="position: relative; height: 500px; width: 100%; border: none"></iframe>

Cluster Access
++++++++++++++

The Nutanix Hosted POC environment can only be accessed via VPN or virtual desktop. **It is recommended that the VPN be used to complete these labs.**

GlobalProtect VPN Access
........................

Browse to https://gp.nutanix.com.

Log in with your OKTA credentials.

Download and install the appropriate GlobalProtect agent for your operating system.

Launch GlobalProtect and configure **gp.nutanix.com** as the **Portal** address.

.. note::

  You can also leverage the legacy VPN solution, Pulse Secure. Connect and download the client from https://sslvpn.nutanix.com.

XenDesktop Access
.................

.. note::

  If you are attending NHT and in a non-SE role (e.g. CSM, Services) you DO NOT have NUTANIXDC.local credentials. Alternate credentials will be provided in class to access the HPOC XenDesktop environment.

Download and install the `Citrix Workspace client <https://www.citrix.com/downloads/workspace-app/>`_. Do **NOT** enable Single Sign-On (SSO) during installation.

In your browser, log in at https://citrixready.nutanix.com with your **NUTANIXDC.local** credentials. This username should match your Corp AD (Okta) username (first.last).

The default password is **welcome123**. You will be prompted to change your password.

.. image:: images/1.png

