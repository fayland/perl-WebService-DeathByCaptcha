# NAME

WebService::DeathByCaptcha - DeathByCaptcha Recaptcha API

# SYNOPSIS

    use WebService::DeathByCaptcha;

    my $dbc = WebService::DeathByCaptcha->new(
        username => ',
        password => $ENV{DEATHBYCAPTCHA_PASS},
    );

    my $dbc_res = $dbc->recaptcha({
        googlekey => '6Le-wvkSAAAAAPBMRTvw0Q4Muexq9bi0DJwx_mJ-',
        pageurl => 'https://www.google.com/recaptcha/api2/demo',
        # proxy => "http://user:password@127.0.0.1:1234",
        # proxytype => 'HTTP',
    }) or die $dbc->errstr;

    die $dbc_res->{error} if $dbc_res->{error};
    my $captcha_id = $dbc_res->{captcha};

    sleep 60;
    my $recaptcha_res;
    while (1) {
        $dbc_res = $dbc->get($captcha_id);
        die $dbc_res->{error} if $dbc_res->{error};

        warn Dumper(\$dbc_res);
        if ($dbc_res->{status} eq '0' and $dbc_res->{text}) {
            $recaptcha_res = $dbc_res->{text};
            last;
        } elsif ($dbc_res->{status} eq '0') {
            sleep 5; # another sleep
        } else {
            die; # should never happen
        }
    }

    # $res = $ua->post('https://www.google.com/recaptcha/api2/demo', Content => [
    #     'g-recaptcha-response' => $recaptcha_res,
    # ]);

# DESCRIPTION

WebService::DeathByCaptcha is for [http://www.deathbycaptcha.com/user/api/newtokenrecaptcha](http://www.deathbycaptcha.com/user/api/newtokenrecaptcha)

# AUTHOR

Fayland Lam <fayland@gmail.com>

# COPYRIGHT

Copyright 2017- Fayland Lam

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO
