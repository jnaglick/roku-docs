roUrlEvent
==========

The roUrlTransfer component sends the roUrlEvent with the following methods:

Supported methods
-----------------

### GetInt() as Integer

Returns the type of event, which may be one of the following values:

| Event Type | Description |
| --- | --- |
| 1   | transfer complete |
| 2   | transfer started. Headers are available for suitable protocols. (Not currently implemented) |

### GetResponseCode() as Integer

Returns the protocol response code associated with this event. For a successful HTTP request this will be the HTTP status code 200. For unexpected errors the return value is negative. There are lots of possible negative errors from the CURL library but it's often best just to look at the text version via GetFailureReason().

The following table lists some of the potential errors (not all of them can be generated):

| Status | Name | Description |
| --- | --- | --- |
| \-1 | CURLE\_UNSUPPORTED\_PROTOCOL |     |
| \-2 | CURLE\_FAILED\_INIT |     |
| \-3 | CURLE\_URL\_MALFORMAT |     |
| \-4 | CURLE\_NOT\_BUILT\_IN |     |
| \-5 | CURLE\_COULDNT\_RESOLVE\_PROXY |     |
| \-6 | CURLE\_COULDNT\_RESOLVE\_HOST |     |
| \-7 | CURLE\_COULDNT\_CONNECT |     |
| \-8 | CURLE\_FTP\_WEIRD\_SERVER\_REPLY |     |
| \-9 | CURLE\_REMOTE\_ACCESS\_DENIED | A service was denied by the server due to lack of access - when login fails this is not returned |
| \-11 | CURLE\_FTP\_WEIRD\_PASS\_REPLY |     |
| \-13 | CURLE\_FTP\_WEIRD\_PASV\_REPLY |     |
| \-14 | CURLE\_FTP\_WEIRD\_227\_FORMAT |     |
| \-15 | CURLE\_FTP\_CANT\_GET\_HOST |     |
| \-16 | CURLE\_HTTP2 |     |
| \-17 | CURLE\_FTP\_COULDNT\_SET\_TYPE |     |
| \-18 | CURLE\_PARTIAL\_FILE |     |
| \-19 | CURLE\_FTP\_COULDNT\_RETR\_FILE |     |
| \-21 | CURLE\_QUOTE\_ERROR | Quote command failure |
| \-22 | CURLE\_HTTP\_RETURNED\_ERROR |     |
| \-23 | CURLE\_WRITE\_ERROR |     |
| \-25 | CURLE\_UPLOAD\_FAILED | Failed upload "command" |
| \-26 | CURLE\_READ\_ERROR | Could open/read from file |
| \-27 | CURLE\_OUT\_OF\_MEMORY |     |
| \-28 | CURLE\_OPERATION\_TIMEDOUT | The timeout time was reached |
| \-30 | CURLE\_FTP\_PORT\_FAILED | FTP PORT operation failed |
| \-31 | CURLE\_FTP\_COULDNT\_USE\_REST | The REST command failed |
| \-33 | CURLE\_RANGE\_ERROR | RANGE "command" didn't work |
| \-34 | CURLE\_HTTP\_POST\_ERROR |     |
| \-35 | CURLE\_SSL\_CONNECT\_ERROR | Wrong when connecting with SSL |
| \-36 | CURLE\_BAD\_DOWNLOAD\_RESUME | Couldn't resume download |
| \-37 | CURLE\_FILE\_COULDNT\_READ\_FILE |     |
| \-38 | CURLE\_LDAP\_CANNOT\_BIND |     |
| \-39 | CURLE\_LDAP\_SEARCH\_FAILED |     |
| \-41 | CURLE\_FUNCTION\_NOT\_FOUND |     |
| \-42 | CURLE\_ABORTED\_BY\_CALLBACK |     |
| \-43 | CURLE\_BAD\_FUNCTION\_ARGUMENT |     |
| \-45 | CURLE\_INTERFACE\_FAILED | CURLOPT\_INTERFACE failed |
| \-47 | CURLE\_TOO\_MANY\_REDIRECTS | Catch endless re-direct loops |
| \-48 | CURLE\_UNKNOWN\_TELNET\_OPTION | User specified an unknown option |
| \-49 | CURLE\_TELNET\_OPTION\_SYNTAX | Malformed telnet option |
| \-51 | CURLE\_PEER\_FAILED\_VERIFICATION | Peer's certificate or fingerprint wasn't verified fine |
| \-52 | CURLE\_GOT\_NOTHING | When this is a specific error |
| \-53 | CURLE\_SSL\_ENGINE\_NOTFOUND | SSL crypto engine not found |
| \-54 | CURLE\_SSL\_ENGINE\_SETFAILED | Can not set SSL crypto engine as default |
| \-55 | CURLE\_SEND\_ERROR | Failed sending network data |
| \-56 | CURLE\_RECV\_ERROR | Failure in receiving network data |
| \-58 | CURLE\_SSL\_CERTPROBLEM | Problem with the local certificate |
| \-59 | CURLE\_SSL\_CIPHER | Couldn't use specified cipher |
| \-60 | CURLE\_SSL\_CACERT | Problem with the CA cert (path?) |
| \-61 | CURLE\_BAD\_CONTENT\_ENCODING | Unrecognized transfer encoding |
| \-62 | CURLE\_LDAP\_INVALID\_URL | Invalid LDAP URL |
| \-63 | CURLE\_FILESIZE\_EXCEEDED | Maximum file size exceeded |
| \-64 | CURLE\_USE\_SSL\_FAILED | Requested FTP SSL level failed |
| \-65 | CURLE\_SEND\_FAIL\_REWIND | Sending the data requires a rewind that failed |
| \-66 | CURLE\_SSL\_ENGINE\_INITFAILED | Failed to initialize ENGINE |
| \-67 | CURLE\_LOGIN\_DENIED | User, password or similar was not accepted and we failed to login |
| \-68 | CURLE\_TFTP\_NOTFOUND | File not found on server |
| \-69 | CURLE\_TFTP\_PERM | Permission problem on server |
| \-70 | CURLE\_REMOTE\_DISK\_FULL | Out of disk space on server |
| \-71 | CURLE\_TFTP\_ILLEGAL | Illegal TFTP operation |
| \-72 | CURLE\_TFTP\_UNKNOWNID | Unknown transfer ID |
| \-73 | CURLE\_REMOTE\_FILE\_EXISTS | File already exists |
| \-74 | CURLE\_TFTP\_NOSUCHUSER | No such user |
| \-75 | CURLE\_CONV\_FAILED | Conversion failed |
| \-76 | CURLE\_CONV\_REQD | Caller must register conversion callbacks using curl\_easy\_setopt options CURLOPT\_CONV\_FROM\_NETWORK\_FUNCTION, CURLOPT\_CONV\_TO\_NETWORK\_FUNCTION, and CURLOPT\_CONV\_FROM\_UTF8\_FUNCTION |
| \-77 | CURLE\_SSL\_CACERT\_BADFILE | Could not load CACERT file, missing or wrong format |
| \-78 | CURLE\_REMOTE\_FILE\_NOT\_FOUND | Remote file not found |
| \-79 | CURLE\_SSH | Error from the SSH layer, somewhat generic so the error message will be of interest when this has happened |
| \-80 | CURLE\_SSL\_SHUTDOWN\_FAILED | Failed to shut down the SSL connection |

