# OCSP: OCSP Count Server Percentage
OCSP: OCSP Count Server Percentage

This is a tool that is run against a list of domain names, like for example the
Alexa Top 1,000,000 and extracts some statistics regarding the usage of OCSP and
OCSP Stapling.

## Usage:

```bash
./ocsp <amount> <file>
# Amount: The maximum amount of domains to run for
# File: A file with one domain name per line and no other content
```

## Security Considerations

This is a `bash(1)` script that is reading a file and then running shell commands
based on the contents of the file. It is trivial to create a file that, if loaded,
will result in arbitrary code execution. Please only use with a trusted domain name
file. The author takes no responsiblity for any security issues that may rise with
the use of this script or any damage, including but not limited to data loss.

## Running on Linux

Inside the `ocsp` script, you can find a variable conveniently titled `OPENSSL_LOCATION`.
This variable should point to the full path of the OpenSSL binary that will be used. If
you have `openssl(1)` in your `bash(1)` `$PATH`, then you can simply enter `openssl`.

The current script will work only on Mac OS X where OpenSSL was manually installed
with `brew(1)` and more specifically version `1.0.2f`. Any recent version of OpenSSL will
work but no others have been tested.

In addition to that, there is a limitation on non-Mac OS X computers, which allows only
a single run of this tool at a time, per folder. This has to do with a hack for a temporary
file created. It will likely be fixed in a future version.
