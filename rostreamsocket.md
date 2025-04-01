roStreamSocket
==============

The roStreamSocket component enables BrightScript apps to accept and connect to TCP streams as well as send and receive data with them. The interface is modeled on and works much like standard Berkeley sockets.

This object is created without any arguments:

`CreateObject("roStreamSocket")`

**Example: Open TCP Connection to Server**

    sendAddress = CreateObject("roSocketAddress")
    sendAddress.SetAddress("www.google.com:80")
    socket = CreateObject("roStreamSocket")
    socket.setSendToAddress(sendAddress)
    If socket.Connect()
        Print "Connected Successfully"
    End If
    

**Example: Echo Server**

    function main()
        messagePort = CreateObject("roMessagePort")
        connections = {}
        buffer = CreateObject("roByteArray")
        buffer[512] = 0
        tcpListen = CreateObject("roStreamSocket")
        tcpListen.setMessagePort(messagePort)
        addr = CreateObject("roSocketAddress")
        addr.setPort(54321)
        tcpListen.setAddress(addr)
        tcpListen.notifyReadable(true)
        tcpListen.listen(4)
        if not tcpListen.eOK()
            print "Error creating listen socket"
            stop
        end if
        while True
            event = wait(0, messagePort)
            if type(event) = "roSocketEvent"
                changedID = event.getSocketID()
                if changedID = tcpListen.getID() and tcpListen.isReadable()
                    ' New
                    newConnection = tcpListen.accept()
                    if newConnection = Invalid
                        print "accept failed"
                    else
                        print "accepted new connection " newConnection.getID()
                        newConnection.notifyReadable(true)
                        newConnection.setMessagePort(messagePort)
                        connections[Stri(newConnection.getID())] = newConnection
                    end if
                else
                    ' Activity on an open connection
                    connection = connections[Stri(changedID)]
                    closed = False
                    if connection.isReadable()
                        received = connection.receive(buffer, 0, 512)
                        print "received is " received
                        if received > 0
                            print "Echo input: '"; buffer.ToAsciiString(); "'"
                            ' If we are unable to send, just drop data for now.
                            ' You could use notifywritable and buffer data, but that is
                            ' omitted for clarity.
                            connection.send(buffer, 0, received)
                        else if received=0 ' client closed
                            closed = True
                        end if
                    end if
                    if closed or not connection.eOK()
                        print "closing connection " changedID
                        connection.close()
                        connections.delete(Stri(changedID))
                    end if
                end if
            end if
        end while
    
        print "Main loop exited"
        tcpListen.close()
        for each id in connections
            connections[id].close()
        end for
    End Function
    

Supported interfaces
--------------------

*   [ifSocketConnection](/docs/references/brightscript/interfaces/ifsocketconnection.md "ifSocketConnection")
*   [ifSocket](/docs/references/brightscript/interfaces/ifsocket.md "ifSocket")
*   [ifSocketAsync](/docs/references/brightscript/interfaces/ifsocketasync.md "ifSocketAsync")
*   [ifSocketStatus](/docs/references/brightscript/interfaces/ifsocketstatus.md "ifSocketStatus")
*   [ifSocketConnectionStatus](/docs/references/brightscript/interfaces/ifsocketconnectionstatus.md "ifSocketConnectionStatus")
*   [ifSocketConnectionOption](/docs/references/brightscript/interfaces/ifsocketconnectionoption.md "ifSocketConnectionOption")
*   [ifSocketOption](/docs/references/brightscript/interfaces/ifsocketoption.md)

Supported events
----------------

*   [roSocketEvent](/docs/references/brightscript/events/rosocketevent.md "roSocketEvent")