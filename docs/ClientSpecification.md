# Client Specification

## [Header Part](#Header Part)

The header part consists of header fields. Each header field is comprised of a name and a value, separated by ‘: ‘ (a colon and a space). The structure of header fields conform to the [HTTP semantic](https://tools.ietf.org/html/rfc7230#section-3.2). Each header field is terminated by ‘\r\n’. Considering the last header field and the overall header itself are each terminated with ‘\r\n’, and that at least one header is mandatory, this means that two ‘\r\n’ sequences always immediately precede the content part of a message.

Currently the following header fields are supported:

| Header Field Name | Value Type | Description                                                  |
| :---------------- | :--------- | :----------------------------------------------------------- |
| Content-Length    | number     | The length of the content part in bytes. This header is required. |
| Content-Type      | string     | The mime type of the content part. Defaults to jsonrpc; charset=utf-8 |

The header part is encoded using the ‘ascii’ encoding. This includes the ‘\r\n’ separating the header and content part.

### [Content Part](#contentPart)

Contains the actual content of the message. The content part of a message uses [JSON-RPC](http://www.jsonrpc.org/) to describe requests, responses and notifications. The content part is encoded using the charset provided in the Content-Type field. It defaults to `utf-8`, which is the only encoding supported right now. If a server or client receives a header with a different encoding than `utf-8` it should respond with an error.

(Prior versions of the protocol used the string constant `utf8` which is not a correct encoding constant according to [specification](http://www.iana.org/assignments/character-sets/character-sets.xhtml).) For backwards compatibility it is highly recommended that a client and a server treats the string `utf8` as `utf-8`.

### Example:

```
Content-Length: ...\r\n
\r\n
{
	"jsonrpc": "2.0",
	"id": 1,
	"method": "textDocument/didOpen",
	"params": {
		...
	}
}
```

### Base Protocol JSON structures

The following TypeScript definitions describe the base [JSON-RPC protocol](http://www.jsonrpc.org/specification):

## Initialize Request

```json
{
    "method" : "CVSEngine/initialize"
    "params" : {
    	// subsystem name from engine extensions
    	"subSystem" : ["git", "svn"]
		"urlFormat" : {
            /* Fill in the URL format of the associated subsystem in the subsystem, 
             * and support two ways of matching, URL prefix and regular matching.
             * It will be used to select the corresponding subsystem for management when initializing the respository
             */
            "git" : {
                "scheme" : ["https", "http"],
                "regExp" : ["....", "...."]
            },
            "svn" : {
                "scheme" : ["https", "http"]
            }
        }
	}
}
```

## Initialize Response

```json
{
    "result" : {
        "capabilities" : {
            /* The service returns the currently supported subsystem. 
             * If the request does not match the content of the return request, 
             * it indicates that there is an unsupported rear terminal system.
             */
            "subSystem" : ["git", "svn"]
            ...
        },
        "serverInfo" : {
        	"name" : "CVSEngine",
        	"version" : "..."
        }
    }
}
```

## checkout respository Request

```json
{
    "method" : "respository/checkout",
	"params" : {
        "remoteUrl" : "https://xxxxxxx.git",
        // target path from local file
        "targetPath" : "file://xxx",
        "userName" : "user1",
        "passwd" : "passwd",
        "token" : {
       		"localfile" : "file:///xxxxx",
            "string" : "xxx"    
        }
    }
}
```

## checkout respository Response

```json
{
    "result" : {
        "remoteUrl" : "https://xxxxxxx.git",
        "targetPath" : "file:///xxx",
        "subSystem" : "git",
    }
}
```

## checkout respository event

```json
{
    "method" : "respository/checkout/event",
    "params" : {
        "message" : string,
        "token" : true | false,
        "Percentage" : {
        	"current" : int,
            "max" : int
        }
    }
}
```

