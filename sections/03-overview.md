# Overview

The basic design of the patch format is a "VPATCH" component (defined
in Section 10.1) containing one or more "PATCH" components (defined
in Section 10.1.1), each specifying a path (which identifies one or
more components in the iCalendar object being patched), and other
components and properties that define the set of changes to be made.

When multiple "VPATCH" components are present in an iCalendar object,
the order in which they are applied is defined by the value of any
"PATCH-ORDER" properties in the "VPATCH" components.  The "VPATCH"
components are sorted in order from lowest "PATCH-ORDER" integer
value to highest, with any components not containing a "PATCH-ORDER"
property placed last.  The patch process is then applied in sorted
order (any components with the same "PATCH-ORDER" value can be
applied in any order).

No specific processing order is defined for multiple "PATCH"
components in a "VPATCH" component.

The "VPATCH" component also contains an optional "PATCH-VERSION"
property to allow future extensions to the format to be recognized.
This document only defines version number "1".  The "PATCH-VERSION"
property only needs to be present if the version number is greater
than "1".  If a patch processing engine is unable to handle the
indicated version it MUST reject the entire patch operation defined
by the enclosing iCalendar object, even if other "VPATCH" components
have a "PATCH-VERSION" number that is supported.

After applying a patch to an iCalendar object, the basic validity of
the resulting iCalendar object SHOULD be checked by the processing
engine (e.g., if the patch added an extra "DTSTART" property to a
"VEVENT" component that would be considered a violation of
[@RFC5545]'s cardinality rules for the "DTSTART" property in a
"VEVENT").  If this occurs, the patch operation MUST fail.

Other validity constraints can be applied if needed.  For example,
CalDAV [@RFC4791] requires that the "UID" property be the same in all
components in a calendar object resource stored on the server.  If a
patch operation adds a component to an iCalendar object with a
different "UID" value than the existing components, that result would
be an invalid CalDAV calendar object resource.  If other validity
constraints are violated, the patch operation MUST fail.

Any failure to process a "VPATCH" component, for whatever reason,
MUST result in the entire patch operation being cancelled, with the
iCalendar object being patched left in its original state.


