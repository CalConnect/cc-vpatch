# Adding or Updating Properties

Any iCalendar property defined in the "PATCH" component (referred to
below as the "action property") is treated as either an addition to
the target component, or as an update of an existing property in the
target component.  A "PATCH-ACTION" (Section 10.4) property parameter
can be defined on action properties and is used to control how the
action is processed.  Any "PATCH-ACTION" property parameter MUST be
removed from the action property when it is added to the target
component.  The following rules are used to process such properties:

* If the action property does not contain a "PATCH-ACTION" property
  parameter, or contains a "PATCH-ACTION" property parameter with
  the default value "BYNAME", then all properties with the same
  name in the target component are removed, and the action property
  is added to the target component.

* If the action property contains a "PATCH-ACTION" property
  parameter with the value "CREATE", then the action property is
  added to the target component.

* If the action property contains a "PATCH-ACTION" property
  parameter with the value "BYVALUE", then all properties with the
  same name and same value in the target component are removed, and
  the action property is added to the target component.

* If the action property contains a "PATCH-ACTION" property
  parameter with the value starting with "BYPARAM", then all
  properties with the same name and a property parameter that
  matches the one that is part of the "PATCH-ACTION" property
  value, in the target component are removed, and the action
  property is added to the target component.

The "PATCH-ACTION=BYNAME" operation is used for adding or updating
"singleton" properties - properties that only appear once in a given
iCalendar component (e.g., "DTSTART", "DTEND", "LOCATION", etc).

The "PATCH-ACTION=CREATE" operation is used for adding "multi-
occurring" properties - properties that can appear more than once in
a given iCalendar component (e.g., "ATTENDEE", "ATTACH", "EXDATE",
etc).

The "PATCH-ACTION=BYVALUE" operation is used for updating a specific
"multi-occurring" property that can be uniquely identified by its
value (e.g., the "ATTENDEE" property can appear multiple times in a
"VEVENT" component, but each property will have a unique value in
that component).  This operation cannot be used when the value of the
property is being changed.  Instead, the "PATCH-ACTION=BYPARAM"
operation can be used to identify the target property.

The "PATCH-ACTION=BYPARAM" operation is used for updating a specific
"multi-occurring" property that can be uniquely identified by a
parameter value that is the same in the action and target properties.

There may be some situations where a multi-occurring property cannot
be uniquely identified.  In such cases, the solution to updating one
or more of them is to use a "PATCH-ACTION=BYNAME" to replace all the
existing properties with one new one, then use "PATCH-ACTION=CREATE"
to add back others that are unchanged or also being updated.  Whilst
this is not ideal, it is anticipated that these situations can be
avoided by adding appropriate property parameters with unique values
to help disambiguate the multi-occurring properties.

