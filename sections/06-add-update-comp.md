# Adding or Updating Components

Any iCalendar component defined in the "PATCH" component (referred to
below as the "action component") is treated as either an addition to
the target component, or as an update of an existing component in the
target component.  The following rules are used to process such
components:

* If the action component contains a "UID" property and a
  "RECURRENCE-ID" property, then any components with the same
  values for both their "UID" and "RECURRENCE-ID" properties, that
  are immediate sub-components of the target component, are removed
  from the target component, and the action component is added to
  the target component.

* If the action component contains a "UID" property and does not
  contain a "RECURRENCE-ID" property, then any components with the
  same value for their "UID" property, and containing no
  "RECURRENCE-ID" property, that are immediate sub-components of
  the target component, are removed from the target component, and
  the action component is added to the target component.

* If the action component does not contain a "UID" property, then
  all components with the same name that do not contain a "UID"
  property, that are immediate sub-components of the target
  component, are removed from the target component, and the action
  component is added to the target component.
