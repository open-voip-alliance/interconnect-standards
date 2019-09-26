# interconnect-standards
A definition on how we interconnect with other parties.

# Definitions
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
      "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and
      "OPTIONAL" in this document are to be interpreted as described in
      [RFC 2119](https://tools.ietf.org/html/rfc2119).


# Voice Codecs
The following voice codecs should be used. Use any priority order you want. You should at least offer the codec's that are labled "Required". Choosing an inefficient codec is always preferred over transcoding.

| Codec      | R/O?     |
| ---------- | -------- |
| G.722      | Required |
| G.711A     | Required |
| G.711U     | Required |
| All others | Optional |

# Response when an exception occurs
An interconnect should use the response in relation to the situation that is described the table below.

| Situation                      | SIP response                | Error type      |
| ------------------------------ | --------------------------- | --------------- |
| Callee declined call           | 486 Busy Here               | Definitive      |
| Callee is busy                 | 486 Busy Here               | Definitive      |
| Number not in use              | 404 Not Found               | Definitive      |
| Number does not exist          | 404 Not Found               | Definitive      |
| Number temporarily unavailable | 480 Temporarily Unavailable | Definitive      |
| General Server error           | 500 Service Unavailable     | Try other route | 
| Destination not allowed        | 403 Forbidden               | Try other route |
| Certificate validation fails   | 437 Unsupported Certificate | Try other route |

A definitive error means that the number isn't available through other routes, and that the calling operator doesn't have to try another route for the call. 

If a customer PBX returns an error code (5xx), it is recommended to rewrite that error to a '480 Temporarily Unavailable' response. Since all errors in the 5xx range are treated as definitive errors. 

# Number format
The e.164 standard should be used. This is a full number, with country prefix and without zero's. We also omit the '+' symbol. It is recommended to still apply normalization if calls come in with other formats.

# Caller identification and ANI
The `From` header must contains the number that will be shown transparantly to the callee. Futhermore, a `P-Asserted-Identity` header should exists in INVITE packets. This header contains the ANI. The PAI header should always be sent, even if both CLI and ANI are identical.

# Authorisation and authentication
Gateways should use IP authentication for interconnection.

# DTMF mode
The RFC4733 standard should be used. In case RFC4733 is not supported, RFC2833 must be used. Other methods are unsupported.

# T.38 Fax Support
The T.38 protocol should be used for fax traffic. Voice codecs can be used as fallback.

# Encryption
The use of SIP TLS and SRTP is recommended, especially over public connections. If TLS is used, certificate validation must take place. If certificate validation fails, the call must find another route.
