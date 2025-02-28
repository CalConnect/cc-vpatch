# Adding or Updating Property Parameters

The "PATCH-PARAMETER" property (defined in Section 10.3.3) is used to
indicate addition or update of property parameters and property
parameter values to properties contained in the components identified by
the "PATCH-TARGET" property in the same "PATCH" component as the
"PATCH-PARAMETER" property.  As such, the value of the "PATCH-
PARAMETER" property is always a relative path (see Section 5) that
refers to a property that is an immediate "child" of the target
component.

The following operations are supported:

Add or update property parameters
:
  The "PATCH-PARAMETER" path value targets a property only.  Any
  property parameters defined on the "PATCH-PARAMETER" replace the
  matching parameters on the target property, or are added to the target
  property if no matching parameters exist.

Add a property parameter value
:
  The "PATCH-PARAMETER" path value targets a multi-valued parameter
  only.  The values in any property parameters defined on the
  "PATCH-PARAMETER" property are added to the corresponding property
  parameters of the target properties.  If no corresponding property
  parameter is defined on the target properties, then property
  parameters are created with the corresponding values.


