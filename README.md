# letsencrypt-api-client
Independent command line client for the "Let’s Encrypt" (ACME) API. It is for people who want to generate certificate signing requests on their own and like to have little bit more control over the signing process.

## precondition
 * You need Ruby >1.9
 * You may need ruby-json, in case it is not within your standard "libruby" distribution package
 * You need write access into the folder <code>/.well-known/acme-challenge/</code> on your web-root where your domain is hosted.

## example usage

First you need an identity also named account key. This a RSA key which can be generated with the following command
<pre><code>openssl genrsa -out account.key 4096</code></pre>

Then you need an key for your Domain you want a certificate for
<pre><code>openssl genrsa -out domain.key 4096</code></pre>

You can generate the certificate signing request file with one of the following commands
<pre><code>openssl req -new -sha256 -key domain.key -subj "/CN=www.example.org" -out domain.csr
openssl req -new -sha256 -key domain.key -subj "/" -reqexts SAN -config <(cat /etc/ssl/openssl.cnf <(printf "[SAN]\nsubjectAltName=DNS:example.org,DNS:www.example.org")) > domain.csr</code></pre>

And finally run the command
<pre><code>./letsencrypt.rb -k account.key -c domain.csr -f /var/www/htdocs/.well-known/acme-challenge -o domain.cer</code></pre>

