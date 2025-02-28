# Deleting Components, Properties, or Property Parameters

The "PATCH-DELETE" property (defined in Section 10.3.2) is used to
indicate deletion of iCalendar elements from the component identified by
the "PATCH-TARGET" property in the same "PATCH" component as the
"PATCH-DELETE" property.  As such, the value of the "PATCH-DELETE"
property is always a relative path (see Section 5) that refers to an
element that is an immediate "child" of the target component.

The following operations are supported:

Delete components
:
  The "PATCH-DELETE" path value targets components only.  The matching
  components are removed from the "parent" target component.

Properties
:
  The "PATCH-DELETE" path value targets properties only.  The matching
  properties are removed from the "parent" target component.

Property parameters
:
  The "PATCH-DELETE" path value targets property parameters on specific
  properties only.  The matching property parameters are removed from
  the corresponding property.

Property values
:
  The "PATCH-DELETE" path value targets a property value on specific
  multi-valued properties only.  The matching property value is removed
  from the the corresponding property.  If that results in a property
  with no value, that property is also removed from its "parent" target
  component.

Property parameter values
:
  The "PATCH-DELETE" path value targets a property parameter value on a
  specific multi-valued property parameter on specific properties only.
  The matching property parameter value is removed from the
  corresponding property parameter.  If that results in a property
  parameter with no value, that property parameter is also removed from
  from the corresponding property.


