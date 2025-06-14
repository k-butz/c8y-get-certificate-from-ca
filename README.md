# About

This project provides a command-line interface to interact with Cumulocitys Certificate Authority. 

The tool is using functionality from the go-client provided by [go-c8y](https://github.com/reubenmiller/go-c8y) library.

# Prerequisites

Following prerequisites are applying:

* The user needs `ROLE_DEVICE_CONTROL_ADMIN` permission in Cumulocity (the permission to register new Devices)

* The Cumulocity-CA feature being enabled in your tenant ([link](https://cumulocity.com/docs/device-certificate-authentication/certificate-authority/#prerequisites))

* CA Certificate must be present in your tenant ([link](https://cumulocity.com/docs/device-certificate-authentication/certificate-authority/#creating-a-ca-certificate-via-the-ui))

See [Cumulocity Certificate Authority](https://cumulocity.com/docs/device-certificate-authentication/certificate-authority/) for further info.

# Usage

> Tip: For CLI usage you can always use `-h/--help` to see the list of available options.

The tool comes with following sub-commands:

* `registerUsingPassword` allowing to provide user-credentials (which will be used for automatic device-registration):

```
./c8y-certificate-cli registerUsingPassword \
  --device-id 'kobu-device-001' \
  --cumulocity-host 'https://iot.cumulocity.com' \
  --cumulocity-tenant-id 't12345' \
  --cumulocity-user 'john.doe' \
  --cumulocity-password 'superSecret1234'
```

* `registerUsingPoller`: This does not require user-credentials for enrollment. Instead, it will periodically poll for registration until a User created a matching Device Registration request in the target tenant.

```
./c8y-certificate-cli registerUsingPoller \
  --device-id 'kobu-device-001' \
  --cumulocity-host 'https://iot.cumulocity.com'
  --one-time-password 'secret-token'
```

> In case you specific a one-time-password, make sure it's less than 32 characters and does not contain a double-quote.

* `renewCert`: Command is accepting current certificate and private-key and requests a new certificate with them.

```
./c8y-certificate-cli renewCert \
  --cumulocity-host 'https://iot.cumulocity.com' \
  --current-certificate ./c8y-certificate.pem \
  --private-key ./c8y-private-key.pem \
  --new-certificate-name ./c8y-certificate.new.pem
```

* `verifyCert`: Command accepts host, certificate and private key and tests if it's valid (by requesting an access token via HTTP). Exit Code 0 if valid, 1 if invalid.

```
./c8y-certificate-cli verifyCert \
  --cumulocity-host 'https://iot.cumulocity.com' \
  --certificate ./c8y-certificate.pem \
  --private-key ./c8y-private-key.pem 
```

* `getAccessToken`: Command accepts host, certificate and private key and responds with an access token obtained from Cumulocity

```
./c8y-certificate-cli getAccessToken \
  --cumulocity-host 'https://iot.cumulocity.com' \
  --certificate ./c8y-certificate.pem \
  --private-key ./c8y-private-key.pem 
```

# Miscellaneous

* The examples folder contains scripts that can be used to connect a Cumulocity Thick-Edge to a Cloud instance via Cumulocity CA.
