Open Standard
=============

OpenSMARTS is an open standard which welcomes contributions from the community.
There is no membership requirements and fees associated with contributing. All
communication is done publicly on *the mailing list*. Private communications
between a selected group may be used but at the very least, the results and the
arguments should be made publicly if this is to lead to a modification of the
Standard.

Versioning
----------

The OpenSMARTS specification's versioning uses a major and minor version
number. Incrementing the major version number allows for changes to the
standard that make the syntax more strict or change semantics. This should
not happen too often to ensure implementations that aim to be compliant
do not have to be changed every few moths. Although there is no formal
voting or approval process, changes only happen after discussion within
the community and interested parties. Also, to maintain backwards
compatibility major changes to the syntax and or semantics are not possible,
if this is desired, these should be submitted as extensions.

Any change to the actual specification (sections `grammar`, `reading`
and `writing` increment the minor version number. Changes to other sections
may be made without changing the version. All maintainer may apply these
changes without consulting the other maintainers if they feel confident
that the changes are valid and do not break the specification. In case
of doubt, the mailing list should be used to verify the changes.

.. _proposals:

Proposals
---------

Accepted
^^^^^^^^

Pending
^^^^^^^

Contributing
------------

Corrections
^^^^^^^^^^^

Corrections can be proposed  on the mailing list. An informal email is
sufficient. Any maintainer can then apply the corrections when they feel
confident the correction is right.

Clarifications
^^^^^^^^^^^^^^

Clarifications may also be suggested on the mailing list and can be
aplied by any maintainer.

Submitting Proposals
--------------------

A proposal may also be submitted using the mailing list. Unlike corrections
and clarifications, proposals are usually longer and require more discussion.
These will be listed in the :ref:`Proposals` section and, if accepted, will
become part of the next major revision of the specification.

Submitting Extensions
---------------------

Extensions may be added any time since these are not required to be supported
in order to be OpenSMARTS compliant. As always, the mailing list should be used
to include these.

Guidelines for Maintainers
--------------------------

The sources for the OpenSMARTS specification are available from
`github <http://www.github.com/timvdm/OpenSMARTS>`_ and can be forked to make
changes. Please remember that the minor version number should be incremented
when making any change to the specification sections.

Depictions
^^^^^^^^^^

Depictions may be included but should always be generated using the process
described here. The chemical file format containing the molecule to be depicted
should use the Chemical Markup Lanuage or \*.cml files and contain 2D
coordinates. When adding or modifying a \*.cml file, the script/depict.py
scitpt should be used to convert these to \*.png images::

  OpenSMARTS $ python scripts/depict.py

This script uses OpenBabel which should be installed. The script should be run
from the OpenSMARTS directory.

Images
^^^^^^

Any images may be imcluded provided that there are no copyright issues. These
have to be in the Portable Netwok Graphics \*.png format. In the case of vector
graphics, the vector graphics file may also be included in the Scalable Vector
Graphics format for future revision if needed. The open source InkScape
application can be used to create \*.svg vector graphcs and allows these to be
exported to \*.png. All \*.png and \*.svg files are placed in the source
directory.
