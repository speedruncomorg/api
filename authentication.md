# Authentication

Some operations the API offers require a user-context to work. In this case, an API client needs to
authenticate on behalf of a registered user.

To achieve this, each user on speedrun.com has an API key (an alphanumeric string). When storing the
key, give it some space, as we might change the length for newly created keys at any time.
``VARCHAR(250)`` would be a good choice for MySQL, for example.

## HTTP Request & Response

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

## Aquiring a User's API Key

Users can access their key on their settings page. If you don't want to instruct your potential
users to go to their settings page and scroll down, you can instead redirect them to a minimal,
dedicated login workflow that is designed to only access the key.

**Note:** No, this is not OAuth in any way. This is not a fully-automated mechanism to get the key,
the user still has to manually copy&paste it.

Redirect the user to [www.speedrun.com/api/auth](http://www.speedrun.com/api/auth).

![Login Screen](https://raw.githubusercontent.com/speedruncom/api/master/images/api-auth-login.png)

After authenticating, the user will be presented with instructions on what to do next:

![API Key View](https://raw.githubusercontent.com/speedruncom/api/master/images/api-auth-key.png)

For an example, take a look at [LiveSplit](http://livesplit.org/) which uses the auth flow with an
embedded browser to improve user experience.
