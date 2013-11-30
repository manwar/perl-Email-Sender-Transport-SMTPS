# NAME

Email::Sender::Transport::SMTPS - Email::Sender joins Net::SMTPS

# SYNOPSIS

	use Email::Sender::Simple qw(sendmail);
	use Email::Sender::Transport::SMTPS;
	use Try::Tiny;

	my $transport = Email::Sender::Transport::SMTPS->new(
	    host => 'smtp.gmail.com',
	    ssl  => 'starttls',
	    sasl_username => 'myaccount@gmail.com',
	    sasl_password => 'mypassword',
	);

	# my $message = Mail::Message->read($rfc822)
	#         || Email::Simple->new($rfc822)
	#         || Mail::Internet->new([split /\n/, $rfc822])
	#         || ...
	#         || $rfc822;
	# read L<Email::Abstract> for more details

	use Email::Simple::Creator; # or other Email::
	my $message = Email::Simple->create(
	    header => [
	        From    => 'myaccount@gmail.com',
	        To      => 'to@mail.com',
	        Subject => 'Subject title',
	    ],
	    body => 'Content.',
	);

	try {
	    sendmail($message, { transport => $transport });
	} catch {
	    die "Error sending email: $_";
	};

# DESCRIPTION

This transport is used to send email over SMTP, either with or without secure
sockets (SSL/TLS). it uses the great [Net::SMTPS](https://metacpan.org/pod/Net::SMTPS).

that's based on a patch for Email::Sender::Transport::SMTP. [https://github.com/fayland/email-sender/commit/bba4a590be87506248349293c5b457dfa533daae](https://github.com/fayland/email-sender/commit/bba4a590be87506248349293c5b457dfa533daae)
most of the code are copied from [Email::Sender::Transport::SMTP](https://metacpan.org/pod/Email::Sender::Transport::SMTP)

# ATTRIBUTES

The following attributes may be passed to the constructor:

- `host`: the name of the host to connect to; defaults to `localhost`
- `ssl`: 'ssl' / 'starttls' / undef, if true, passed to [Net::SMTPS](https://metacpan.org/pod/Net::SMTPS) doSSL.
- `port`: port to connect to; defaults to 25 for non-SSL, 465 for 'ssl' and 587 for 'starttls'
- `timeout`: maximum time in secs to wait for server; default is 120
- `sasl_username`: the username to use for auth; optional
- `sasl_password`: the password to use for auth; required if `username` is provided
- `allow_partial_success`: if true, will send data even if some recipients were rejected; defaults to false
- `helo`: what to say when saying HELO; no default
- `localaddr`: local address from which to connect
- `localport`: local port from which to connect

# PARTIAL SUCCESS

If `allow_partial_success` was set when creating the transport, the transport
may return [Email::Sender::Success::Partial](https://metacpan.org/pod/Email::Sender::Success::Partial) objects.  Consult that module's
documentation.

# EXAMPLES

## send email with Gmail

    my $smtp  = Email::Sender::Transport::SMTP->new({
      host => 'smtp.gmail.com',
      ssl  => 'starttls',
      sasl_username => 'myaccount@gmail.com',
      sasl_password => 'mypassword',
    });

# AUTHOR

Fayland Lam <fayland@gmail.com>

# COPYRIGHT

Copyright 2013- Fayland Lam

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO
