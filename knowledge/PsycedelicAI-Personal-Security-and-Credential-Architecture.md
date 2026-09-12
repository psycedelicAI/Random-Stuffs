# PsycedelicAI Personal Security and Credential Architecture

> A private working document describing Psycedelic’s layered approach to
> authentication, password management, encryption, trusted devices and recovery.

---

## Document Status

| Field | Value |
|---|---|
| Document | Personal Security and Credential Architecture |
| Owner | Psycedelic |
| Working identity | PsycedelicAI |
| Classification | Public |
| Status | Living document |
| Format | Markdown |
| Purpose | Preserve continuity around personal security decisions |
| Last reviewed | 2026-09-12 |

---

## 1. Purpose

This document consolidates the security ideas, tools and design decisions developed
across previous conversations about:

- hardware authentication;
- password managers;
- encrypted storage;
- trusted devices;
- recovery;
- account protection;
- physical security;
- credential separation;
- personal threat modelling;
- and continuity of security decisions over time.

It is not intended to contain secrets.

Do not store the following in this file:

- passwords;
- recovery codes;
- private keys;
- API keys;
- seed phrases;
- vault keys;
- full device serial numbers;
- personal identification numbers;
- exact recovery locations;
- sensitive account details.

This document describes the architecture and principles, not the credentials
themselves.

---

# 2. Central Security Model

The security model is based on layered trust rather than one password or one
application.

```text
Human identity
      +
Trusted device
      +
Strong authentication
      +
Credential separation
      +
Encrypted storage
      +
Context and authorization
      +
Recovery planning
      +
Review and audit
      =
Layered personal security
```

The central principle is:

> No single stolen password, device, file or credential should automatically
> provide complete access to important accounts or data.

Another important principle is:

> A credential may identify someone without proving that the current request,
> device, location or action is trustworthy.

---

# 3. Personal Security Philosophy

The approach is influenced by the wider PsycedelicAI and High-Security Facility
Concept work.

Security should be understood as a relationship between:

- identity;
- role;
- device;
- credential;
- location;
- context;
- requested action;
- operational state;
- authorization;
- revocation;
- recovery;
- and review.

This means that a valid credential should not automatically be treated as
sufficient in every situation.

Examples:

```text
Valid password
+ unknown device
= increased risk
```

```text
Valid YubiKey
+ wrong account
+ unexpected login context
= investigate
```

```text
Trusted device
+ compromised active session
= device and session risk remains
```

The goal is not to create a system that assumes everything is safe.

The goal is to make important actions require several independent conditions.

---

# 4. Authentication Layers

## 4.1 YubiKeys

YubiKeys are used as physical hardware authentication factors.

Their main value is that an attacker normally needs physical possession of the
key in addition to the other required credentials.

YubiKeys can help protect against:

- phishing;
- fake login pages;
- password reuse;
- stolen passwords;
- many remote account-takeover attempts;
- unauthorized access from an unfamiliar device.

Important operational principles:

- Register at least two keys for important accounts where supported.
- Keep a backup key available but protected.
- Do not store both keys together.
- Review registered keys periodically.
- Remove lost, old or unknown keys immediately.
- Keep account recovery methods under review.
- Do not assume a YubiKey protects a device that is already compromised.
- Do not approve unexpected authentication prompts.

The YubiKey is an authentication factor.

It is not automatically:

- a backup;
- a password manager;
- a full-disk encryption key;
- protection against malware;
- protection against an unlocked active session.

---

## 4.2 Two-Key Redundancy

The current design uses two registered YubiKeys for important authentication.

Conceptually:

```text
YubiKey 1 = primary daily-use key
YubiKey 2 = protected backup key
```

The backup key exists to prevent account lockout if the primary key is:

- lost;
- damaged;
- unavailable;
- forgotten;
- or temporarily inaccessible.

The backup key must be protected strongly enough that losing physical control
does not expose the accounts it protects.

The exact storage location should not be recorded in this document.

---

## 4.3 OnlyKey

OnlyKey has been considered and used as a protected password-entry device.

Its intended benefit is to reduce the need to manually type sensitive passwords,
particularly where keyloggers or observation are a concern.

OnlyKey should be treated as a separate security component from YubiKey.

```text
YubiKey
= authentication and hardware-backed account access

OnlyKey
= protected password entry and credential handling
```

A hardware password-entry device does not eliminate all risk.

Remaining risks include:

- compromised endpoint;
- malware capturing credentials after entry;
- unauthorized physical access;
- weak account recovery;
- incorrect device configuration;
- accidental exposure;
- loss of the device;
- unsafe browser sessions.

---

## 4.4 Trusted Device Authentication

A trusted device can make daily access more practical, but trust should not be
permanent or unconditional.

The current model includes a primary approved laptop, referred to in previous
conversations as the **zBook**.

A trusted-device decision should consider:

- operating-system integrity;
- full-disk encryption;
- security updates;
- endpoint protection;
- physical control;
- browser state;
- active sessions;
- local user accounts;
- device recovery;
- and whether the device is being used in an expected context.

A device being marked as trusted should not remove the need for strong
authentication for sensitive actions.

---

# 5. Primary Device Security

## 5.1 zBook

The zBook is treated as the primary approved device for sensitive work.

The described configuration includes:

- ZorinOS/Linux;
- full-disk encryption;
- HP Wolf Security or related hardware and endpoint protections;
- controlled use for sensitive accounts;
- password-management access;
- YubiKey-based authentication.

This creates several useful layers:

```text
Physical device control
      ↓
Full-disk encryption
      ↓
Operating-system security
      ↓
Endpoint and hardware protections
      ↓
YubiKey authentication
      ↓
Password-manager access
```

This does not mean the device is invulnerable.

Important remaining risks include:

- malware running while the system is unlocked;
- browser-session theft;
- malicious extensions;
- phishing;
- supply-chain compromise;
- stolen active sessions;
- unsafe recovery procedures;
- accidental disclosure;
- weak passwords protecting encrypted storage.

---

## 5.2 Full-Disk Encryption

Full-disk encryption primarily protects data when the device is:

- powered off;
- lost;
- stolen;
- or physically accessed by someone without the disk-unlock credentials.

It does not fully protect data when:

- the device is already unlocked;
- malware is running;
- a user session is compromised;
- a malicious process has access to decrypted files;
- credentials are entered into a compromised browser.

The distinction is:

```text
Device powered off
→ disk encryption is a strong protection layer
```

```text
Device unlocked and session compromised
→ disk encryption may no longer protect active data
```

Full-disk encryption should therefore be combined with:

- strong login credentials;
- secure boot where appropriate;
- automatic locking;
- current updates;
- endpoint protection;
- least privilege;
- controlled physical access;
- and secure backups.

---

# 6. Password-Management Architecture

Several password-management tools have been discussed or used, with the goal of
giving each tool a clear role rather than allowing uncontrolled duplication.

## 6.1 heylogin

heylogin is used as one part of the password-management and authentication
architecture.

The model discussed includes:

```text
Approved browser or device
      +
YubiKey authentication
      +
heylogin access
      =
stronger contextual access
```

The purpose is to avoid relying only on a manually entered master password for
every access decision.

Important considerations:

- maintain a clear recovery path;
- review approved browsers and devices;
- remove old devices;
- protect the account email;
- ensure that the account does not become a single point of failure;
- understand how credentials are recovered;
- verify what data is stored locally and what is stored remotely;
- review active sessions regularly.

---

## 6.2 Proton Pass

Proton Pass is used for some password-management purposes.

Its potential role is:

- everyday account credentials;
- generated passwords;
- secure notes where appropriate;
- aliases or identity separation if used;
- credentials that benefit from synchronized access.

It should not automatically contain every secret or become the only recovery
path for the entire security architecture.

The important question is not only whether the vault is encrypted, but also:

- who can unlock it;
- from which devices;
- through which recovery methods;
- what happens if the account is unavailable;
- what happens if the endpoint is compromised;
- and whether a second independent recovery path exists.

