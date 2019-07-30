---
layout: default
title: Configuration
permalink: /docs/configuration
has_children: true
nav_order: 3
---

## Configuration

`oauth2_proxy` can be configured via [config file](#config-file), [command line options](#command-line-options) or [environment variables](#environment-variables).

To generate a strong cookie secret use `python -c 'import os,base64; print base64.urlsafe_b64encode(os.urandom(16))'`

### Config File

An example [oauth2_proxy.cfg](contrib/oauth2_proxy.cfg.example) config file is in the contrib directory. It can be used by specifying `-config=/etc/oauth2_proxy.cfg`

### Command Line Options

```
Usage of oauth2_proxy:
  -acr-values string:  optional, used by login.gov (default "http://idmanagement.gov/ns/assurance/loa/1")
  -approval-prompt string: OAuth approval_prompt (default "force")
  -auth-logging: Log authentication attempts (default true)
  -auth-logging-format string: Template for authentication log lines (see "Logging Configuration" paragraph below)
  -authenticated-emails-file string: authenticate against emails via file (one per line)
  -azure-tenant string: go to a tenant-specific or common (tenant-independent) endpoint. (default "common")
  -basic-auth-password string: the password to set when passing the HTTP Basic Auth header
  -client-id string: the OAuth Client ID: ie: "123456.apps.googleusercontent.com"
  -client-secret string: the OAuth Client Secret
  -config string: path to config file
  -cookie-domain string: an optional cookie domain to force cookies to (ie: .yourcompany.com)
  -cookie-expire duration: expire timeframe for cookie (default 168h0m0s)
  -cookie-httponly: set HttpOnly cookie flag (default true)
  -cookie-name string: the name of the cookie that the oauth_proxy creates (default "_oauth2_proxy")
  -cookie-path string: an optional cookie path to force cookies to (ie: /poc/)* (default "/")
  -cookie-refresh duration: refresh the cookie after this duration; 0 to disable
  -cookie-secret string: the seed string for secure cookies (optionally base64 encoded)
  -cookie-secure: set secure (HTTPS) cookie flag (default true)
  -custom-templates-dir string: path to custom html templates
  -display-htpasswd-form: display username / password login form if an htpasswd file is provided (default true)
  -email-domain value: authenticate emails with the specified domain (may be given multiple times). Use * to authenticate any email
  -extra-jwt-issuers: if -skip-jwt-bearer-tokens is set, a list of extra JWT issuer=audience pairs (where the issuer URL has a .well-known/openid-configuration or a .well-known/jwks.json)
  -exclude-logging-paths: comma separated list of paths to exclude from logging, eg: "/ping,/path2" (default "" = no paths excluded)
  -flush-interval: period between flushing response buffers when streaming responses (default "1s")
  -banner string: custom banner string. Use "-" to disable default banner.
  -footer string: custom footer string. Use "-" to disable default footer.
  -gcp-healthchecks: will enable /liveness_check, /readiness_check, and / (with the proper user-agent) endpoints that will make it work well with GCP App Engine and GKE Ingresses (default false)
  -github-org string: restrict logins to members of this organisation
  -github-team string: restrict logins to members of any of these teams (slug), separated by a comma
  -google-admin-email string: the google admin to impersonate for api calls
  -google-group value: restrict logins to members of this google group (may be given multiple times).
  -google-service-account-json string: the path to the service account json credentials
  -htpasswd-file string: additionally authenticate against a htpasswd file. Entries must be created with "htpasswd -s" for SHA encryption
  -http-address string: [http://]<addr>:<port> or unix://<path> to listen on for HTTP clients (default "127.0.0.1:4180")
  -https-address string: <addr>:<port> to listen on for HTTPS clients (default ":443")
  -logging-compress: Should rotated log files be compressed using gzip (default false)
  -logging-filename string: File to log requests to, empty for stdout (default to stdout)
  -logging-local-time: If the time in log files and backup filenames are local or UTC time (default true)
  -logging-max-age int: Maximum number of days to retain old log files (default 7)
  -logging-max-backups int: Maximum number of old log files to retain; 0 to disable (default 0)
  -logging-max-size int: Maximum size in megabytes of the log file before rotation (default 100)
  -jwt-key string: private key in PEM format used to sign JWT, so that you can say something like -jwt-key="${OAUTH2_PROXY_JWT_KEY}": required by login.gov
  -jwt-key-file string: path to the private key file in PEM format used to sign the JWT so that you can say something like -jwt-key-file=/etc/ssl/private/jwt_signing_key.pem: required by login.gov
  -login-url string: Authentication endpoint
  -insecure-oidc-allow-unverified-email: don't fail if an email address in an id_token is not verified
  -oidc-issuer-url: the OpenID Connect issuer URL. ie: "https://accounts.google.com"
  -oidc-jwks-url string: OIDC JWKS URI for token verification; required if OIDC discovery is disabled
  -pass-access-token: pass OAuth access_token to upstream via X-Forwarded-Access-Token header
  -pass-authorization-header: pass OIDC IDToken to upstream via Authorization Bearer header
  -pass-basic-auth: pass HTTP Basic Auth, X-Forwarded-User and X-Forwarded-Email information to upstream (default true)
  -pass-host-header: pass the request Host Header to upstream (default true)
  -pass-user-headers: pass X-Forwarded-User and X-Forwarded-Email information to upstream (default true)
  -profile-url string: Profile access endpoint
  -provider string: OAuth provider (default "google")
  -ping-path string: the ping endpoint that can be used for basic health checks (default "/ping")
  -proxy-prefix string: the url root path that this proxy should be nested under (e.g. /<oauth2>/sign_in) (default "/oauth2")
  -proxy-websockets: enables WebSocket proxying (default true)
  -pubjwk-url string: JWK pubkey access endpoint: required by login.gov
  -redeem-url string: Token redemption endpoint
  -redirect-url string: the OAuth Redirect URL. ie: "https://internalapp.yourcompany.com/oauth2/callback"
  -redis-connection-url string: URL of redis server for redis session storage (eg: redis://HOST[:PORT])
  -redis-sentinel-master-name string: Redis sentinel master name. Used in conjuction with --redis-use-sentinel
  -redis-sentinel-connection-urls: List of Redis sentinel conneciton URLs (eg redis://HOST[:PORT]). Used in conjuction with --redis-use-sentinel
  -redis-use-sentinel: Connect to redis via sentinels. Must set --redis-sentinel-master-name and --redis-sentinel-connection-urls to use this feature (default: false)
  -request-logging: Log requests to stdout (default true)
  -request-logging-format: Template for request log lines (see "Logging Configuration" paragraph below)
  -resource string: The resource that is protected (Azure AD only)
  -scope string: OAuth scope specification
  -session-store-type: Session data storage backend (default: cookie)
  -set-xauthrequest: set X-Auth-Request-User and X-Auth-Request-Email response headers (useful in Nginx auth_request mode)
  -set-authorization-header: set Authorization Bearer response header (useful in Nginx auth_request mode)
  -signature-key string: GAP-Signature request signature key (algorithm:secretkey)
  -silence-ping-logging bool: disable logging of requests to ping endpoint (default false) 
  -skip-auth-preflight: will skip authentication for OPTIONS requests
  -skip-auth-regex value: bypass authentication for requests path's that match (may be given multiple times)
  -skip-jwt-bearer-tokens: will skip requests that have verified JWT bearer tokens
  -skip-oidc-discovery: bypass OIDC endpoint discovery. login-url, redeem-url and oidc-jwks-url must be configured in this case
  -skip-provider-button: will skip sign-in-page to directly reach the next step: oauth/start
  -ssl-insecure-skip-verify: skip validation of certificates presented when using HTTPS
  -standard-logging: Log standard runtime information (default true)
  -standard-logging-format string: Template for standard log lines (see "Logging Configuration" paragraph below)
  -tls-cert-file string: path to certificate file
  -tls-key-file string: path to private key file
  -upstream value: the http url(s) of the upstream endpoint or file:// paths for static files. Routing is based on the path
  -validate-url string: Access token validation endpoint
  -version: print version string
  -whitelist-domain: allowed domains for redirection after authentication. Prefix domain with a . to allow subdomains (eg .example.com)
```

Note, when using the `whitelist-domain` option, any domain prefixed with a `.` will allow any subdomain of the specified domain as a valid redirect URL.

See below for provider specific options

### Upstreams Configuration

`oauth2_proxy` supports having multiple upstreams, and has the option to pass requests on to HTTP(S) servers or serve static files from the file system. HTTP and HTTPS upstreams are configured by providing a URL such as `http://127.0.0.1:8080/` for the upstream parameter, that will forward all authenticated requests to be forwarded to the upstream server. If you instead provide `http://127.0.0.1:8080/some/path/` then it will only be requests that start with `/some/path/` which are forwarded to the upstream.

Static file paths are configured as a file:// URL. `file:///var/www/static/` will serve the files from that directory at `http://[oauth2_proxy url]/var/www/static/`, which may not be what you want. You can provide the path to where the files should be available by adding a fragment to the configured URL. The value of the fragment will then be used to specify which path the files are available at. `file:///var/www/static/#/static/` will ie. make `/var/www/static/` available at `http://[oauth2_proxy url]/static/`.

Multiple upstreams can either be configured by supplying a comma separated list to the `-upstream` parameter, supplying the parameter multiple times or provinding a list in the [config file](#config-file). When multiple upstreams are used routing to them will be based on the path they are set up with.

### Environment variables

Every command line argument can be specified as an environment variable by
prefixing it with `OAUTH2_PROXY_`, capitalising it, and replacing hypens (`-`)
with underscores (`_`). This is particularly useful for storing secrets outside
of a configuration file or the command line.

For example, the `--cookie-secret` flag becomes `OAUTH2_PROXY_COOKIE_SECRET` and
the `--set-authorization-header` flag becomes `OAUTH2_PROXY_SET_AUTHORIZATION_HEADER`.


## Logging Configuration

By default, OAuth2 Proxy logs all output to stdout. Logging can be configured to output to a rotating log file using the `-logging-filename` command.

If logging to a file you can also configure the maximum file size (`-logging-max-size`), age (`-logging-max-age`), max backup logs (`-logging-max-backups`), and if backup logs should be compressed (`-logging-compress`).

There are three different types of logging: standard, authentication, and HTTP requests. These can each be enabled or disabled with `-standard-logging`, `-auth-logging`, and `-request-logging`.

Each type of logging has their own configurable format and variables. By default these formats are similar to the Apache Combined Log.

Logging of requests to the `/ping` endpoint can be disabled with `-silence-ping-logging` reducing log volume. This flag appends the `-ping-path` to `-exclude-logging-paths`.

### Auth Log Format
Authentication logs are logs which are guaranteed to contain a username or email address of a user attempting to authenticate. These logs are output by default in the below format:

```
<REMOTE_ADDRESS> - <user@domain.com> [19/Mar/2015:17:20:19 -0400] [<STATUS>] <MESSAGE>
```

The status block will contain one of the below strings:

- `AuthSuccess` If a user has authenticated successfully by any method
- `AuthFailure` If the user failed to authenticate explicitly
- `AuthError` If there was an unexpected error during authentication

If you require a different format than that, you can configure it with the `-auth-logging-format` flag.
The default format is configured as follows:

```
{% raw %}{{.Client}} - {{.Username}} [{{.Timestamp}}] [{{.Status}}] {{.Message}}{% endraw %}
```

Available variables for auth logging:

| Variable | Example | Description |
| --- | --- | --- |
| Client | 74.125.224.72 | The client/remote IP address. Will use the X-Real-IP header it if exists. |
| Host  | domain.com | The value of the Host header. |
| Protocol | HTTP/1.0 | The request protocol. |
| RequestMethod | GET | The request method. |
| Timestamp | 19/Mar/2015:17:20:19 -0400 | The date and time of the logging event. |
| UserAgent | - | The full user agent as reported by the requesting client. |
| Username | username@email.com | The email or username of the auth request. |
| Status | AuthSuccess | The status of the auth request. See above for details. |
| Message | Authenticated via OAuth2 | The details of the auth attempt. |

### Request Log Format
HTTP request logs will output by default in the below format:

```
<REMOTE_ADDRESS> - <user@domain.com> [19/Mar/2015:17:20:19 -0400] <HOST_HEADER> GET <UPSTREAM_HOST> "/path/" HTTP/1.1 "<USER_AGENT>" <RESPONSE_CODE> <RESPONSE_BYTES> <REQUEST_DURATION>
```

If you require a different format than that, you can configure it with the `-request-logging-format` flag.
The default format is configured as follows:

```
{% raw %}{{.Client}} - {{.Username}} [{{.Timestamp}}] {{.Host}} {{.RequestMethod}} {{.Upstream}} {{.RequestURI}} {{.Protocol}} {{.UserAgent}} {{.StatusCode}} {{.ResponseSize}} {{.RequestDuration}}{% endraw %}
```

Available variables for request logging:

| Variable | Example | Description |
| --- | --- | --- |
| Client | 74.125.224.72 | The client/remote IP address. Will use the X-Real-IP header it if exists. |
| Host  | domain.com | The value of the Host header. |
| Protocol | HTTP/1.0 | The request protocol. |
| RequestDuration | 0.001 | The time in seconds that a request took to process. |
| RequestMethod | GET | The request method. |
| RequestURI | "/oauth2/auth" | The URI path of the request. |
| ResponseSize | 12 | The size in bytes of the response. |
| StatusCode | 200 | The HTTP status code of the response. |
| Timestamp | 19/Mar/2015:17:20:19 -0400 | The date and time of the logging event. |
| Upstream | - | The upstream data of the HTTP request. |
| UserAgent | - | The full user agent as reported by the requesting client. |
| Username | username@email.com | The email or username of the auth request. |

### Standard Log Format
All other logging that is not covered by the above two types of logging will be output in this standard logging format. This includes configuration information at startup and errors that occur outside of a session. The default format is below:

```
[19/Mar/2015:17:20:19 -0400] [main.go:40] <MESSAGE>
```

If you require a different format than that, you can configure it with the `-standard-logging-format` flag. The default format is configured as follows:

```
{% raw %}[{{.Timestamp}}] [{{.File}}] {{.Message}}{% endraw %}
```

Available variables for standard logging:

| Variable | Example | Description |
| --- | --- | --- |
| Timestamp | 19/Mar/2015:17:20:19 -0400 | The date and time of the logging event. |
| File | main.go:40 | The file and line number of the logging statement. |
| Message | HTTP: listening on 127.0.0.1:4180 | The details of the log statement. |

## <a name="nginx-auth-request"></a>Configuring for use with the Nginx `auth_request` directive

The [Nginx `auth_request` directive](http://nginx.org/en/docs/http/ngx_http_auth_request_module.html) allows Nginx to authenticate requests via the oauth2_proxy's `/auth` endpoint, which only returns a 202 Accepted response or a 401 Unauthorized response without proxying the request through. For example:

```nginx
server {
  listen 443 ssl;
  server_name ...;
  include ssl/ssl.conf;

  location /oauth2/ {
    proxy_pass       http://127.0.0.1:4180;
    proxy_set_header Host                    $host;
    proxy_set_header X-Real-IP               $remote_addr;
    proxy_set_header X-Scheme                $scheme;
    proxy_set_header X-Auth-Request-Redirect $request_uri;
  }
  location = /oauth2/auth {
    proxy_pass       http://127.0.0.1:4180;
    proxy_set_header Host             $host;
    proxy_set_header X-Real-IP        $remote_addr;
    proxy_set_header X-Scheme         $scheme;
    # nginx auth_request includes headers but not body
    proxy_set_header Content-Length   "";
    proxy_pass_request_body           off;
  }

  location / {
    auth_request /oauth2/auth;
    error_page 401 = /oauth2/sign_in;

    # pass information via X-User and X-Email headers to backend,
    # requires running with --set-xauthrequest flag
    auth_request_set $user   $upstream_http_x_auth_request_user;
    auth_request_set $email  $upstream_http_x_auth_request_email;
    proxy_set_header X-User  $user;
    proxy_set_header X-Email $email;

    # if you enabled --pass-access-token, this will pass the token to the backend
    auth_request_set $token  $upstream_http_x_auth_request_access_token;
    proxy_set_header X-Access-Token $token;

    # if you enabled --cookie-refresh, this is needed for it to work with auth_request
    auth_request_set $auth_cookie $upstream_http_set_cookie;
    add_header Set-Cookie $auth_cookie;

    # When using the --set-authorization-header flag, some provider's cookies can exceed the 4kb
    # limit and so the OAuth2 Proxy splits these into multiple parts.
    # Nginx normally only copies the first `Set-Cookie` header from the auth_request to the response,
    # so if your cookies are larger than 4kb, you will need to extract additional cookies manually.
    auth_request_set $auth_cookie_name_upstream_1 $upstream_cookie_auth_cookie_name_1;

    # Extract the Cookie attributes from the first Set-Cookie header and append them
    # to the second part ($upstream_cookie_* variables only contain the raw cookie content)
    if ($auth_cookie ~* "(; .*)") {
        set $auth_cookie_name_0 $auth_cookie;
        set $auth_cookie_name_1 "auth_cookie_name_1=$auth_cookie_name_upstream_1$1";
    }

    # Send both Set-Cookie headers now if there was a second part
    if ($auth_cookie_name_upstream_1) {
        add_header Set-Cookie $auth_cookie_name_0;
        add_header Set-Cookie $auth_cookie_name_1;
    }

    proxy_pass http://backend/;
    # or "root /path/to/site;" or "fastcgi_pass ..." etc
  }
}
```

If you use ingress-nginx in Kubernetes (which includes the Lua module), you also can use the following configuration snippet for your Ingress:

```yaml
nginx.ingress.kubernetes.io/auth-response-headers: Authorization
nginx.ingress.kubernetes.io/auth-signin: https://$host/oauth2/start?rd=$request_uri
nginx.ingress.kubernetes.io/auth-url: https://$host/oauth2/auth
nginx.ingress.kubernetes.io/configuration-snippet: |
  auth_request_set $name_upstream_1 $upstream_cookie_name_1;

  access_by_lua_block {
    if ngx.var.name_upstream_1 ~= "" then
      ngx.header["Set-Cookie"] = "name_1=" .. ngx.var.name_upstream_1 .. ngx.var.auth_cookie:match("(; .*)")
    end
  }
```

You have to substitute *name* with the actual cookie name you configured via --cookie-name parameter. If you don't set a custom cookie name the variable  should be "$upstream_cookie__oauth2_proxy_1" instead of "$upstream_cookie_name_1" and the new cookie-name should be "_oauth2_proxy_1=" instead of "name_1=".
