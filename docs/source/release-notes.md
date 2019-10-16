## Release Notes

### Casa 4.0 

- Added support for FIDO2 / WebAuthn devices    
- Smarter 2FA: Users no longer need to select a "preferred mechanism", and are now simply prompted for the strongest enrolled credential type (e.g. FIDO > OTP). [Learn more](https://gluu.org/docs/casa/4.0/administration/2fa-basics/#associated-strength-of-credentials)
- "Hot deployment" for plugins -- now plugins can be added by dropping files directly in the chroot filesystem, in addition to through the UI.  
- New sample plugins for developers plus an improved developer guide    
- Enhanced localization support, including a language picker in the user-facing dashboard     