---

## 6.3 KeePassXC

KeePassXC provides a local password-vault model.

Potential advantages:

- local control;
- offline access;
- transparent file storage;
- compatibility with encrypted storage;
- reduced dependence on one hosted provider;
- ability to maintain separate vaults.

Potential risks:

- loss or corruption of the vault file;
- weak master password;
- unsafe backups;
- accidental duplication;
- stale copies;
- exposed key files;
- insecure synchronization;
- loss of access if the recovery design is incomplete.

A KeePassXC vault should have:

- a strong unique master password;
- carefully controlled storage;
- encrypted backups;
- tested recovery;
- version awareness;
- clear separation between primary and backup copies;
- and a defined process for changing the vault password.

---

## 6.4 The “First Password Unlocks Nothing” Model

One of the strongest ideas discussed was that the first password alone should not
unlock important access.

The conceptual model is:

```text
First password
      +
YubiKey
      +
Second password or additional secret
      =
usable access
```

This reduces the value of stealing the first password alone.

However, the model must be implemented carefully.

Questions to verify:

- What exactly does the first password unlock?
- Is the second password genuinely independent?
- Can recovery bypass the YubiKey?
- Can an attacker reset the second factor through email?
- Are both secrets stored or entered on the same compromised device?
- Does one service still act as a single point of failure?
- Are the two factors exposed in the same session?

The principle is valuable, but the real security depends on the implementation and
recovery path.

---

## 6.5 Password-Manager Separation

Using several password tools can improve compartmentalization, but it can also
create confusion.

Each tool should have an explicit purpose.

Example:

```text
heylogin
= authentication-oriented access and selected credentials

Proton Pass
= synchronized everyday password management and aliases

KeePassXC
= local or offline-controlled vault

OnlyKey
= protected password entry

YubiKey
= hardware authentication
```

The tools should not all contain identical copies of every credential unless this
is a deliberate backup decision.

Uncontrolled duplication increases:

- the number of places that need updating;
- the chance of stale passwords;
- the number of exposed vaults;
- recovery complexity;
- uncertainty about which copy is authoritative.

A useful rule is:

> Every credential should have one clearly defined authoritative location and
> a deliberate backup strategy.

---

# 7. Encrypted Storage

## 7.1 Samsung T7 Touch

A Samsung T7 Touch has been documented as encrypted mobile storage.

Potential uses:

- encrypted project backups;
- offline document storage;
- transfer of protected files;
- recovery material;
- local archives;
- portable Memory Bank copies.

The device should be treated as a physical security object.

Important practices:

- do not leave it connected unnecessarily;
- keep backups separate from the primary device;
- protect it from loss and unauthorized access;
- verify that encryption is actually enabled;
- test restoration;
- avoid storing unencrypted copies elsewhere;
- do not use it as the only copy of important data.

---

## 7.2 Cryptomator

Cryptomator has been discussed for encrypting files before they are stored in
cloud services.

Its conceptual role is:

```text
Local file
      ↓
Cryptomator encryption layer
      ↓
Encrypted cloud storage
```

This provides an additional protection layer when the cloud provider should not
be trusted with readable file contents.

Important distinctions:

- encryption protects confidentiality;
- authentication protects access to the service;
- integrity protection helps detect unauthorized modification;
- backups protect availability;
- key management determines whether recovery is possible.

Cryptomator does not remove the need for:

- strong account authentication;
- safe vault passwords;
- secure endpoint use;
- backups;
- file-integrity awareness;
- careful sharing practices.

---

## 7.3 VeraCrypt

VeraCrypt has been documented as a tool for encrypted containers or storage.

Its potential role is:

- offline archives;
- sensitive project material;
- portable encrypted containers;
- compartmentalized files;
- local backups.

A VeraCrypt container should be managed with care.

Important considerations:

- use a strong unique password;
- keep more than one protected copy if the data matters;
- do not store the password with the container;
- test opening the container before relying on it;
- understand whether the container is mounted;
- unmount it when not in use;
- avoid leaving sensitive files in temporary locations;
- consider metadata and backup copies.

