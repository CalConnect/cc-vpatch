#  Security Considerations

Patch processing engines MUST ensure that the result of applying a
patch is a valid iCalendar object in the context of the application
using the calendar data.  At the very least, the resulting iCalendar
object MUST comply with the requirements of [@RFC5545].

Security considerations described in [@RFC5545], [@RFC5789], and
[@RFC4791] MUST be adhered to.

