# VPATCH Examples

Examples of single command patch documents for common iCalendar data
operations.

##  Add a new component

Creates a new "VEVENT" component.

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR
BEGIN:VEVENT
UID:1234
DTSTART:20160902T103000Z
DURATION:PT1H
SUMMARY:Test event
END:VEVENT
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Add a new VALARM component

Creates a new "VALARM" component in the "VEVENT" component with the
"UID" property value "1234".

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234]
BEGIN:VALARM
UID:4567
ACTION:DISPLAY
TRIGGER:-PT30M
DESCRIPTION:Time to leave
END:VALARM
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Replace a component

Replace the "VEVENT" component with the "UID" property value "1234"
with a new component.

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234]
BEGIN:VEVENT
UID:1234
DTSTART:20160903T123000Z
DURATION:PT2H
SUMMARY:Changed event
END:VEVENT
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Remove a component

Remove the "VEVENT" component with the "UID" property value "1234".

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR
PATCH-DELETE:/VEVENT[UID=1234]
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Add properties to a component

Add "STATUS" and "COMPLETED" properties to the "VTODO" component with
the "UID" property value "4321".

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VTODO[UID=4321]
STATUS;PATCH-ACTION=CREATE:COMPLETED
COMPLETED;PATCH-ACTION=CREATE:20160902T224515Z
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Update properties in a component

Update the "SUMMARY" and "LOCATION" properties in the "VEVENT"
component with the "UID" property value "1234".

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234]
SUMMARY:Title was changed
LOCATION:New place
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Update a targeted property in a component

Update the "ATTENDEE" property with value "mailto:cyrus@example.com"
in the "VEVENT" component with the "UID" property value "1234".

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234]
ATTENDEE;PATCH-ACTION=BYVALUE;PARTSTAT=ACCEPTED:
mailto:cyrus@example.com
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Remove a property from a component

Remove the "URL" property from the "VEVENT" component with the "UID"
property value "1234".

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234]
PATCH-DELETE:#URL
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Remove a property with a specific value from a component

Remove the "ATTENDEE" property with the value
"mailto:cyrus@example.com" in the "VEVENT" component with the "UID"
property value "1234".

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234]
PATCH-DELETE:#ATTENDEE[=mailto:cyrus@example.com]
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Change a parameter on a property with a specific value from a
   component

Change or add the "PARTSTAT" parameter on the "ATTENDEE" property
with the value "mailto:cyrus@example.com" in the "VEVENT" component
with the "UID" property value "1234".

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234]
PATCH-PARAMETER;PARTSTAT=ACCEPTED:
#ATTENDEE[=mailto:cyrus@example.com]
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Remove a parameter on a property with a specific value from a component

Remove the "PARTSTAT" parameter from the "ATTENDEE" property with the
value "mailto:cyrus@example.com" in the "VEVENT" component with the
"UID" property value "1234".

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234]
PATCH-DELETE:#ATTENDEE[=mailto:cyrus@example.com];PARTSTAT
END:PATCH
END:VPATCH
END:VCALENDAR
```

# Remove a value from a multi-valued parameter on a property with a
  specific value from a component

Remove the "mailto:calext@example.com" value from the "MEMBER"
parameter on the "ATTENDEE" property with the value
"mailto:cyrus@example.com" in the "VEVENT" component with the "UID"
property value "1234".

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234]
PATCH-DELETE:#ATTENDEE[=mailto:cyrus@example.com]
;MEMBER=mailto:calext@example.com
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Remove a value from a multi-valued property from a component

Remove the value "20160903T120000Z" from the "EXDATE" property in the
"VEVENT" component with the "UID" property value "1234".

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234]
PATCH-DELETE:#EXDATE=20160903T120000Z
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Attendee updating their participation status

When an attendee updates their participation status in an event, they
will typically: update the "PARTSTAT" parameter on their "ATTENDEE"
property, remove the "RSVP" parameter on their "ATTENDEE" property,
update the "TRANSP" property in the "VEVENT" component.  This set of
changes is shown below in a single "PATCH" component, with the
attendee having the calendar user address "mailto:cyrus@example.com".
The patch targets all "VEVENT" components in the iCalendar object
being changed.

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT
PATCH-DELETE:#ATTENDEE[=mailto:cyrus@example.com];RSVP
PATCH-PARAMETER;PARTSTAT=ACCEPTED:
#ATTENDEE[=mailto:cyrus@example.com]
TRANSP:OPAQUE
END:PATCH
END:VPATCH
END:VCALENDAR
```

##  Recurring event adding one override

A daily recurring "VEVENT" component with the "SUMMARY" property
being overridden for the second instance.

iCalendar object before the patch:

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VEVENT
UID:1234
DTSTART:20160905
DURATION:PT1H
SUMMARY:Test event
RRULE:FREQ=DAILY
END:VEVENT
END:VCALENDAR
```

Patch:

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[RID=20160906]
SUMMARY:Test event - modified
END:PATCH
END:VPATCH
END:VCALENDAR
```

iCalendar object after the patch:

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VEVENT
UID:1234
DTSTART:20160905
DURATION:PT1H
SUMMARY:Test event
RRULE:FREQ=DAILY
END:VEVENT
BEGIN:VEVENT
UID:1234
RECURRENCE-ID:20160906
DTSTART:20160905
DURATION:PT1H
SUMMARY:Test event - modified
END:VEVENT
END:VCALENDAR
```

##  Removal of an overridden instance

A daily recurring "VEVENT" component has one existing instance
override removed with an "EXDATE" added for it.

iCalendar object before the patch:

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VEVENT
UID:1234
DTSTART:20160905
DURATION:PT1H
SUMMARY:Test event
RRULE:FREQ=DAILY
END:VEVENT
BEGIN:VEVENT
UID:1234
RECURRENCE-ID:20160906
DTSTART:20160905
DURATION:PT1H
SUMMARY:Test event - modified
END:VEVENT
END:VCALENDAR
```

Patch:

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VPATCH
UID:abcd
DTSTAMP:20160901T000000Z
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR
PATCH-DELETE:/VEVENT[RID=20160906]
END:PATCH
BEGIN:PATCH
PATCH-TARGET:/VCALENDAR/VEVENT[RID=M]
EXDATE;PATCH-ACTION=CREATE:20160906
END:PATCH
END:VPATCH
END:VCALENDAR
```


iCalendar object after the patch:

{align=left}
```
BEGIN:VCALENDAR
PRODID:Example
VERSION:2.0
BEGIN:VEVENT
UID:1234
DTSTART:20160905
DURATION:PT1H
SUMMARY:Test event
RRULE:FREQ=DAILY
EXDATE:20160906
END:VEVENT
END:VCALENDAR
```