Encrypted storage protects the contents, but not necessarily:

- filenames outside the container;
- file existence;
- recent file lists;
- temporary files;
- mounted contents;
- screenshots;
- copied excerpts;
- cloud synchronization metadata.

---

## 7.4 BitLocker and USB Encryption

BitLocker and automatic USB encryption have appeared in the wider workplace
security discussions.

The principle is:

> Portable storage should not be trusted simply because it is physically inside
> a company or personal environment.

USB storage should ideally have:

- enforced encryption;
- controlled use;
- clear ownership;
- malware scanning;
- logging where appropriate;
- data classification;
- secure disposal;
- and a process for lost devices.

The goal is to prevent a lost USB device from becoming a complete data breach.

---

# 8. Recovery Architecture

Recovery is part of security.

A system that is extremely difficult to recover may encourage unsafe shortcuts,
such as storing backup passwords together or disabling security controls.

Recovery planning should answer:

- What happens if the primary YubiKey is lost?
- Where is the backup YubiKey?
- What happens if the primary laptop fails?
- Can the password vault be restored?
- Can encrypted files be opened?
- Where are recovery codes stored?
- Which email account controls recovery?
- Can an attacker abuse account recovery?
- What happens if a provider is unavailable?
- Which data must be available offline?
- Which data should remain inaccessible during an incident?

A recovery plan should be:

- documented;
- protected;
- tested;
- reviewed;
- and separate from ordinary daily credentials.

Do not place all recovery material in the same vault or device that it is meant
to recover.

---

# 9. Threat Model

The main threats considered in previous discussions include:

## 9.1 Phishing

Mitigations:

- YubiKey or passkey authentication;
- domain verification;
- password uniqueness;
- avoiding unexpected login prompts;
- use of password managers that reduce manual entry;
- checking the target account and browser context.

## 9.2 Stolen Password

Mitigations:

- unique generated passwords;
- hardware MFA;
- second-factor separation;
- password-manager access controls;
- monitoring sessions;
- removing old recovery methods.

## 9.3 Lost or Stolen Device

Mitigations:

- full-disk encryption;
- automatic locking;
- remote account session revocation;
- YubiKey requirements;
- device tracking where appropriate;
- separate backups;
- minimal local sensitive data.

## 9.4 Compromised Endpoint

Mitigations:

- current operating system;
- endpoint protection;
- trusted-device restrictions;
- least privilege;
- separate authentication devices;
- limited session duration;
- reviewing browser extensions;
- avoiding sensitive actions from uncertain devices.

A compromised device may observe data after decryption or after login.

No password manager or hardware key can guarantee safety if the active endpoint
is fully controlled by an attacker.

## 9.5 Provider Compromise

Mitigations:

- encryption before cloud upload;
- multiple providers or local copies;
- independent backups;
- hardware authentication;
- minimal provider-held plaintext;
- clear migration and recovery plans.

## 9.6 Social Engineering

Mitigations:

- never reveal passwords or recovery codes;
- never approve unexpected prompts;
- require independent verification;
- treat urgency as a risk signal;
- do not trust claims of emergency authority;
- keep recovery processes private.

## 9.7 Account Recovery Bypass

Recovery paths may be weaker than normal authentication.

Review:

- recovery email;
- phone numbers;
- backup codes;
- support-assisted recovery;
- trusted devices;
- active sessions;
- password-reset procedures;
- account ownership information.

A strong YubiKey setup can be undermined by a weak recovery channel.

---

# 10. High-Security Facility Mapping

The personal security system can be understood using the same trust-architecture
ideas as the High-Security Facility Concept.

