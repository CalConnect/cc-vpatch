# Use with CalDAV and HTTP

The CalDAV [@RFC4791] calendar access protocol allows clients and
servers to exchange iCalendar data. iCalendar data is typically stored
in calendar object resources on a CalDAV server.  A CalDAV client
typically updates the calendar object resource data via an HTTP PUT
request, which requires sending the entire iCalendar object in the HTTP
request body.

A server can also support the HTTP PATCH method [@RFC5789] which allows
a patch document to be specified in the request body, and for that patch
document to be applied to the resource targeted by the HTTP request.  In
this case, the server would advertise the "text/ calendar" media type in
an "Accept-Patch" header field as described in Section 3.1 of
[@RFC5789].  Note that the requirements for parameters on this media
type when advertised in "Accept-Patch" are as follows:

*  MUST include a "component" parameter with a value of "VPATCH"

*  MUST include an "optinfo" parameter with a value of "PATCH-
  VERSION:<n>", where <n> is the maximum patch version supported by
  the server

*  MAY include a "charset" parameter as appropriate

For example:

{align=left}
```
Accept-Patch: text/calendar; component=VPATCH;
optinfo="PATCH-VERSION:1"; charset=utf-8
```

The PATCH-TARGET property defined by this specification does not
allow targeting the entire iCalendar object, and hence an HTTP PATCH
request cannot be used to create a new resource (a normal HTTP PUT
request is used instead).

