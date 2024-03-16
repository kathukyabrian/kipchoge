# HTTP(HYPER TEXT TRANSFER PROTOCOL) V1.1

- [reference doc](https://datatracker.ietf.org/doc/html/rfc2616#section-5)


## Purpose
- application level protocol
- history
    - http 0.9
        - raw data transfer over the internet
    - http 1.0
        - improved the protocol by allowing messages to be in the format of MIME-like messages
        - challenge
            - it does not sufficiently take into consideration 
                - effects of hierarchical proxies 
                - caching
                - the need of persistent connections

## Terminology
1. connection - transport layer virtual circuit established between 2 programs for the purpose of communication.
1. message - basic unit of HTTP communication - consists of a sequence of octets matchin a certain syntax and transmitted via the *connection*
1. request - HTTP request message
1. response - HTTP response message
1. resource - network data object or service that can be identified by a *URI*
1. entity - information transferred as the payload of a *request* or *response*. consists of metainformation in the form of entity-header fields and content in the form of entity-body
1. representation - an entity included with a response that is subject to *content negotiation*
1. content negotiation - mechanism of selecting the appropriate representation when serving a request 
1. client - program that establishes a connection for the purpose of sending requests
1. user agent - the client which initiates a request eg browsers
1. server - an application program that accepts connections in order to service requests by sending back responses. 
1. origin server - the server on which a certain resource resides or is to be created.
1. proxy - an intermediary program which acts as both a server and a client for the purpose of making requests on behalf of other clients.
    - transparent proxy - does not change the request/response beyond what is required for proxy authentication and identiification
    - non-transparent proxy - modifies the request/response in order to give some added service to the user agent e.g annotation services, media type transformation, protocol reduction or anonymity filtering
1. gateway - a server which acts as an intermediary for some other server. unlike a proxy, the gateway server receives the request as if it were the origin server for the requested resource
1. tunnel - intermediary program which is acting as a blind relay between 2 connections.
1. cache - program's local store of response messages and the subsystem that controls its message storage, retrieval and deletion. any client or server may include a cache though a cache cannot be used by a server that is acting as a tunnel.
1. cacheable - a response is cacheable if a cache is allowed to store a copy of the response message for use in answering subsequent requests
1. first hand - a response is first hand if it comes directly and without unnecessary delay from the origin server, perhaps via one or more proxies. it is also first hand if its validity has just been checked directly with the origin server.
1. explicit expiration time -the time at which the origin server intends that an entity should no longer be returned by a cache without further validation
1. heuristic expiration time - expiration time assigned by a cache when no explicit expiration time is available
1. age - the age of a response is the time since it was sent by or successfully validated with the origin server
1. freshness lifetime - the length of the time between the generation of a response and its expiration time.
1. fresh - a response is fresh if its age has not exceeded its freshness lifetime.
1. stale - a response is stale if its age has passed its freshness lifetime.
1. semantically transparent - when a cache is semantically transparent, the client receives exactly the same response (except for hop-by-hop headers that it would have received had its request been handled directly by the origin server
1. validator - a protocol element that is used to find out whether a cache entry is an equivalent copy of an entity
1. upstream/downstream - describe the flow of a message 
1. inbound/outbound - refer to the request and response paths for messages. inbound means travelling towards the origin server and outbound means travelling towards the user agent

## Overall Operation
- Request/response protocol
- Client sends request to server in the form of
    - request method
    - URI
    - protocol version

    - MIME-like message
        - request modifiers
        - client information
        - possible body content
- Server responds with 
    - status line
        - message protocol version
        - success/error code
    - MIME-like message
        - server information
        - entity metainformation
        - possible entity body content  
- Most HTTP communication is initiated by a user agent towards an origin server
- Some more complicated scenarios will include a proxy, gateway or tunnel(intermediaries)
- Any intermediary other than a tunnel may employ the use of a cache for handling requests.
- Default port is 80 - takes place in TCP/IP connections.
- HTTP presumes a reliable transport. any protocol that has that can be used.
- In HTTP/1.1 a connection can be used for one/more request/response exchanges although connections can be closed for a variety of reasons 

## Basic Rules
- HTTP/1.1 defines the sequence CR LF as the end of line marker for all protocol elements except the entity-body (\r\n). The end of line marker within an entity-body is defined by its associative media type
- HTTP/1.1 header field values can be folded onto multiple lines if the continuation line begins with a space or horizontal tab. All linear whitespace including folding has the same semantics as SP.
- A recipient may replace any linear whitespace with a single SP before interpreting the field value or forwarding the message downstream
- The TEXT rule is only used for descriptive field contents and values that are not intended to be interpreted by the message parser
- A CRLF is allowed in the definition of TEXT only as part of a header field continuation. it is expected that the following LWS will be replaced with a single SP before interpretation of the TEXT value
- Many HTTP/1.1 header field values consist of words separated by LWS or special characters. These special characters must be in quoted string to be used within a parameter value.
- Comments can be included in some HTTP header fields by sorrounding the comment text with parantheses. Comments are only allowed in fields containing "comment" as part of their field value definition. In all other fields, parantheses are considered part of the field value.
- A string or text is considered a single word if its is quoted using double quote marks    
- The backslash character MAY be used as a single-character quote mechanism only within quoted-string and comment constructs

## Protocol Parameters
### HTTP Version
- HTTP uses *major.minor* numbering scheme to indicate versions.
- allows sender to indicate the format of a message and its capacity understanding further HTTP communication, rather than features obtained via that communication.
- version of an HTTP message is indicated by an HTTP-Version field in the first line of the messae
- applications sending request and response messages must include an HTTP version
- proxy and gateway applications must be careful when forwarding messages in protocol versions different from that of the application. 

### Uniform Resource Identifiers
- formatted strings which identify a resource via:
    - name
    - location
    - any other characteristic
- they can be represented in:
    - absolute form
    - relative form
- HTTP does not place any limit on the length of a URI, a server should return 414 if a URI is longer than the server can handle

### Date/Time formats
- HTTP applications have historically allowed three different formats for the representation of date/time stamps:
    - Sun, 06 Nov 1994 08:49:37 GMT  ; RFC 822, updated by RFC 1123
    - Sunday, 06-Nov-94 08:49:37 GMT ; RFC 850, obsoleted by RFC 1036
    - Sun Nov  6 08:49:37 1994       ; ANSI C's asctime() format
- Note: Recipients of date values are encouraged to be robust in accepting date values that may have been sent by non-HTTP applications, as is sometimes the case when retrieving or postingmessages via proxies/gateways to SMTP or NNTP.

### Character Sets
- HTTP uses the definition of character set as that described by MIME
- HTTP character sets are identified by case-insensitive tokens
- complete set of tokens is defined by the IANA character set registry

### Content Codings
- indicate an encoding transformation that has been or can be applied to an entity.
- primarily used to allow a document to be compressed or otherwise usefully transformed without loosing the identity of its underlying media type and without loss of information.
- all content coding values are case insensitive. although the value describes the content-coding, what is more important is that it indicates what decoding mechanism will be required to remove the encoding.
- described by IANA and includes the following tokens
    - gzip 
    - compress
    - deflate

### Transfer Codings
- indicate an encoding transformation that has been or may need to be applied to an entity-body in order to ensure 'safe transport' through the network.
- differs from content coding in that the transfer coding is a property of the message, not the original entity
- chunked encoding modifies the body of a message in order to transfer it as a series of chunks each with its own size indicator, followed by an optional footer containing entity-header fields.
- this allows dynamically produced content to be transferred along with the information necessary for the recipient to verify that it has received the full message.
- the chunked encoding is ended by a zero-sized chunk followed by the footer, which is terminated by an empty line. 
- the purpose of the footer is to provide an efficient way to supply information about an entity that is generated dynamically; applications MUST NOT sendheader fields in the footer which are not explicitly defined as being appropriate for the footer, such as Content-MD5 or future extensions to HTTP for digital signatures or other facilities.
- all HTTP/1.1 applications MUST be able to receive and decode the chunked transfer coding and must ignore transfer coding extensions they do not understand.

### Media Types
- Internet Media Types used in the Content-Type and Accept header fields in order to provide open and extensible data typing and type negotiation.
- user agents that recognize the media type MUST process the parameters for that MIME type as described by that type/subtype definition.

### Product Tokens
- used to allow communicating applications to identify themselves by software name and version
- should be short and precise

### Quality Values
- HTTP content negotiation uses short floating point  numbers to indicate the relative performance of various negotiable parameters.
- a weight is normalized to a real number in the range 0 through 1 where 0 is the minimum and 1 is the maximum
- HTTP/1.1 applications must not generate more than 3 digits after the decimal pint

### Language Tags
- identifies a natural language
- used within Accept-Language and Content-Language fields

### Entity Tags
- used for comparing 2 or more entities from the same requested resource.
- used in the following header fields 
    - ETag 
    - If-Match 
    - If-None-Match  
    - If-Range

### Range Units
- allows a client to request that only part of the response entity be included within the response.
- used in following header fields
    - Range
    - Content-Range
- only range unit defined by HTTP/1.1 is bytes

## HTTP Messages
### Message Types
- 2 types
    - request
    - response
- consist of:
    - start line
    - one or more headers
    - empty line(line with nothing preceding CRLF) - indicating end of header fields
    - optional message-body
- if the server is reading the protocol stream and receives a CRLF first, it should ignore the CRLF

### Message Headers
- each header field consists of a name followed by ':' and the field value.
- field names are case-insensitive
- field value may be preceded by any amount of LWS, though a single SP is preffered.
- header fields can be extended over multiple lines by preceding each extra line with atleast 1 SP or HT
- headers order conventions (discussed below)
    - general headers first
    - request headers/response header
    - entity headers

### Message Body
- used to carry the entity body associated with a request/response.
- message body differs from entity body only when a transfer coding has been applied.
- transfer-encoding must be set to indicate any transfer codings applied by an application to ensure safe and proper transfer of the message.
- transfer-encoding is a property of the message, not of the entity
- presence of a message-body in a request/response is signalled by the inclusion of following headers
    - Content-Length
    - Transfer-Encoding
- not all request methods allow an entity-body
- for response messages, whether or not a message body is included is dependent on both
    - request method
    - response status code
        - those without
            - 1xx
            - 204
            - 304

### Message Length
- when a message-body is included in a message, the length of that body is determined by one of the following
    - any response message which MUST not include a message-body is always terminated by the first empty line after the header fields, regardless of the header fields present in the message.
    - if a transfer-encoding header field is present and indicates that the chunked transfer coding has been applied then the length is defined by the chunked encoding.
    - if a content length header field is present its value in bytes represents the length of the message body.
    - if the message uses the media type multipart/byte-ranges which is self delimiting, then that defines the length. this media type must not be used unless the sender knows that the recipient can parse it
    - by the server closing the connection
    - if a request contains a message body and a content-length is not given, the server should respond with 400 if it cannot determine the length of the message or with 411(Length required) if it wishes to insists on receiving valid content length
    - all HTTP/1.1 applications that receive entities MUST accept the chunked transfer coding thus allowing the mechanism to be used for messages when the message length cannot be determined in advance.
    - messages MUST NOT include both Content-Length header and the chunked transfer encoding. if both are received the Content-Length must be ignored.
    - when a Content-Length is given in a message where a message-body is allowed, its field value must exactly match the number of octets in the message-body. HTTP/1.1 user agents must notify the user when an invalid length is received and detected.

### General Header Fields
- have general applicability for both request and response messages but which do not apply to the entity being transferred
- examples:
    - Cache-Control
    - Connection
    - Date
    - Pragma
    - Transfer-Encoding
    - Upgrade
    - Via
- unrecognized header fields are treated as entity header fields.

## Request
- structure
    - request line
    - headers
    - CRLF
    - message body

### Request Line
- begins with method token, followed by URI and then protocol version and ending with a CRLF
- elements are separated by SP characters

#### Method
- indicates method to be performed on the resource identified by the URI
- case sensitive
- examples
    - OPTIONS
    - GET
    - HEAD
    - POST
    - PUT
    - DELETE
    - TRACE
- the list of methods allowed by a resource can be specified in an Allow header field 
- the return code of the response always notifies the client whether a method is allowed or not
- servers should return code 405 if the method is known by the server but not allowed for the specific resource
- servers should return code 501(Not Implemented) if the method is unrecognized or not implemented by the server 
- list of methods known by the server can be listed in a __Public__ response header
- methods __GET__ and __HEAD__ must be supported all general purpose servers

#### Request-URI
- identifies the resource upon which to apply the request
- BNF
    - Request-URI = "*" | absoluteURI | abs_path
- __"*"__ means that the request does not apply to a particular resource but to the server itself and is only allowed when the method does not necessarily apply to a resource e.g
    - OPTIONS * HTTP/1.1
- __absoluteURI__ is required when the request is being made to a proxy. the proxy is requested to forward the request or service it from a valid cache and return the response. in avoid to avoid request loops, a proxy MUST be able to recognize all of its server names e.g
    - GET http://meliora.co.ke/articles HTTP/1.1
- __abs_path__ is used to identify a resource on origin server or gateway. the absolute path of the URI MUST be trasmitted as the Request-URI and the network location of the URI MUST be transmitted in the __Host__ header field e.g
    -  GET /articles HTTP/1.1
    - Host: www.meliora.co.ke
- If a proxy receives a request without any path in the Request-URI and the method specified is capable of supporting the asterisk form of request, then the last proxy on the request chain MUST forward the request with "*" as the final Request-URI. For example, the request


### The Resource Identified by a Request
- origin servers SHOULD be aware that the exact resource identified by an internet request is determined by examining both the __Request-URI__ and the __Host header field__
- An origin server that does differentiate resources based on the host requested (virtual hosts or vanity hostnames) MUST use the following rules for determining the requested resource on an HTTP/1.1 request:
    - if Request-URI is an absoluteURI, the host is part of the Request-URI. Any Host header field value in the request MUST be ignored.
    - if the Request-URI is not an absoluteURI, and the request includes a Host header field, the host is determined by the Host header field value
    - if the host as determined by rule 1 or 2 is not a valid host on the server, the response MUST be a 400 (Bad Request) error message.

### Request Header Fields
- allow client to pass additional information about the request and about the client itself to the server
- examples:
    - Accept                   
    - Accept-Charset           
    - Accept-Encoding          
    - Accept-Language          
    - Authorization            
    - From                     
    - Host                     
    - If-Modified-Since        
    - If-Match                 
    - If-None-Match            
    - If-Range                 
    - If-Unmodified-Since      
    - Max-Forwards             
    - Proxy-Authorization      
    - Range                    
    - Referer                  
    - User-Agent     

## Response          
### Status Line
- consists of
    - HTTP Version
    - numeric status code and associated textual representation
- BNF
    - Status Line = HTTP-Version SP Status-Code SP Reason-Phrase CRLF

### Status Codes and Reason Phrases
- classes
    - 1XX - informational
    - 2XX - success
    - 3XX - redirection
    - 4XX - client error
    - 5XX - server error
- status codes 
    - 100 - continue
    - 101 - switching protocols
    - ----
    - 200 - ok
    - 201 - created
    - 202 - accepted
    - 203 - non-authoritative information
    - 204 - no content
    - 205 - reset content
    - 206 - partial content
    - ----
    - 300 - multiple choices 
    - 301 - moved permanently
    - 302 - moved temporarily
    - 303 - see other
    - 304 - not modified
    - 305 - use proxy
    - ------
    - 400 - bad request
    - 401 - unauthorized
    - 402 - payment required
    - 403 - forbidden
    - 404 - not found
    - 405 - method not allowed
    - 406 - not acceptable
    - 407 - proxy authentication required
    - 408 - request time-out
    - 409 - conflict
    - 410 - gone
    - 411 - length required
    - 412 - precondition failed
    - 413 - request entity too large
    - 414 - request URI too large
    - 415 - unsupported media type
    - -------
    - 500 - internal server error
    - 501 - not implemented
    - 502 - bad gateway
    - 503 - service unavailable
    - 504 - gateway time out
    - 505 - http version not supported

### Response Header Fields
- allow server to pass additional info about the response
- examples
    - Age
    - Location
    - Proxy-Authenticate
    - Public
    - Retry-After
    - Server
    - Vary
    - Warning
    - WWW-Authenticate

## Entity
- request and response may transfer an entity if not otherwise restricted by the request method or response status code
- entity consists of
    - entity-header fields
    - entity-body

### Entity Header Fields
- define optional meta-info about the entity-body or, if no body is present about the resource identified by the request.
- examples
    - Allow
    - Content-Base
    - Content-Encoding
    - Content-Language
    - Content-Length
    - Content-Location
    - Content-MD5
    - Content-Range
    - Content-Type
    - ETag
    - Expires
    - Last-Modified
    - extension header

### Entity Body
- when an entity body is included with a message, the data type of that body is determined via the Content-Type and Content-Encoding header fields
    - __entity-body := Content-Encoding( Content-Type( data ) )__

- any HTTP/1.1 message containing an entity-body SHOULD include a Content-Type header
- if and only if a content type is not given the recipient MAY attempt to guess it by analyzing the message or using the extension given
- if the media type remains unknown the recipient SHOULD treat it as type __*application/octet-stream*__

----
- the length of an entity body is the length of the message body after the transfer codings have been removed

## Connections
### Persistent Connections
- default behaviour of HTTP/1.1 
- before, every HTTP request created a new TCP connection
- advantages
    - CPU time and memory is saved. less opening and closing of TCP connections
    - reduced network congestion - reduced  TCP opens
    - HTTP requests and responses can be *pipelined*. pipelining is whereby an http client can send multiple requests to a server without waiting for each response - a single TCP connection is used in this case.
    - HTTP can evolve gracefully

### Overall Operation
- client and server have a mechanism to signal the end of a TCP connection. __Connection__ header field is used for this purpose. once a close has been signalled the client must not send any more requests on that connection


### Negotiation
- a server may assume that a HTTP/1.1 client intends to maintain a persisten connection unless a __Connection__ header with value __close__ was sent.
- if a server chooses to close the connection immediately after sending the response it should send __Connection__ header with value __close__
- an HTTP/1.1 client MAY expect a connection to remain open but would decide to keep it open based on whether the response from a server contains a Connection header with value __close__
- If either the client or the server sends the close token in the Connection header, that request becomes the last one for the connection

### Pipelining
- a client that supports persistent connections may pipeline its requests
- a server must send the responses in the same order the requests were received.
- clients which assume persistent connections and pipeline immediately after connection establishment SHOULD be prepared to retry their connection if the first pipelined attempt fails 
- if a client does such a retry, it MUST NOT pipeline before it knows the connection is persistent 
- clients MUST also be prepared to resend their requests if the server closes the connection before sending all of the corresponding responses.

### Proxy Servers
- the proxy server MUST signal persistent connections separately with its clients and the origin servers (or other proxy servers) that it connects to. each persistent connection applies to only one transport link
- a proxy server MUST NOT establish a persistent connection with an HTTP/1.0 client.


### Message Transmission Requirements
- HTTP/1.1 servers SHOULD maintain persistent connections and use TCP's flow control mechanisms to resolve temporary overloads, rather than terminating connections with the expectation that clients will retry. The latter technique can exacerbate network congestion
- An HTTP/1.1 (or later) client sending a message-body SHOULD monitor the network connection for an error status while it is transmitting the request. If the client sees an error status, it SHOULD immediately cease transmitting the body. If the body is being sent using a "chunked" encoding (section 3.6), a zero length chunk and empty footer MAY be used to prematurely mark the end of the message. If the body was preceded by a Content-Length header, the client MUST close the connection.
- An HTTP/1.1 (or later) client MUST be prepared to accept a 100(Continue) status followed by a regular response.
- An HTTP/1.1 (or later) server that receives a request from a HTTP/1.0 (or earlier) client MUST NOT transmit the 100 (continue) response; it SHOULD either wait for the request to be completed normally (thus avoiding an interrupted request) or close the connection prematurely.
    - pon receiving a method subject to these requirements from an HTTP/1.1 (or later) client, an HTTP/1.1 (or later) server MUST either respond with 100 (Continue) status and continue to read from the input stream, or respond with an error status. If it responds with an error status, it MAY close the transport (TCP) connection or it MAY continue to read and discard the rest of the request. It MUST NOT perform the requested method if it returns an error status.
- Clients SHOULD remember the version number of at least the most recently used server; if an HTTP/1.1 client has seen an HTTP/1.1 or later response from the server, and it sees the connection close before receiving any status from the server, the client SHOULD retry the request without user interaction so long as the request method is idempotent (see section 9.1.2); other methods MUST NOT be automatically retried, although user agents MAY offer a human operator the choice of retrying the request.. If the client does retry the request, the client
    - MUST first send the request header fields, and then
    - MUST wait for the server to respond with either a 100 (Continue) response, in which case the client should continue, or with an error status.
- If an HTTP/1.1 client has not seen an HTTP/1.1 or later response from the server, it should assume that the server implements HTTP/1.0 or older and will not use the 100 (Continue) response. If in this case the client sees the connection close before receiving any status from the server, the client SHOULD retry the request. If the client does retry the request to this HTTP/1.0 server, it should use the following "binary exponential backoff" algorithm to be assured of obtaining a reliable response:
    1. Initiate a new connection to the server

    1. Transmit the request-headers

    1. Initialize a variable R to the estimated round-trip time to the server (e.g., based on the time it took to establish the connection), or to a constant value of 5 seconds if the round-trip time is not available.

    1. Compute T = R * (2**N), where N is the number of previous retries of this request.
    
    1. Wait either for an error response from the server, or for T seconds(whichever comes first)

    1. If no error response is received, after T seconds transmit the body of the request.

    1. If client sees that the connection is closed prematurely, repeat from step 1 until the request is accepted, an error response is received, or the user becomes impatient and terminates the retry process.

- no matter the version, if an error code is received, the client
    - MUST NOT continue and
    - MUST close the connection if it has not completed sending the
     message.

- An HTTP/1.1 (or later) client that sees the connection close after receiving a 100 (Continue) but before receiving any other status SHOULD retry the request, and need not wait for 100 (Continue) response (but MAY do so if this simplifies the implementation)

## Method Definitions
### Safe and Idempontent Methods
- a __safe__ method is one that should never have any significance other than retrieval e.g GET and HEAD
- other like POST, PUT and DELETE are unsafe in that they have other 'unsafe' efffects i.e creation, update and deletion respectively
----
- idempotence on the other hand means that apart from error or expiration the side effects of N>0 identical requests is the same for a single request
- examples include:
    - GET
    - HEAD
    - PUT 
    - DELETE

### OPTIONS
- represents a request for information about the communication options available on the request/response chain identified by the Request-URI
- responses to this method are not cacheable
- if the Request-URI is an __*__ then the request refers to the server itself.
- a 200 response should include any header fields which indicate optional features implemented by the server e.g Public
- an __OPTIONS *__ request can  be applied through a proxy by specifying the destination server in the Request-URI without any path information
- if the Request-URI is not an asterisk, the OPTIONS request applies only to the options that are available when communicating with that resource


### GET
- retrieves whatever information is identified by the Request-URI
- if the Request-URI refers
   to a data-producing process, it is the produced data which shall be returned as the entity in the response and not the source text of the process, unless that text happens to be the output of the process.
- the semantics of the GET method change to a "conditional GET" if the request message includes an If-Modified-Since, If-Unmodified-Since, If-Match, If-None-Match, or If-Range header field.
- a conditional GET method requests that the entity be transferred only under the circumstances that the conditional header is met.
- the conditional GET method is intended to reduce unnecessary network usage by allowing cached entities to be refreshed without requiring multiple requests or transferring data already held by the client.
- the semantics of the GET method change to a "partial GET" if the request message includes a Range header field. 
- a partial GET requests that only part of the entity be transferred, as described in section 14.36. The partial GET method is intended to reduce unnecessary network usage by allowing partially-retrieved entities to be completed without transferring data already held by the client.
- the response to a GET request is cachable if and only if it meets the requirements for HTTP caching 

### HEAD
- HEAD method is identical to GET except that the server MUST NOT
 return a message-body in the response.the metainformation contained in the HTTP headers in response to a HEAD request SHOULD be identical to the information sent in response to a GET request
 - This method can
   be used for obtaining metainformation about the entity implied by the
   request without transferring the entity-body itself. This method is
   often used for 
    - testing hypertext links for validity, accessibility,
   and recent modification.
- the response to a HEAD request may be cachable in the sense that the information contained in the response may be used to update a previously cached entity from that resource. 
- if the new field values indicate that the cached entity differs from the current entity (as would be indicated by a change in Content-Length, Content-MD5, ETag or Last-Modified), then the cache MUST treat the cache entry as stale.

### POST
- POST method is used to request that the destination server accept the entity enclosed in the request as a new subordinate of the resource identified by the Request-URI in the Request-Line
- designed to allow a uniform method to:
    - annotation of existing resources
    - posting a message to a bulletin board, newsgroup, mailing list, or similar group of articles;
    - providing a block of data, such as the result of submitting a form, to a data-handling process
    - extending a database through an append operation

- the actual function performed by the POST method is determined by the server and is usually dependent on the Request-URI
- the action performed by the POST method might not result in a resource that can be identified by a URI. In this case, either 200(OK) or 204 (No Content) is the appropriate response status, depending on whether or not the response includes an entity that describes the result.
- if a resource has been created on the origin server, the response SHOULD be 201 (Created) and contain an entity which describes the status of the request and refers to the new resource, and a Location header
- responses to this method are not cachable, unless the response includes appropriate Cache-Control or Expires header fields. However, the 303 (See Other) response can be used to direct the user agent to retrieve a cachable resource.

### PUT
- PUT method requests that the enclosed entity be stored under the supplied Request-URI
- if the Request-URI refers to an already existing resource, the enclosed entity SHOULD be considered as a modified version of the one residing on the origin server. 
- if the Request-URI does not point to an existing resource, and that URI is capable of being defined as a new resource by the requesting user agent, the origin server can create the resource with that URI. 
- if a new resource is created, the origin server MUST inform the user agent via the 201 (Created) response.  
- if an existing resource is modified, either the 200 (OK) or 204 (No Content) response codes SHOULD be sent to indicate successful completion of the request.
- if the request passes through a cache and the Request-URI identifies one or more currently cached entities, those entries should be treated as stale. Responses to this method are not cachable.

### DELETE
- DELETE method requests that the origin server delete the resource identified by the Request-URI 
- MAY be overridden by human intervention (or other means) on the origin server
- the client cannot
   be guaranteed that the operation has been carried out, even if the
   status code returned from the origin server indicates that the action
   has been completed successfully.    
- however, the server SHOULD not
   indicate success unless, at the time the response is given, it
   intends to delete the resource or move it to an inaccessible
   location.
- a successful response SHOULD be 200 (OK) if the response includes an entity describing the status, 202 (Accepted) if the action has not yet been enacted, or 204 (No Content) if the response is OK but does not include an entity.
-  if the request passes through a cache and the Request-URI identifies one or more currently cached entities, those entries should betreated as stale. Responses to this method are not cachable.

### TRACE
- TRACE method is used to invoke a remote, application-layer loop-back of the request message
- final recipient of the request SHOULD reflect the message received back to the client as the entity-body of a 200 (OK) response
- The final recipient is either the origin server or the first proxy or gateway to receive a Max-Forwards value of zero (0) in the request. A TRACE request MUST NOT include an entity.
- allows the client to see what is being received at the other end of the request chain and use that data for testing or diagnostic information
- he value of the Via header field is of particular interest, since it acts as a trace of the request chain. 
- use of the Max-Forwards header field allows the client to limit the length of the request chain, which is useful for testing a chain of proxies forwarding messages in an infinite loop 
- if successful, the response SHOULD contain the entire request message in the entity-body, with a Content-Type of "message/http". Responses to this method MUST NOT be cached.

## Status Code Definitions
1xx(informational)
---
### 100 Continue
- the client may continue with its request
- used to inform client that the initial part of the request has been received and has not yet been rejected by the server
- the client should continue by
    - sending the remainder of the request or
    - if the request has already been completed ignoring this response

### 101 Switching Protocols
- the server and understands and is willing to comply with the client's request via the Upgrade message header to change the application protocol being used on this connection.
- the server will switch protocols to those defined by the response's __Upgrade__ header immediately after the empty line which terminates the 101 response.
- the protocol should only be switched when it's advantageous to do so.

2xx(Successful)
---------
### 200 OK
- the request has succeeded

### 201 CREATED
- request has been fulfilled and has resulted in a new resource being created.
- the newly created resource can be referenced by the URI(s) returned in the entity of the response with the most specific URL for the resource given by __Location__ header field.
- origin server should create the resource before returning 201, if it is unable to do it should return 202

### 202 ACCEPTED
- request has been accepted for processing but the processing has not been completed.
- intentionally non-committal. its purpose is to allow server to accept a request for some other process
- entity returned should include an indication of the request's current status and either a pointer to a status monitor or some estimate of when the user can expect the request to be fulfilled.

### 203 NON AUTHORITATIVE INFORMATION
- the returned metainformation in the entity header is not the definitive set as available from the origin server but is gathered from a local or a third party copy
- the set presented may be a subset or superset of the original version

### 204 NO CONTENT
- server has fulfilled the request but there is no new information to be sent back.
- if the client is a user agent, it should not change its document view from that which caused the request to be sent.
- primarily intended to allow input for actions to take place without causing a change to the user agent's active document view.
- must not include a message-body but can make use of header fields

### 205 RESET CONTENT
- server has fulfilled the request and the user agent SHOULD reset the document view which caused the request to be sent.
- primarily inteneded to allow input for actions to take place via user input followed by a clearing of the form in which the input is given so that the user can easily initiate another input action
- response must NOT include entity body

### 206 PARTIAL CONTENT
- the server has fulfilled the partial GET request for the resource.
- the request must have included a __Range__ header field indicating the desired range.
- response must include either a __Content-Range__ header field indicating the range included with this response or multipart/byteranges __Content-Type__ including __Content-Range__ fields for each part
- if Multipart/byteranges is not used the __Content-Length__ header field in the response MUST match the actual number of octets transmitted in the message body

3xx Redirection
----
### 300 MULTIPLE CHOICES
- requested resource corresponds to any one of a set of representations, each with its own specific location, and agent- driven negotiation information is being provided so that the user can choose a preffered representation and redirect its request to that location.

### 301 MOVED PERMANENTLY
- the requested resource has been assigned a new permanent URI 
- if the new URI is a location, its location should be given in the __Location__ header 

### 302 MOVED TEMPORARILY
- requested resource resides temporarily under a different URI.

### 303 SEE OTHER
- response to the request can be found under a different URI and SHOULD be retrieved using a GET method on that resource.

### 304 NOT MODIFIED
- if the client has performed a conditional GET request and access is allowed but the document has not been modified
- response must not contain a message-body
- response must include following header fields
    - Date 
    - ETag and or Content-Location if the header would have been sent in a 200 response of the same request
    - Expires, Cache-Control and or Vary

### 305 USE PROXY
- the requested resource must be accessed through the proxy given by the __Location__ field 
- recipient is expected to repeat the request via the proxy

4xx (Client Error)
-----


5xx (Server Error)
----
### 500 INTERNAL SERVER ERROR
- server encountered an unexpected condition which prevented it from fulfilling the request

### 501 NOT IMPLEMENTED
- server does not support the functionality required to fulfill the request e.g request method not supported

### 502 BAD GATEWAY
- the server while acting as a gateway/proxy received an invalid response from the upstream server it accessed in attempting to fulfill the request

### 503 SERVICE UNAVAILABLE
- server is currently unable to handle the request due to a temporary overloading or maintenance of the server
- implication is that this is a temporary condition which will be alleviated after some delay
- if known, the length of the delay may be indicated in the __Retry-After__ header, else the client should handle the response as it would handle a 500 response

### 504 GATEWAY TIMEOUT
- server while acting as a gateway or proxy did not receive a timely response from the upstream server it accessed to service the request

### 505 HTTP VERSION NOT SUPPORTED
- the server does not support or refuses to support the HTTP version that was used in the request message
- the response SHOULD contain an entity describing why that version is not supported and what other protocols are supported by that server.


## Access Authentication