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