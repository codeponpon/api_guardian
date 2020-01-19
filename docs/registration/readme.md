# ApiGuardian Registration

Registration can be handled in a number of ways. The gist of it is that each
registration strategy has different required fields, but you will always pass in
a `type` attribute to specify the strategy you want to use. If registration
succeeds, the user record will be returned.

Endpoint: POST `{engine_mount_path}/register`

## Configuration

ApiGuardian has a hook available after a registration takes place. This allows you to do something like send a welcome email, or perhaps subscribe the user to a mailing list, or maybe send a Slack notification. No matter which registration stategy is used, this block will be executed.

```rb
ApiGuardian.configure do |config|
  # ...

  # Often, applications will want to send emails or do other things specific to
  # registration. You can use this block to hook into what happens after a user is
  # registered.
  config.after_user_registered = lambda do |user|
    MyMailer.welcome(user).deliver_later
  end

  # ...
end
```

## Email

To register a user via email, the following fields are required.

```js
{
  "type": "email",
  "email": "person@example.com",
  "password": "somepassword",
  "password_confirmation": "somepassword"
}
```

## Third-Party Registration

Each third-party registration strategy makes use of a handful of fields to provide
the proper data for creating a user. *Note: Password can be optionally provided so
that the user can also sign in via email on return trips. If it is not provided
then a strong, random password will be generated for them*

### Facebook

Facebook registration assumes that a Facebook OAuth access token has been acquired
from some other client library. All that you'll need to pass in is the access token
and ApiGuardian will take care of validating it and creating a user.

To register a user via Facebook, the following fields are required.

```js
{
  "type": "facebook",
  "access_token": "access_token_returned_from_facebook_sdk",
  "password": "somepassword", // Optional
  "password_confirmation": "somepassword" // Optional
}
```

---

ApiGuardian is copyright © 2015-2020 Travis Vignon. It is free software, and may be
redistributed under the terms specified in the [`MIT-LICENSE`](https://github.com/lookitsatravis/api_guardian/blob/master/MIT-LICENSE) file.
