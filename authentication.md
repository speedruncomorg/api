# Authentication

Some operations the API offers require a user-context to work. In this case, an API client needs to
authenticate on behalf of a registered user.

To achieve this, each user on speedrun.com has an API key (an alphanumeric string). When storing the
key, give it some space, as we might change the length for newly created keys at any time.
``VARCHAR(250)`` would be a good choice for MySQL, for example.

The key needs to be sent in form of a **HTTP header** called ``X-API-Key``. Take a look at this
sample:

```http
GET /api/v1/profile HTTP/1.1
Host: www.speedrun.com
Accept: application/json
X-API-Key: r0hbq2qq84hf9t47jdvmeh4gl
```

If the API key is missing or invalid, the API will respond with a **HTTP 403** response:

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json; charset=utf-8

{
  "status": 403,
  "message": "This operations requires a user context, but no valid API Key was submitted in your request."
}
```

Note that users can revoke API keys at any time, so be prepared that a once functioning key suddenly
fails to work.