### GetFailureReason() as String

Returns a description of the failure that occurred.

### GetString() as String

For transfer complete AsyncGetToString, AsyncPostFromString and AsnycPostFromFile requests this will be the actual response body from the server. This method returns the string associated with the event.

### GetSourceIdentity() as Integer

Returns a magic number that can be matched with the value returned by the [roUrlTransfer.GetIdentity()](/docs/references/brightscript/interfaces/ifurltransfer.md#getidentity-as-integer) method to determine the source of the roUrlTransfer event.

### GetResponseHeaders() as Object

Returns an [roAssociativeArray](/docs/references/brightscript/components/roassociativearray.md "roAssociativeArray") containing all the headers returned by the server for appropriate protocols (such as HTTP). Headers are only returned when the status code is greater than or equal to 200 and less than 300

### GetTargetIpAddress() as String

Returns the IP address of the destination.

### GetResponseHeadersArray() as Object

This method returns an [roArray](/docs/references/brightscript/components/roarray.md "roArray") of [roAssociativeArrays](/docs/references/brightscript/components/roassociativearray.md "roAssociativeArray"), where each associative array contains a single header name/value pair. Use this function if you need access to duplicate headers, since GetResponseHeaders() returns only the last name/value pair for a given name. All headers are returned regardless of the status code