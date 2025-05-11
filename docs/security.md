# Important Security Notes

## Do Not Run on the Internet

Please note that Firefly Field Day Logger does **NOT** have the
required web application security to run over the public
Internet. FFDL is intended to be a lightweight application for local use
at a Field Day operation over a local LAN/WiFi connection with a group of
well-behaved, well-meaning operators. It does not contain any authentication
security, serious input sanitization, or significant anti-XSS protections.

If you feel like you MUST implement this over the Internet, the best I can
suggest is that the server implements HTTP Basic Authentication at the
server-level. However **DO NOT** plan to actually do this - it is not
recommended.

## HTTPS / TLS Considerations

As the web evolves, the reliability of using cookies with sites not using
the https:// (i.e. TLS-encrypted) connections is becoming problematic. It
is strongly recommended that the server is configured to offer https://
connections and clients use those connections. There are known issues
with stock Chromium on Linux with non-TLS-protected cookies. Here's how to enable
https:// connections for this application.

Test the TLS configuration by visiting the server on https://. For example
if the hostname is logger.fd.local, browse to https://logger.fd.local. A
warning about a certificate error will appear because the configuration
is using the default "fake" certificate. This is OKAY for non-Internet-connected
purposes. Accept the error and select the browser's equivalent of "trust this site"
or "continue anyway" or "accept the risk and continue". The site should load.

It is strongly recommended to enable HTTPS/TLS on all Firefly Logger installations
to avoid potential cookie problems.