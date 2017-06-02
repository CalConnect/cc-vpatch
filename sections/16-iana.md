#  IANA Considerations

##  Component Registrations

This document defines the following new iCalendar components to be
added to the registry defined in Section 8.3.1 of [@RFC5545]:

    +-----------+---------+-------------------------+
    | Component | Status  | Reference               |
    +-----------+---------+-------------------------+
    | VPATCH    | Current | RFCXXXX, Section 10.1   |
    | PATCH     | Current | RFCXXXX, Section 10.1.1 |
    +-----------+---------+-------------------------+

##  Property Registrations

This document defines the following new iCalendar properties to be
added to the registry defined in Section 8.3.2 of [@RFC5545]:

    +-----------------+---------+-------------------------+
    | Property        | Status  | Reference               |
    +-----------------+---------+-------------------------+
    | PATCH-VERSION   | Current | RFCXXXX, Section 10.2.1 |
    | PATCH-ORDER     | Current | RFCXXXX, Section 10.2.2 |
    | PATCH-TARGET    | Current | RFCXXXX, Section 10.3.1 |
    | PATCH-DELETE    | Current | RFCXXXX, Section 10.3.2 |
    | PATCH-PARAMETER | Current | RFCXXXX, Section 10.3.3 |
    +-----------------+---------+-------------------------+

##  Parameter Registrations

This document defines the following new iCalendar parameters to be
added to the registry defined in Section 8.3.3 of [@RFC5545]:

    +--------------+---------+-----------------------+
    | Property     | Status  | Reference             |
    +--------------+---------+-----------------------+
    | PATCH-ACTION | Current | RFCXXXX, Section 10.4 |
    +--------------+---------+-----------------------+

##  Property and Parameter Value Registries

Two new IANA registrys for iCalendar elements have been added.
Additional codes MAY be used, provided the process described in
Section 8.2.1 of [@RFC5545] is used to register them, using the
template in Section 8.2.6 of [@RFC5545].

###  Patch Version Registry

The following table has been used to initialize the Patch Version
Registry:


    +---------------+---------+-----------+
    | Patch Version | Status  | Reference |
    +---------------+---------+-----------+
    | 1             | Current | RFCXXXX   |
    +---------------+---------+-----------+

###  Patch Action Registry

The following table has been used to initialize the Patch Action
Registry:

    +--------------+---------+-----------------------+
    | Patch Action | Status  | Reference             |
    +--------------+---------+-----------------------+
    | CREATE       | Current | RFCXXXX, Section 10.4 |
    | BYNAME       | Current | RFCXXXX, Section 10.4 |
    | BYVALUE      | Current | RFCXXXX, Section 10.4 |
    | BYPARAM      | Current | RFCXXXX, Section 10.4 |
    +--------------+---------+-----------------------+


