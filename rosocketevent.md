roSocketEvent
=============

An roStreamSocket or roDataGramSocket object sends the roSocketEvent to indicate a change in the status of the socket. The socket must enable specific event notifications via the notify methods of ifSocketAsync.

Supported methods
-----------------

### GetSocketID() as Integer

Returns the ID of the socket this event is for. The ID of a socket can be obtained from ifSocketAsync.GetID(). Use [ifSocketStatus](/docs/references/brightscript/interfaces/ifsocketstatus.md "ifSocketStatus") or [ifSocketConnectionStatus](/docs/references/brightscript/interfaces/ifsocketconnectionstatus.md "ifSocketConnectionStatus") on the indicated socket to query the new status for the socket.