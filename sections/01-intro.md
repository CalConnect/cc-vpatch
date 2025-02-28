#  Introduction

The iCalendar [@RFC5545] data format is in widespread use to represent
calendar data. iCalendar data can grow large (e.g., a family calendar
containing events over a period of several years).  Updating large
resources over the network currently requires the entire data to be
sent - even if the change itself is minor.

This specification defines a new iCalendar component that can be used
to "patch" (incrementally update) iCalendar data in an efficient
manner.  When combined with the HTTP PATCH method [@RFC5789], it can
be used to update calendar object resources on a CalDAV [@RFC4791]
server, or any resource on an HTTP server that contains iCalendar
data.

