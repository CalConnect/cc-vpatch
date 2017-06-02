# iCalendar Extensions

This specification adds a new "VPATCH" calendar component to
iCalendar.  The "VPATCH" component is itself a container for a new
"PATCH" sub-component.

## VPATCH Component

Component Name:  VPATCH

Purpose:
: Provide a grouping of "PATCH" sub-components that describe
  the patch actions to be performed.

Description:
:  This component serves as a container for a series of
  "PATCH" sub-components, each specifying patch actions to be
  performed on a certain target element in an iCalendar object.

Format Definition:
:
  A "VPATCH" calendar component is defined by the following notation:

  {align=left}
  ```abnf
  vpatchc     = "BEGIN" ":" "VPATCH" CRLF
                 vpatchprop action
               "END" ":" "VPATCH" CRLF

  vpatchprop  = *(
                  ;
                  ; The following are REQUIRED,
                  ; but MUST NOT occur more than once.
                  ;
                  dtstamp / uid /
                  ;
                  ;
                  ; The following are OPTIONAL,
                  ; but MUST NOT occur more than once.
                  ;
                  patch-version / patch-order /
                  ;
                  ; The following are OPTIONAL,
                  ; and MAY occur more than once.
                  ;
                  other-prop
                  ;
                )

  other-prop  = ( iana-prop / x-prop )

  action      = *( patchc / iana-comp / x-comp )
  ```

### PATCH Component

Component Name:
:  PATCH

Purpose
:
  Provide a set of components, properties, and property parameters to be
  added to, deleted from, or updated in the iCalendar object.

Description:
:
  This component provides a grouping of patch actions to be performed
  within the scope of a set of components.  If the "PATCH-TARGET"
  property matches one or more iCalendar components, then the target
  components are patched using the remaining properties and components.
  If there is no iCalendar component that matches the "PATCH-TARGET"
  property in the iCalendar object, the "PATCH" action MUST succeed
  without any changes being applied to the iCalendar object being
  patched.

Format Definition:
:
  A "PATCH" calendar component is defined by the following notation:

  {align=left}
  ```abnf
  patchc     = "BEGIN" ":" "PATCH" CRLF
                    patchprop subcomp
                  "END" ":" "PATCH" CRLF

   patchprop  = *(
                    ;
                    ; The following is REQUIRED,
                    ; but MUST NOT occur more than once.
                    ;
                    patchtarget /
                    ;
                    ; The following are OPTIONAL,
                    ; and MAY occur more than once.
                    ;
                    patchdelete / patchparam / other-prop
                    ;
                  )

   subcomp    = *(
                   ;
                   ; The following are OPTIONAL,
                   ; and MAY occur more than once.
                   ;
                   eventc / todoc / journalc / freebusyc /
                   timezonec / alarmc / standard / daylight /
                   availabilityc / availablec /
                   iana-comp / x-comp
                   ;
                )
  ```


## VPATCH Properties

The "VPATCH" properties are attributes that apply to the "VPATCH"
component, as a whole.  These properties do not appear within "VPATCH"
sub-components.  They SHOULD be specified after the "BEGIN:VPATCH"
delimiter string and prior to any sub-component.


### PATCH-VERSION Property

Property Name:  PATCH-VERSION

Purpose:  This property specifies the identifier corresponding to the
  highest version number of the "VPATCH" specification that is
  required in order to interpret the "VPATCH" component.

Value Type:  INTEGER

Property Parameters:  IANA and nonstandard property parameters can be
  specified on this property.

Conformance:  This property can be specified once in an "VPATCH"
  component.  The default value is "1".  This property MUST be
  specified if its value is greater than "1".  Otherwise, this
  property is OPTIONAL.

Description:  A value of "1" corresponds to this memo.  See Section 3
  for a description of how this property is used.

Format Definition:  This property is defined by the following
  notation:

{align=left}
```abnf
patch-version  = "PATCH-VERSION pverparam ":" pvervalue CRLF

pverparam      = *(";" other-param)

pvervalue      = "1" / pmaxver
              ; "1" signifies compliance with this memo

pmaxver        = <A IANA-registered VPATCH version>
              ; Maximum VPATCH version needed to process the VPATCH
              ; component.
```

Example:  The following is an example of this property:

{align=left}
```
PATCH-VERSION:1
```

### PATCH-ORDER Property

Property Name:  PATCH-ORDER

Purpose:  This property specifies the ordering of the associated
  "VPATCH" component.

Value Type:  INTEGER

Property Parameters:  IANA and nonstandard property parameters can be
  specified on this property.

Conformance:  This property can be specified once in a "VPATCH"
  component.

Description:  This property is OPTIONAL and is used to indicate the
  relative ordering of the associated "VPATCH" component amongst its
  siblings.  See Section 3 for a description of how this property is
  used.

Format Definition:  This property is defined by the following
  notation:

{align=left}
```abnf
patch-order  = "PATCH-ORDER porderparam ":" integer CRLF

porderparam  = *(";" other-param)
```

Example:  The following is an example of this property:

{align=left}
```
PATCH-ORDER:1
```

## PATCH Component Properties

The following properties can appear within PATCH components.

