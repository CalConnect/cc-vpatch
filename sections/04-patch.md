# PATCH Component

A "PATCH" component (defined in Section 10.1.1) MUST contain one
"PATCH-TARGET" property whose value is an iCalendar path (see
Section 5) that identifies components within the iCalendar object
being patched (see Section 11.2 for special handling of components
representing recurring items).  The set of components thus identified
are the "target components" for the patch operations.  The set of
patch operations defined by the other components and properties
present in the "PATCH" component are then applied to each target
component (in the order specified below).  If a "PATCH-TARGET"
property does not match any components in the iCalendar object being
patched, then the patch operation MUST succeed without any changes
being applied to the iCalendar object being patched.

Four patch operations are supported:

* Component additions or updates: any components within the "PATCH"
  component are considered to be additions or updates (see
  Section 6).

* Property additions or updates: any properties (other than those
  whose name starts with the "PATCH-" prefix) are considered to be
  additions or updates (see Section 7).

* Component, property, property parameter, property value, or
  property parameter value deletion: indicated by the present of
  one or more "PATCH-DELETE" properties (see Section 8).

* Property parameter additions or updates: indicated by the
  presence of one or more "PATCH-PARAMETER" properties (see
  Section 9).

When processing a "PATCH" component, the processing engine MUST
follow this order:

*  Process all "PATCH-DELETE" properties first.
*  Process all "PATCH-PARAMETER" properties second.
*  Process all other components third.
*  Process all non "PATCH-" prefixed properties fourth.
