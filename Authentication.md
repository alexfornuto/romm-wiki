_Authentication will be generally available in version [2.0.0](https://github.com/zurdi15/romm/releases). If you'd like to try it before the next release, use the [latest 2.0.0 release candidate](https://hub.docker.com/r/zurdi15/romm/tags) on the Docker Hub._

***

RomM provides support for various forms of authentication, granting flexibility in securing access to its features. Authentication is an optional feature, disabled by defaults and needs to be manually enabled.

### Enabling authentication

Authentication can be enabled by setting the `ROMM_AUTH_ENABLED` environment variable to `true`.

- `ROMM_AUTH_USERNAME` and `ROMM_AUTH_PASSWORD` should be set to create the default admin user
- `ROMM_AUTH_SECRET_KEY` is required and can be generated with `openssl rand -hex 32`

**Note: sessions are backed by Redis, and login will not work without it. [See how to enable the experimental Redis backend.](https://github.com/zurdi15/romm/wiki/Experimental-Redis-Cache)**

<details>
  <summary>Example docker-compose.yml</summary>
  
  ```yaml
  version: "3"
  volumes:
    mysql_data:
  services:
    romm:
      image: zurdi15/romm:latest
      container_name: romm
      environment:
        - ROMM_DB_DRIVER=sqlite
        - ROMM_AUTH_ENABLED=true
        - ROMM_AUTH_SECRET_KEY=<secret key> # Generate a key with `openssl rand -hex 32`
        - ROMM_AUTH_USERNAME=admin
        - ROMM_AUTH_PASSWORD=<admin password> # default: admin
        - ENABLE_EXPERIMENTAL_REDIS=true
        - REDIS_HOST=redis
        - REDIS_PORT=6379
        - CLIENT_ID=<IGDB client id>
        - CLIENT_SECRET=<IGDB client secret>
      volumes:
        - "/path/to/library:/romm/library"
      ports:
        - 80:80
      depends_on:
        - romm_db
      restart: "unless-stopped"

    redis:
      image: redis:alpine
      container_name: redis
      restart: unless-stopped
      ports:
        - 6379:6379
  ```
</details>

### Sessions

When the `/login` endpoint is called with valid credentials, a `session_id` is generated, stored as a cookie and sent to the browser. The same token is used to create a cache entry in Redis (or in-memory if Redis is disabled) which maps the token to the user. This way no sensitive information is stored on the client.

### Roles

A user can have one of the following roles:

- VIEWER: Can view platforms and ROMs, download ROMs, and edit own profile
- EDITOR: Can create/edit/delete platforms and ROMs
- ADMIN: Can view all users, and create/edit/disable/delete users

As permissions are additive, editors will have all permissions of the `viewer` role, and admins all those of the `editor` role.

## Basic Authentication

Requests can be made to protected API endpoints with an authorization header. The token is the base64 encoded value of `username:password`.

Example using cURL:

```bash
curl https://romm.local/api/platforms -H 'Authorization: Basic YWRtaW46aHVudGVyMg=='
```

## OAuth

Along with the above forms of authentication, we've added an endpoint to generate expiring, scope-limited authentication tokens (`/api/token`). Successfully authenticating with that endpoint with return an `access_token` valid for 15 minutes, and a [`refresh_token`](https://oauth.net/2/grant-types/refresh-token/) valid for 2 weeks. The `refresh_token` can be used to generate a new `access_token` when needed.

The `/api/token` endpoint requires a username, password, and a list of [scopes](https://oauth.net/2/scope/) in the format `read:roms write:roms read:platforms ...`. The list of scopes and endpoints are available to browse via Swagger UI or Redoc (see next section).

**Note: As of now, only the legacy [password grant type](https://oauth.net/2/grant-types/password/) is supported.** We plan to eventually add support for [Client Credentials](https://oauth.net/2/grant-types/client-credentials/).

### OpenAPI

The API endpoints are fully documented and compliant with the OpenAPI specification. Explore the API endpoints using the Swagger UI interface at `/api/docs` and the Redoc interface at `/api/redoc`, or view the raw JSON at `/openapi.json`.

For more information on OpenAPI, visit the [OpenAPI Specification](https://www.openapis.org/) website.

## FAQ

### Can I disable authentication after having it enabled?

Authentication can be disabled at any time with no risk to data. Permissions checks are bypassed, so RomM will grant full access to any user who accesses the app. Existing users and permissions are maintained in the database; if authentication is re-enabled, those users will still be able to access RomM with their credentials.

**Note that since full access is granted to all endpoints when authentication is disabled, any visitor will be able to create/update/delete any user, even admins.**

### Is the API available when authentication is disabled?

Yes, but permission checks are bypassed, so full access to every endpoint is granted to any user/machine who requests it.

### I want to allow an EDITOR to edit ROMs but not delete them. Can I do that?

At this time, fine-grain control over permissions within a role is not supported. This decision was taking in order to simplify user management in the client, and authentication/permission code on the server.

### Is authentication safe/robust? Can I trust it?

We've done our best to build an authentication system that is simple, clear and comprehensible. We have automated tests which verify that access is granted when it should be, and blocked when not (invalid credentials, missing permissions, expired access tokens, etc.). That being said, we welcome any reviews of our authentication and permission flows, PRs to fix issues, and new tests to cover edge cases.

### I found an bug/issue with authentication. How do I report it?

Please report bugs in our authentication/permission system privately by [submitting a vulnerability report](https://github.com/zurdi15/romm/security/advisories/new).