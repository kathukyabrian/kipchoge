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