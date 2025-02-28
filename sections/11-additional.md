# Additional Considerations

## Handling Default Properties and Parameters

iCalendar properties and property parameters can have default values,
which allows those items to be omitted from the iCalendar data, but
with the default value assumed.  A patch operation might add
properties or property parameters with default values.  A patch
processing engine MAY choose to remove properties or property
parameters with default values from the patched iCalendar object.

## Handling Recurrences

Recurring events (or other types of component) in iCalendar are defined
by the presence of "RRULE", "RDATE", and "EXDATE" properties in a
"master" iCalendar component.  Those rules produce a set of "generated"
instances.  In some cases specific "generated" instances are changed,
resulting in the presence of "overridden" components, which are
identified by having the same "UID" property value as the "master"
component, and a "RECURRENCE-ID" property whose value matches the start
time of the corresponding "generated" instance (which can be different
from the actual start time of the overridden instance).

When a set of master and overridden recurring components exist in the
iCalendar object being patched, each can be uniquely targeted by using
the "RID=" match item in the component segment of the path value of a
"PATCH-TARGET" or "PATCH-DELETE" property.  To target the master
component, a "RID=M" match item is used.  To target an overridden
component, the "RID=" value is set to the value of the "RECURRENCE-ID"
property in the overridden component.

Patch commands can also be used to implicitly create overridden
components in the iCalendar object being patched by specifying a path
with a "RID=" match item, using what would be the overridden component's
"RECURRENCE-ID" value if it existed as a separate component.  This is
useful when an overridden component needs to be added, but the changes
to it are small (e.g., an instance where only the summary of the event
is different).

If the value of a "RID=" match item in a path does not correspond to an
existing instance (either because its value does not match a "generated"
instance, or its value matches an "EXDATE" in the "master"
component), then the patch operation MUST fail.

For example, consider the following daily recurring event:

{align=left}
```
BEGIN:VCALENDAR
PRODID:test
VERSION:2.0
BEGIN:VEVENT
UID:1234
DTSTART:20160902T120000Z
DURATION:PT1H
SUMMARY:Master component
RRULE:FREQ=DAILY
END:VEVENT
END:VCALENDAR
```

The following patch command could be used to update the "SUMMARY"
property value of the second instance of the recurring event:

{align=left}
```
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234][RID=20160903T120000Z]
SUMMARY:Override second instance
END:PATCH
END:VPATCH
```

which results in the following updated iCalendar component:

{align=left}
```
BEGIN:VCALENDAR
PRODID:test
VERSION:2.0
BEGIN:VEVENT
UID:1234
DTSTART:20160902T120000Z
DURATION:PT1H
SUMMARY:Master component
RRULE:FREQ=DAILY
END:VEVENT
BEGIN:VEVENT
UID:1234
RECURRENCE-ID=20160903T120000Z
DTSTART:20160903T120000Z
DURATION:PT1H
SUMMARY:Override second instance
END:VEVENT
END:VCALENDAR
```

A similar result could have been achieved by using a path targeting the
"VCALENDAR" component, and the entire "overridden" component supplied as
the data.  However, the implicit override behaviour allows for a more
compact representation of this type of change.

There is no equivalent behavior when it comes to removing "overridden"
components from an iCalendar object to cancel the instance.  In that
case, two "PATCH" components are required: one to delete the
"overridden" component, and one to create an "EXDATE" property value in
the master component to cover the cancellation.  So, continuing from the
example data immediately above, the following patch commands would
cancel the instance that was previously overridden:

{align=left}
```
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR
PATCH-DELETE:/VEVENT[UID=1234][RID=20160903T120000Z]
END:PATCH
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234][RID=M]
EXDATE;PATCH-ACTION=CREATE:20160903T120000Z
END:PATCH
END:VPATCH
```

which results in the following updated iCalendar component:

{align=left}
```
BEGIN:VCALENDAR
PRODID:test
VERSION:2.0
BEGIN:VEVENT
UID:1234
DTSTART:20160902T120000Z
DURATION:PT1H
SUMMARY:Master component
RRULE:FREQ=DAILY
EXDATE:20160903T120000Z
END:VEVENT
END:VCALENDAR
```

## Folded lines

iCalendar data can contain "folded" lines (as described in Section 3.1
of [@RFC5545]).  The patch operations described in this specification
are a "semantic" rather than "syntactic" update to the data. i.e., they
apply to the underlying object model as opposed to the "raw" iCalendar
text data.  As such, folded lines in the iCalendar data targeted by the
patch commands are not significant.

Any iCalendar data supplied as data items in a patch command MAY contain
folded lines.

## Encoding

Text values in iCalendar use a backslash escape mechanism for certain
characters (as described in Section 3.3.11 [@RFC5545]).  Patch
operations apply to the escaped form of the iCalendar data.  For
example, to delete a "DESCRIPTION" property that contains an encoded
line feed character:

{align=left}
```
DESCRIPTION:Line one\nLine two
```

the following PATCH-DELETE property would be used:

{align=left}
```
PATCH-DELETE:#DESCRIPTION[=Line one\nLine two]
```

Similarly, to update the "DESCRIPTION" property, the following patch
command could be used:

{align=left}
```
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT
DESCRIPTION:Line one\nLine two\nLine three
END:PATCH
END:VPATCH
```

## Generation

This specification does not define how patch data is generated, as that
is likely to be highly dependent on the nature of the implementation.
However, it is recommended that patch generators use sets of commands
that keep the overall patch data as compact as possible, since one of
the goals of this specification is to reduce the size of data needed to
do updates.  One example is the choice of whether to update an entire
property, or just property parameters, when changes are made to just
property parameters.  In some cases, the data in a property parameter
can be large, so repeating that in a full property update may result in
larger data than simple using the "PATCH-PARAMETER" property to do an
update.  On the other hand, if lots of property parameters are being
updated or removed, it can be more efficient to update the entire
property rather than using lots of "PATCH-PARAMETER" and "PATCH-DELETE"
properties.

