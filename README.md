# NAME

Plack::App::HipChat::WebHook - HipChat WebHook Plack Application

# VERSION

version 0.001

# SYNOPSIS

    use Plack::App::HipChat::WebHook;

    my $app = Plack::App::HipChat::WebHook->new({
        hipchat_user_agent => 'ExpectedUserAgent',  # Optional
        webhooks => {
            '/webhook_notification' => sub {
                my $rh = shift;

    #
    # Do something with $rh (decoded JSON webhook notification)
    #

                return [ 200,
                         [ 'Content-Type' => 'text/plain' ],
                         [ 'Completed' ]
                     ];
            },
        },
    })->to_app;

    # plackup <abovescript.pl>

# DESCRIPTION

A Plack application to receive WebHook notifications from HipChat (see
https://www.hipchat.com/docs/apiv2/webhooks). Register webhooks
to new() with callbacks which are called with the decoded JSON payload
of a event when received from HipChat that matches the path.

The callback is passed a hashref that looks like this:

    \ {
        event             "room_notification",
        item              {
            message   {
                color            "yellow",
                date             "2015-01-17T20:29:06.018495+00:00",
                from             "Some Dude",
                id               "aaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
                mentions         [],
                message          "Message Text",
                message_format   "html",
                type             "notification"
            },
            room      {
                id      123456,
                links   {
                    participants   "https://api.hipchat.com/v2/room/123456/participant",
                    self           "https://api.hipchat.com/v2/room/123456",
                    webhooks       "https://api.hipchat.com/v2/room/123456/webhook"
                },
                name    "ChatRoom"
            }
        },
        oauth_client_id   "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
        webhook_id        123456
    }

Pass hipchat\_user\_agent to new() for checking of the 'User-Agent' in each
request. This defaults to 'HipChat.com'.

## call

If the path matches a configured webhook, checks that the content-type and
user-agent are set as expected, then decode the JSON content and fire the
callback with that hashref.

## return\_400

Return a 400 (Bad request) error.

## return\_404

Return a 404 (Not found) error.

# SEE ALSO

[WebService::HipChat](https://metacpan.org/pod/WebService::HipChat)

# AUTHOR

Chris Hughes <chrisjh@cpan.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2015 by Chris Hughes.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
