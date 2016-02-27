# OCSP: OCSP Count Server Percentage
OCSP: OCSP Count Server Percentage

This is a tool that is run against a list of domain names, like for example the
Alexa Top 1,000,000 and extracts some statistics regarding the usage of OCSP and
OCSP Stapling.

## Usage

```bash
./ocsp <amount> <file>
# Amount: The maximum amount of domains to run for
# File: A file with one domain name per line and no other content
```

## Example Output

```
+ - - - - - - +
| Statistics  |
+ - - - - - - +
Total Domains Processed:                                   10
Total Domains with OCSP Stapling:                          3
Total Domains without OCSP Stapling:                       6
Total Domains with OCSP:                                   9
Total Domains with correct OCSP Stapling:                  3
Total Domains with HTTPS:                                  9
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

## Script Configuration

There are several variables in the `ocsp` bash script. You can tweak them in order to change
the behavior of the script. Here is some explanation on what they do:

### OPENSSL_LOCATION
The variable `OPENSSL_LOCATION` is used to determine the location of the `openssl(1)`
executable. This can be a full path or a relative path, or simply if you have the desired
version in your `$PATH` "openssl". Please note that the executable *MUST* be called or contain
`openssl` in its name or be a wrapper that finally calls a binary named `openssl`.

### OUTPUT_FILE
The variable `OUTPUT_FILE` must contain a file in a location in your system where both are
Readable and Writeable by the current user. The contents of this file will be *deleted* and
replaced with new data. The data stored in this file is all the SSL/TLS Connection data obtained
via OpenSSL. This includes OCSP Response Data, Certificates, as well as other information.
Please note that this file *cannot* be `/dev/null` or a similar file/device since it is then
read by the script. If you do not want this file stored, you can use `/tmp`.

### SLEEP_TIME
The variable `SLEEP_TIME` contains the amount in seconds between the processing of each
domain name in the list. Domains are processed in parallel but in order to avoid problems
with local system resources or the network you can configure the rate at which a parallel
processor starts.
