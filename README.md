# Pronouns in /.well-known Protocol

## 1. Introduction

The Pronouns in /.well-known protocol provides a standardized way for clients to retrieve pronoun information for users or resources. This protocol allows clients to request pronouns for a specific user or retrieve a default set of pronouns.

## 2. Protocol Specification

### 2.1 Request

Clients can send a GET request to the `/.well-known/pronouns` endpoint to retrieve pronoun information. The request can include an optional query parameter `resource` to specify a specific user or resource.

Example request:

```
GET /.well-known/pronouns HTTP/1.1
Host: example.com
```

For sites of multiple users, you can use an arbitrary string in the params to request a specific one. For example:

```
GET /.well-known/pronouns?resource=astrid%40example.com HTTP/1.1
Host: example.com
```

### 2.2 Response

The server responds with a JSON object containing pronoun information. The keys of the object are language codes and the values are arrays of pronouns.

ISO-639-1 language codes are used to specify the language of the pronouns. For example, the language code for English is `en`. The language code can be further refined with a region code, such as `en-us` for American English.

Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json

{
    "en-us": ["she/her", "they/them", "it/its"]
}
```

For English pronouns outside of the usual set (which the protocol might include, but not be limited to: he/him, she/her, they/them, it/its), you can specify every position:

```
HTTP/1.1 200 OK
Content-Type: application/json

{
    "en-us": [
        {
            "subject": "they",
            "object": "them",
            "dependent_possessive": "their",
            "independent_possessive": "theirs",
            "reflexive": "themself"
        },
        {
            "subject": "xe",
            "object": "xem",
            "dependent_possessive": "xir",
            "independent_possessive": "xirs",
            "reflexive": "xirself"
        }
    ]
}
```

### 2.3 Error Handling

Servers are allowed to respond with a 403 Forbidden status code for this endpoint if the client is not authorized to access the requested resource.

```
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
    "error": "Forbidden"
}
```

## 3. Security Considerations

As with any protocol that exposes user information, care must be taken to ensure that privacy is maintained. Servers should only provide pronoun information for users who have explicitly opted in to this feature. Additionally, servers should implement appropriate access controls to prevent unauthorized access to user information.