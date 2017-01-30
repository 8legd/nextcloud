# Phusion's baseimage seems a good starting point
# See https://github.com/phusion/baseimage-docker
FROM phusion/baseimage:0.9.19

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# Install Caddy (as per https://github.com/abiosoft/caddy-docker)
RUN curl --silent --show-error --fail --location \
  --header "Accept: application/tar+gzip, application/x-gzip, application/octet-stream" -o - \
  "https://caddyserver.com/download/build?os=linux&arch=amd64" \
  | tar --no-same-owner -C /usr/bin/ -xz caddy \
  && chmod 0755 /usr/bin/caddy \
  && /usr/bin/caddy -version

COPY ./Caddyfile /etc/Caddyfile
