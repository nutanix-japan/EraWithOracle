.. title:: Nutanix Workshops HOWTO

.. toctree::
  :maxdepth: 2
  :caption: Virtual Bootcamps
  :name: _vbootcamps
  :hidden:

  vbootcamps/vbootcamps

.. toctree::
  :maxdepth: 2
  :caption: SE Getting Started
  :name: _se
  :hidden:

  se_reserve/se_reserve
  create/create
  stage/stage
  access/access

.. toctree::
  :maxdepth: 2
  :caption: FMM Getting Started
  :name: _fmm
  :hidden:

  fmm_reserve/fmm_reserve

.. _getting_started:

-----------------------
Nutanix Workshops HOWTO
-----------------------

The purpose of this living document is to provide a complete reference for SEs and Field Marketing on how to properly run a Nutanix Bootcamp (or Workshop) using RX, the Hosted POC environment, and the Nutanix Hands on Workshops platform.

.. note::

    Feedback and suggestions can be submitted to bootcamps@nutanix.com

Workshop Vs. Bootcamp
++++++++++++++++++++

**What's the difference?!**

Generally, we use **Workshops** to refer to internal or partner facing hands on lab content. **Bootcamps**, which this document will cover in-depth, are customer facing, funded marketing events that can be used for lead generation and accelerating deals.

The purpose of a bootcamp is to give attendees a close-up view of some of the powerful web-scale capabilities of the Nutanix Enterprise Cloud Platform, and showcase how simple Nutanix is to configure and manage.

The purpose of a bootcamp is to give attendees a close-up view of some of the powerful web-scale capabilities of the Nutanix Enterprise Cloud Platform, and showcase how simple Nutanix is to configure and manage.

Bootcamp Format
+++++++++++++++

Bootcamps are run as a 4-5 hour in-person event with lunch and swag included. The overall flow includes a brief presentation that provides a high-level overview of Nutanix and covers features in depth. Presentations are interspersed with demos and hands-on exercises on a live cluster to give participants a feel for the Nutanix environment. Attendees will use an online workbook (created and sent by the SE) to go through the exercises. Printed supplemental workbooks are also provided (ordered by FMMs) so that the attendees can annotate and make notes. The powerpoint slides and demos are arranged to be aligned chronologically with a customer’s Nutanix experience.

**Do you want to run a Technology Bootcamp on Test Drive?**

Then check out this `Wiki <https://confluence.eng.nutanix.com:8443/display/TDP/Bootcamps+on+Test+Drive>`_ guide on what to consider and how to do so. A few key points to note:
  - Test Drive can be used for running a virtual bootcamp, however it is not a direct replacement for `traditional Technology Bootcamps <https://confluence.eng.nutanix.com:8443/display/SEW/Bootcamps>`_ hosted on HPOC, but rather compliments the existing program.
  - Test Drive is not a panacea Certain bootcamps/products/solutions cannot be run on Test Drive (see the decision chart on the Wiki Guide noted above
  - The cloud is not infinite - if you plan on hosting a virtual bootcamp or event leveraging Test Drive, you must let the Test Drive team know ahead of time using this `form <https://ntnx.wufoo.com/forms/m3d5g6o13dj895/>`_ so adequate resources can be provisioned ahead of time so users aren't left waiting for a Test Drive Cluster to provision.

Ways To Run A Bootcamp
++++++++++++++++++++++

Marketing Event Bootcamp
........................

A Marketing hosted event for prospects to get hands-on with Nutanix solutions.

FMM Handles:

  - Creates Registration page
  - sends event emails
  - Salesforce tracking
  - Reserves the Marketing Cluster
  - Location & Food
  - Swag
  - Printed Materials (If Requested/Needed)

SE Handles:

  - Reserves HPOC if MKTG cluster is unavailable
  - Creates Event Specific Bootcamp from template in the Hands on Workshop platform
  - **Takes the created digital workbook link, and sends the to attendees before the bootcamp**
  - Stages the Marketing Cluster (or HPOC) with staging script.

Virtual Bootcamp
................

A 4-5 hour prospect or customer bootcamp that is hosted virtually over Zoom.

FMM Handles:

  - Creates Registration page
  - sends event emails
  - Salesforce tracking
  - Reserves the Marketing Cluster
  - Swag

SE Handles:

  - Reserves HPOC if MKTG cluster is unavailable
  - Creates Event Specific Bootcamp from template in the Hands on Workshop platform
  - **Takes the created digital workbook link, and sends the to attendees before the bootcamp**
  - Stages the Marketing Cluster (or HPOC) with staging script.

Account/Partner Based Bootcamp
..............................

This is a more targeted approach to running a Bootcamp. Great way to get a prospect some stick time to accelerate a deal, show an existing customer a product they do not currently use to go wide in the account, or lastly to enable a partner so they can go out and sell for us.

FMM Handles:

  - Creates Registration page
  - sends event emails
  - Salesforce tracking
  - Reserves the Marketing Cluster
  - Location (if not done onsite) & Food (If not handled by Acct/Channel team)
  - Swag
  - Printed Materials (If Requested/Needed)

SE Handles:

  - Reserves HPOC if MKTG cluster is unavailable
  - Creates Event Specific Bootcamp from template in the Hands on Workshop platform
  - **Takes the created digital workbook link, and sends the to attendees before the bootcamp**
  - Stages the Marketing Cluster (or HPOC) with staging script.



Bootcamp Materials & Resources
++++++++++++++++++++++++++++++

PDF version of printable workbooks (supplemental to the digital workbook) and customer-facing powerpoints: https://ntnx.how/PresentationDecks

SE Wiki Page for an In-depth look at Bootcamps [*Must be connected to VPN*]: `SE Wiki Page Bootcamps <https://confluence.eng.nutanix.com:8443/display/SEW/Bootcamps>`_

Internal Bootcamp Content Guide: `Bootcamp Content Guide & One Pagers <https://docs.google.com/document/d/1FzC2GX61nBP17qY6Dw-4d583nx6BPTsbO_eRszXIbmc/edit?usp=sharing>`_

Bootcamp Swag funded by corporate and ordered by Field Marketing: `Internal Program Store (Program Materials) <https://nutanix.jniwebshop.com/category/16/program-materials>`_

Printable Workbooks funded by corporate and ordered by Field Marketing: `Internal Program Store (Print on Demand) <https://nutanix.jniwebshop.com/category/74/print-on-demand>`_

Getting Help
++++++++++++

Have a question related to lab content or staging?

Come ask in `#technology-bootcamps <slack://channel?id=C0RAC0CHX&team=T0252CLM8>`_ or `#hands-on-workshops <slack://channel?id=C8WLPRTB3&team=T0252CLM8>`_.

Have a question related to RX, HPOC (including initial cluster Foundation)?

Come ask in `#rx-and-hpoc <slack://channel?id=C0JSE04TA&team=T0252CLM8>`_.

Have a question related to Frame, Parallels VDI, or Pulse VPN access?

Come ask in `#x-labs <slack://channel?id=CF6GRQ4TU&team=T0252CLM8>`_.
