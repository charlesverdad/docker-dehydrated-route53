# docker-dehydrated-route53

A Docker image for obtaining Let's Encrypt SSL certificates for AWS Route53 domains using [dehydrated](https://github.com/lukas2511/dehydrated) and [whereisaaron/dehydrated-route53-hook-script](https://github.com/whereisaaron/dehydrated-route53-hook-script). This is based off the Cloudflare version in [TimDumol/docker-dehydrated-cloudflare](https://github.com/TimDumol/docker-dehydrated-cloudflare)

## Usage

#### Create Docker Volume (optional)

This is where the certs and account data will be stored

```
$ docker create -v /etc/ssl/certs:/dehydrated/certs -v /dehydrated/accounts --name dehydrated-data charlesverdad/docker-dehydrated-route53
```

#### Register an account (One-time run)
Register an account with the CA. You only need to do this once.

```
$ docker run --rm -e CONTACT_EMAIL='tech@example.com' --volumes-from dehydrated-data timdumol/docker-dehydrated-route53 --register --accept-terms
```

#### Generate the certificates

```
$ docker run \
    -e CONTACT_EMAIL='tech@example.com' \
    -e AWS_ACCESS_KEY_ID=youraccesskeyid \
    -e AWS_SECRET_ACCESS_KEY='yoursecretaccesskey' \
    --volumes-from dehydrated-data \
    charlesverdad/docker-dehydrated-route53 \
    --cron \
    -d "*.example.com" \
    --alias example

```

Configuration for dehydrated is also available by setting the appropriate environemnt variable (see `config.tmpl` for the full list of variables)