### PATCH-TARGET Property

Property Name:  PATCH-TARGET

Purpose:  This property specifies a path targeting one or more
  components within an iCalendar object.

Value Type:  TEXT

Property Parameters:  IANA and nonstandard property parameters can be
  specified on this property.

Conformance:  This property MUST be specified within any "PATCH" sub-
  component.

Description:  This property is used to match iCalendar components
  that the patch operations will be applied to.  The path value is
  always an absolute path, and interpreted as described in
  Section 5.

Format Definition:  This property is defined by the following
  notation:

{align=left}
```abnf
patchtarget   = "PATCH-TARGET ptargetparam ":" ptargetpath CRLF

ptargetparam  = *(";" other-param)

ptargetpath   = abs-comp-path / comp-path
               ; This specification only defines how abs-comp-path
               ; is used. Use of the comp-path element will be
               ; defined by other specifications wishing to make use
               ; of "relative" patches.
```

Example:  The following is an example of this property:

{align=left}
```
PATCH-TARGET:/VCALENDAR/VEVENT[UID=1234]
```

### PATCH-DELETE Property

Property Name:  PATCH-DELETE

Purpose:  This property specifies a path (relative to "PATCH-TARGET")
  targeting one or more components, properties, or parameters to be
  removed from the target components identified by "PATCH-TARGET".

Value Type:  TEXT

Property Parameters:  IANA and nonstandard property parameters can be
  specified on this property.

Conformance:  This property can be specified within a "PATCH" sub-
  component.

Description:  This property is used to match iCalendar elements that
  will be deleted.  The path value is always a relative path for
  only immediate components and properties within the target
  component, and interpreted as described in Section 8.

Format Definition:  This property is defined by the following
  notation:

{align=left}
```abnf
patchdelete   = "PATCH-DELETE pdeleteparam ":" pdeletepath CRLF

pdeleteparam  = *(";" other-param)

pdeletepath   = rel-one-path
             ; PATCH-DELETE path is relative to PATCH-TARGET path
```

Example:  The following are examples of this property:

PATCH-DELETE:/VEVENT[UID=1234]
PATCH-DELETE:#ATTENDEE[=mailto:cyrus@example.com]

### PATCH-PARAMETER Property

Property Name:  PATCH-PARAMETER

Purpose:  This property specifies a set of parameters to be set on
  the target property.

Value Type:  TEXT

Property Parameters:  IANA and nonstandard property parameters can be
  specified on this property.

Conformance:  This property can be specified within a "PATCH" sub-
  component.

Description:  This property specifies parameters to be set on the
  target property.  The path value is always a relative path to a
  property within the target component, and interpreted as described
  in Section 9.

Format Definition:  This property is defined by the following
  notation:

{align=left}
```abnf
patchparam   = "PATCH-PARAMETER pparamparam ":" pparampath CRLF

pparamparam  = *(";" other-param)

pparampath   = prop-param-path
```

Example:  The following are examples of this property:

{align=left}
```
PATCH-PARAMETER;PARTSTAT=NEEDS-ACTION:
 #ATTENDEE[=mailto:cyrus@example.com]
PATCH-PARAMETER;PARTSTAT=NEEDS-ACTION:#ATTENDEE[@CN=Cyrus Daboo]
PATCH-PARAMETER;MEMBER=mailto:newgroup@example.com:#ATTENDEE;MEMBER
```

##  PATCH-ACTION Property Parameter

Parameter Name:  PATCH-ACTION

Purpose:  To specify whether the property should be added or
  replaced.

Format Definition:  This parameter is defined by the following
  notation:

{align=left}
```abnf
pactionparam    = "PATCH-ACTION" "="
                   pactioncreate /
                   pactionbyname /
                   pactionbyvalue /
                   pactionbyparam /
                   iana-token /     ; IANA registered value
                   x-name           ; Experimental value

pactioncreate   = "CREATE"
               ; Always add property to target component.

pactionbyname   = "BYNAME"
               ; Always remove properties with the same name
               ; from the target component,
               ; then add this property to the target component.
               ; This value is the default and MAY be omitted.

pactionbyvalue  = "BYVALUE"
               ; Always remove properties with the same name
               ; and value from the target component,
               ; then add this property to the target component.

pactionbyparam  = DQUOTE "BYPARAM" param-match   DQUOTE
               ; Always remove properties with the same name
               ; and parameter name/value from the target
               ; component, then add this property to the target
               ; component.
```

Description:  This parameter can be specified on properties contained
  in a "PATCH" component and MUST NOT be specified on properties
  outside of a "PATCH" component.  This parameter specifies whether
  the property should be added to the target component or should
  replace existing properties in the target component.  In the
  latter case, the parameter also specifies how to match existing
  properties.  The processing of this property parameter is
  described in Section 7.

Examples:  The following are examples of this property parameter:

{align=left}
```
ATTENDEE;PATCH-ACTION=BYVALUE;PARTSTAT=NEEDS-ACTION:
mailto:cyrus@example.com
DESCRIPTION;PATCH-ACTION="BYPARAM@LANGUAGE=en_GB";LANGUAGE=en_US:
Meeting to discuss VPATCH
```



