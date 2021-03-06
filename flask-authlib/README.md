# Flask Authlib experiment

Poking at using Authlib to facilitate OAuth2 authentication, and taking notes.

There'll be some "don't do this at home" code, so watch out if you copy this.

## Notes

`flask_login` is neutral on persistence in a way that doesn't come through in docs and demos.

Get a `github_client_id` and `github_client_secret` by registering an OAuth app. The page for that is in Developer Settings. It wants a callback url. I used `http://0.0.0.0:5000/github/auth` This motivates setting `AUTHLIB_INSECURE_TRANSPORT=1`.

On successful authentication, github includes `?code=XXXXX` when GETing callback URL. `loginpass._flask` turns that code into an `auth_token`, discarding the code. That token is used to fetch profile info. The token and the profile are then passed to the callback given when constructing the github blueprint. This isn't crystal clear in the doc.


## Open Questions

`flask_login` provides a `@requires_fresh_login` decorator for cases that require extra security. How does that play (or how can it be made to do the right thing) when using OAuth2? You'd need to be logged out of github, lest is say "Sure thing!" and reauthenticate.

Why does github refuse to authenticate if I pass a `redirect_uri` that sure looks like it matches the callback url I registered with the app? Does url-encoding it make a difference? (Authentication works if I don't include a `redirect_uri`, so saving this as a mystery to maybe pursue later.)

## References

* https://docs.authlib.org/en/latest/client/oauth2.html
* https://developer.github.com/apps/building-oauth-apps/authorizing-oauth-apps/
* https://docs.authlib.org/en/latest/client/flask.html

