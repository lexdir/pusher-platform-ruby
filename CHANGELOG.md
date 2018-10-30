# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased](https://github.com/pusher/pusher-platform-ruby/compare/0.11.1...HEAD)

### Changes

- Remove rack as a required dependency

## [0.11.1](https://github.com/pusher/pusher-platform-ruby/compare/0.11.0...0.11.1) - 2018-09-18

### Changes

- Makes rack an explicit required dependency in the Gemspec

## [0.11.0](https://github.com/pusher/pusher-platform-ruby/compare/0.10.0...0.11.0) - 2018-09-18

### Changes

- *Breaking* An instance of `SDKInfo` is now required when instantiating an instance of `BaseClient`. This can either be directly provided to the `BaseClient` and then the base client provided to an instance of `Instance` in the `client` key or the `options` object that `Instance`s initializer takes, or you can provide the `SDKInfo` instance to the `Instance` initializer in the `sdk_info` key of its `options` parameter

## [0.10.0](https://github.com/pusher/pusher-platform-ruby/compare/0.9.0...0.10.0) - 2018-09-17

### Changes

- All errors now inherit from `PusherPlatform::Error`
- An exception is raised if no `key` key is present in the hash passed to the `initialize` function for `Instance`

## [0.9.0](https://github.com/pusher/pusher-platform-ruby/compare/0.8.2...0.9.0) - 2018-09-13

### Changes

- Removed defaulting to generating a JWT with the `su: true` claim if no JWT is provided to a call to `request`
- Bump jwt dependency to 2.1.0
- Bump excon dependency to 0.62.0
- JWT algorithm now enforced to be `HS256` when decoding a refresh token

## [0.8.2](https://github.com/pusher/pusher-platform-ruby/compare/0.8.1...0.8.2) - 2018-07-20

### Fixes

- Fixes `ErrorResponse` being incorrectly instantiated in `BaseClient` [#19](https://github.com/pusher/pusher-platform-ruby/issues/19) [@CarlLooksUp](https://github.com/CarlLooksUp)

## [0.8.1](https://github.com/pusher/pusher-platform-ruby/compare/0.8.0...0.8.1) - 2018-05-24

### Fixes

- Fixes `ErrorResponse` being incorrectly instantiated [#18](https://github.com/pusher/pusher-platform-ruby/issues/18)

### Additions

- Adds support for service-specific claims in tokens
- Adds an example of publishing to a feed in [Feeds](https://pusher.com/feeds)

### Changes

- Removed runtime dependency on Rack

## [0.8.0](https://github.com/pusher/pusher-platform-ruby/compare/0.7.0...0.8.0) - 2018-04-20

### Additions

- `authenticate_with_refresh_token` has been added if you want to support the `refresh_token` grant type and return refresh tokens as part of the authentication process
- `authenticate_with_refresh_token_and_request` has been added if you want to support the `refresh_token` grant type and return refresh tokens as part of the authentication process, as well as providing the request object when authenticating

### Changes

- `authenticate` no longer returns a `refresh_token` and no longer accepts the `refresh_token` grant type
- Calls to `authenticate` and `authenticate_with_refresh_token` always return an `AuthenticationResponse` that looks like this (in JSON form):

```ruby
{
  status: 200,
  headers: { "Some-Header" => "some value" },
  body: {
    access_token: "an_access_token",
    token_type: "bearer",
    expires_in: 86400
  }
}
```

where:

* `status` is the suggested HTTP response status code,
* `headers` are the suggested response `headers`,
* `body` holds either the token payload or an appropriate error payload.

Here is an example of the expected usage (using Sinatra), simplified for brevity:

```ruby
post '/' do
  auth_payload = instance.authenticate_with_request(request, { user_id: 'USER_ID' })
  [auth_payload.status, auth_payload.body.to_json]
end
```

## [0.7.0](https://github.com/pusher/pusher-platform-ruby/compare/0.6.0...0.7.0) - 2018-04-12

### Changes

- Rename `authenticate` to `authenticate_with_request`
- `authenticate` (now `authenticate_with_request`) no longer returns a status code. It will either return a `Hash` containing the required authentication payload or an error with information about the reason for authentication failure
- Add some examples

### Additions

- Examples for `authenticate` and `authenticate_with_request`

### Fixes

- Changed module name from `Pusher` to `PusherPlatform` (fixes #15)
- Fixes some `require`s

## [0.6.0](https://github.com/pusher/pusher-platform-ruby/compare/0.5.1...0.6.0) - 2018-01-26

### Changes

- `app` claim in JWTs changed to `instance`

_.. prehistory_
