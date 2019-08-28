# Storage of User Credentials

The following provides a summary of where user credentials can be found in LDAP. If you need information regarding Couchbase, please open a support ticket.

## U2F devices
Information about U2F security keys and Super Gluu devices are stored under a `fido` branch for every user entry. The entries belong to the `oxDeviceRegistration` object class and contain information about each enrolled device. One way to differentiate between a U2F Security Key and a Super Gluu device is to inspect the `oxDeviceData` attribute. Super Gluu devices will include information here, while U2F Security Keys will not.

!!! Note  
    We strongly recommend **not** modifying these attributes directly unless you have a good reason to do so.  

## FIDO 2 devices
Relevant information can be found under `fido2_register` branch of the user's entry.

## TOTP / HOTP devices
TOTP/HOTP device information is stored in the `oxExternalUid` attribute as well as in `oxOTPDevices`.

## Phone Numbers
Verified mobile phone numbers are stored in the `mobile` attribute of the user entry. Associated information (date added, nickname of device, etc.) is stored in JSON format in the `oxMobileDevices` attribute.

## 2FA enforcement policy

When administrators allow users to set their own strong authentication policy, that is, users being able to decide if 2FA authentication always takes place or only when device/location used is not recognized, the LDAP attributes involved are `oxStrongAuthPolicy` and `oxTrustedDevicesInfo`. The former contains the user preference, and the latter the information of his trusted devices and locations. For privacy reasons, data stored in `oxTrustedDevicesInfo` is encoded so the only applicable operation upon this data is flushing the list; this can be achieved by removing the attribute entirely.

