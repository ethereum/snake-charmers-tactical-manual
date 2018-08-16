# Security

The software we work on is used in financial applications.  This makes us targets.
The following minimum security practices should be used.

- Use a password manager with strong passwords.
- Don't use browser based password storage or autofilling of passwords.
- Enable two factor authentication but **DO NOT** use SMS codes as a second
  factor.
  * Mobile carriers are notoriously bad at safe-guarding their customers'
    accounts against hijacking.  In some cases, simply knowing someone's mobile
    number and appearing in person at one of their carrier's stores is enough
    to hijack their account.  Accounts can also be socially hacked by knowing
    the mobile number in addition to a few publically available details of the
    account holder's life which are commonly used in account recovery processes
    (father's middle name, mother's maiden name, model of first car, etc.).
- Setup auto-signing of your git commits
    - https://stackoverflow.com/questions/10161198/is-there-a-way-to-autosign-commits-in-git-with-a-gpg-key
- No password sharing (even for family, close friends, etc)
- Auto-lock machine after a short period (5 minutes) of inactivity
- No shared machine usage (family computer, etc)
- Use pre-boot encryption
- Recommend using a VPN anytime you are on an untrusted network
    - ([ProtonVPN](https://protonvpn.com/) is a good VPN provider)
- Never connect to untrusted devices (USB, etc).  
    - This means any device that you cannot personally account for it's origins.  
    - This means you cannot plug someone else's USB drive into your machine to transfer files.
    - This means you cannot plug someone else's phone into your computer to charge it.
