People do are implementing KONG in their environment. We recommend the following plugins to be deployed at least in their environment, so that they could be protected from many of the threats. 
 
- https://docs.konghq.com/hub/kong-inc/ip-restriction/ (IP Restrict your KONG Server, there by Securing access to KONG Server) 
- https://docs.konghq.com/hub/kong-inc/rate-limiting/ ( apply rate limits to at least all the IRIS FINANCIAL APIs, Authentication APIs, and OTP APIs.) 
- https://docs.konghq.com/hub/kong-inc/rate-limiting-advanced/ (Only in enterprise version of Kong, but it will give you more control) 
- https://docs.konghq.com/hub/kong-inc/request-transformer/ (Apply this plugin to add Security HEADERS and Remove un-wanted headers from API Requests) 