| Facility concept | Personal security equivalent |
|---|---|
| Identity | Account identity and human owner |
| Actor type | Human, device, application, provider or recovery process |
| Credential | Password, YubiKey, passkey or certificate |
| Zone | Device, vault, account, storage location or service |
| Movement | Access from one system or data zone to another |
| Device context | Trusted laptop, browser, phone or unknown endpoint |
| Privilege | Read, edit, export, publish, delete or administer |
| Surveillance | Login history, alerts, audit logs and session review |
| Operational state | Normal, degraded, lost-device or incident mode |
| Revocation | Remove keys, sessions, devices or credentials |
| Recovery | Restore access after loss or failure |
| Governance | Human approval, documented rules and review |
| Resilience | Backup keys, encrypted backups and alternate access paths |

The same principle applies:

> A credential should not be evaluated separately from the context in which it
> is used.

---

# 11. Current Conceptual Stack

```text
Human owner: Psycedelic
        ↓
Primary trusted device: zBook
        ↓
ZorinOS/Linux with full-disk encryption
        ↓
Hardware and endpoint protections
        ↓
YubiKey-based authentication
        ↓
Password-management layer:
    - heylogin
    - Proton Pass
    - KeePassXC
        ↓
Protected password entry:
    - OnlyKey where appropriate
        ↓
Encrypted storage:
    - Samsung T7 Touch
    - Cryptomator
    - VeraCrypt
        ↓
Backups, recovery and review
```

This is a conceptual map, not a guarantee that every component is configured
correctly at all times.

---

# 12. Open Questions and Review Items

The following areas should be reviewed periodically:

- Which accounts support YubiKey or passkeys?
- Which accounts still depend on SMS or email recovery?
- Are both YubiKeys registered everywhere important?
- Is the backup YubiKey accessible during an emergency but protected from theft?
- Which password manager is authoritative for each credential?
- Are there duplicate or stale credentials?
- Are recovery codes stored separately and securely?
- Can the KeePassXC vault be restored?
- Can encrypted T7, Cryptomator and VeraCrypt data be opened?
- Are old browsers and devices removed from trusted lists?
- Are active sessions reviewed?
- Is the primary email account protected most strongly?
- Does any provider remain a single point of failure?
- Is any sensitive data stored unencrypted?
- Has the recovery process been tested recently?

---

# 13. Security Principles

1. Use unique credentials.
2. Prefer phishing-resistant hardware authentication.
3. Maintain a protected backup authentication path.
4. Do not rely on one device, provider or vault.
5. Separate daily access from recovery access.
6. Encrypt data at rest.
7. Encrypt sensitive data before cloud storage where appropriate.
8. Treat active endpoints as part of the threat model.
9. Review account recovery as seriously as normal login.
10. Remove stale devices, keys and sessions.
11. Use least privilege.
12. Keep credentials out of project documentation.
13. Do not confuse encryption with authentication.
14. Do not confuse authentication with authorization.
15. Do not confuse a trusted device with a safe device.
16. Prefer clear architecture over uncontrolled tool accumulation.
17. Test recovery before it is needed.
18. Record decisions, not secrets.
19. Treat security as a living system.
20. Preserve continuity without exposing sensitive material.

---

# 14. Final Summary

The overall approach is not based on one “perfect” password manager.

It is a layered personal security architecture:

```text
YubiKey
= hardware-backed authentication

OnlyKey
= protected password entry

heylogin
= authentication-oriented credential access

Proton Pass
= synchronized password management and aliases

KeePassXC
= local or offline-controlled vault

Full-disk encryption
= protection against offline device access

Cryptomator
= encrypted cloud-file layer

VeraCrypt
= encrypted local or portable containers

Samsung T7 Touch
= protected portable storage

Trusted zBook
= controlled primary working environment

Recovery planning
= continuity after loss, failure or compromise
```

The most important design decision is the separation of layers.

No single password, device, vault, provider or authentication method should be
treated as the entire security system.

> **Security is not one lock.**
>
> **It is the relationship between identity, credentials, devices, context,
> authorization, encryption, recovery and review.**
```

## My recommendation

Save it as a **private** file named:

```text
personal-security-and-credential-architecture.md
```

A good location would be a private wiki or encrypted personal repository—not the
public PsycedelicAI GitHub organization.
