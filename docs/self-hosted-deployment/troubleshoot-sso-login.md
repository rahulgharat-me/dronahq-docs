---
sidebar_position: 98.5
---

# Troubleshoot SSO Login

When a user cannot sign in to the End User Portal (`/webapp/login`) or the Creator Portal through your identity provider
(IdP), your self-hosted instance writes a step-by-step trace of the attempt to its container log. This guide shows you
how to read that trace, what each failure code means, and what to change to fix it.

:::info VERSION REQUIREMENT

This logging is available in self-hosted DronaHQ **`3.11.3-stable` and above**, or **`3.12.1-edge` and above** on the
edge release track. On earlier versions several SSO failures produced no log line at all, so if you are on an older
build, [update to the latest version](/self-hosted-deployment/update-dronahq-to-latest) before troubleshooting.

:::

## You do not need to enable anything first

The trace is always on. Every SSO sign-in attempt already logs each step it completes and, when the sign-in is refused,
the reason it was refused. You only need the [verbose flags](#get-more-detail-with-sso_debug) when the reason alone is
not enough — typically when you suspect a claim mapping problem or you need the full stack of an unexpected error.

## Step 1: Reproduce the sign-in and capture the trace

All DronaHQ services run inside a single container, so one log stream holds the whole sign-in chain.

Follow the log, then ask the user to attempt the sign-in again:

```bash
sudo docker compose logs -f webapp | grep "sso login"
```

If you are not using Docker Compose, address the container directly:

```bash
sudo docker logs -f dronahq-self-hosted-webapp | grep "sso login"
```

Every line of the trace carries the tag `sso login`, so that filter gives you the whole flow and nothing else.

### Pin down a single attempt

Each sign-in attempt is given a short **trace id**, and every line belonging to that attempt repeats it. The same id is
also returned to the browser in the `dronahq-sso-trace` response header, which is the quickest way to tie a user's
failed attempt to the right lines in a busy log:

1. In the user's browser, open the developer tools and switch to the **Network** tab.
2. Reproduce the failed sign-in.
3. Select any of the sign-in requests and read the `dronahq-sso-trace` value from the response headers.
4. Search your log for that value:

```bash
sudo docker compose logs webapp | grep "a1b2c3d4"
```

## Step 2: Read the trace

Each line follows the same shape:

```text
[2026-11-27T11:10:10.635Z] [INFO]  sso login [a1b2c3d4] saml-parse > parse assertion - 2 claim(s) extracted
```

| Part | In the example | What it is |
| --- | --- | --- |
| Timestamp | `[2026-11-27T11:10:10.635Z]` | When the step ran, in UTC |
| Level | `[INFO]` | `INFO` for a completed step, `ERROR` for a refusal or failure |
| Tag | `sso login` | Always this, so you can filter the whole flow with `grep` |
| Trace id | `[a1b2c3d4]` | Identifies one sign-in attempt, shared by every line of that attempt |
| Stage | `saml-parse` | Which part of the sign-in this is — see the table below |
| Step | `parse assertion` | What the stage was doing at this point |
| Detail | `2 claim(s) extracted` | The outcome, or on an `ERROR` line the reason it was refused |

Every line of the trace follows this shape, whatever the stage.

A failing attempt reads as a series of completed steps followed by one `[ERROR]` line that names the cause. For example,
a sign-in refused because the claim mapping does not match what the IdP actually sends:

```text
[INFO]  sso login [a1b2c3d4] webapp > POST /idp/saml - from ip=10.20.30.40
[INFO]  sso login [a1b2c3d4] saml-acs > assertion received - SAMLResponse=present(len=6218) RelayState=okta-provider-1
[INFO]  sso login [a1b2c3d4] saml-parse > resolve provider - provider_id (RelayState)=okta-provider-1
[INFO]  sso login [a1b2c3d4] saml-parse > resolve provider - channel=ACME (channel_id=42)
[INFO]  sso login [a1b2c3d4] saml-parse > parse assertion - 1 claim(s) extracted
[ERROR] sso login [a1b2c3d4] saml-parse > map claims - no email could be mapped. The configuration expects a claim
        named "email", but the IdP sent [http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress].
```

The last line you see tells you how far the sign-in got, which narrows the problem to one stage.

:::note

The log calls your workspace a **channel**, so `channel=ACME` in a log line means the workspace named `ACME`. A
`provider_id` is the internal identifier of one SSO configuration, and for SAML it is the same value the identity
provider sends back as `RelayState`.

:::

### What each stage does

| Stage | What it is doing | A failure here means |
| --- | --- | --- |
| `webapp` | Receiving the request at the portal | The request reached DronaHQ but a handler could not complete it |
| `webapp-ajax` | Resolving which workspace the browser is signing in to | The portal could not determine the workspace, so no SSO button can be shown |
| `idp-check` | Deciding whether the email signs in with a password or an IdP, and which IdP | The user is not mapped to any IdP — check the SSO configuration and its **Restricted Domain** |
| `saml-acs` | Receiving the SAML assertion posted back by the IdP | The IdP's response did not arrive in a usable form |
| `saml-parse` | Reading the assertion and extracting the user's email, name and groups | The assertion could not be parsed, or the claim mapping does not match |
| `oauth-callback` | Exchanging the OAuth/OIDC authorization code for a token | The provider refused the exchange, or its token endpoint was unreachable |
| `token-login` | Turning the verified identity into a DronaHQ session | The identity was verified but the user is not permitted to sign in to this workspace |
| `password-login` | A username and password sign-in | Normal password sign-in failure |
| `app-render` | Opening an app after sign-in | Sign-in succeeded but the app could not be loaded |

## Step 3: Look up the code

When DronaHQ refuses a sign-in it returns a numeric code to the browser and logs that code with its meaning. Use this
table to turn the code into an action.

| Code | Meaning | What to do |
| --- | --- | --- |
| `0` | Unexpected server error | Read the `[ERROR]` line just above it for the underlying error, then enable [`SSO_DEBUG`](#get-more-detail-with-sso_debug) for the stack and send it to support |
| `1` | A required value was missing, the SSO type is unsupported, or the user's email or name could not be read from the IdP claims | Check **Add Claim List** in your SSO configuration against the claims your IdP actually sends |
| `2` | The sign-in handoff was lost, or the workspace is invalid | Ask the user to retry. If it repeats, confirm they complete the IdP login in the popup and that the server clock is correct |
| `-1` | The token from the IdP has expired | Ask the user to retry, and check for clock drift between your server and the IdP |
| `3`, `100` | The portal's own client credentials were not recognized | Contact DronaHQ support — this is not a setting you manage |
| `6` | The user has no account in this workspace and just-in-time provisioning is off, or the session could not be stored | Add the user under **Manage Users**, or enable just-in-time access for their domain |
| `7` | The user is not authorized in this workspace, their email domain is not mapped, or limited access blocked provisioning | Add the user's domain to **Restricted Domain**, or add the user to the workspace. See the sub-code table below |
| `8` | The device limit was reached, or the sign-in could not be recorded | Review the device limit for the workspace |
| `10` | The account exists but has not been verified | Activate the user — see [Activate Users](/self-hosted-deployment/users-rest-api/activate-users) |
| `19` | No workspace is mapped to the user's email domain | Add the domain to **Restricted Domain** in your SSO configuration |
| `22` | The workspace is on a developer plan and cannot be signed in to | Contact DronaHQ support to change the plan |
| `403` | Sign-in is locked for this workspace | Check your license status |
| `A`, `AD`, `D`, `L` | The license has expired | Renew or reactivate your license |

Some codes carry a sub-code with a more specific reason:

| Sub-code | Meaning |
| --- | --- |
| `RD` | The device is reported as rooted, and rooted devices are not allowed |
| `LD` | The device limit for this workspace has been reached |
| `LIC` | No user license seat is available, so no account could be created |
| `AL` | The account is locked |

## Common causes and fixes

### The claim mapping does not match what the IdP sends

**Log line:** `map claims - no email could be mapped. The configuration expects a claim named "..." but the IdP sent [...]`

This is the most common cause. The log prints both halves of the mismatch: the claim name DronaHQ is looking for, and
the claim names the IdP actually sent. Copy the correct name from the second list into **Add Claim List** against
`DHQ User Email` in `Manage Users -> SSO CONFIGURATION`, then save and activate.

Providers differ here. Microsoft Entra ID and AD FS commonly send long claim names such as
`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`, while Okta and others send a short `email`.

### The SSO configuration was never activated

**Log line:** `resolve provider - no active SSO configuration for provider_id "..."`

A configuration that is still a draft, or that was deactivated, is not used for sign-in. Open
`Manage Users -> SSO CONFIGURATION`, use **Test SSO** to confirm it works, then choose **Activate** from the
configuration's more-options menu.

:::note

If you deactivate and re-activate a configuration, confirm afterwards that it is listed as active and that its
**Restricted Domain** and claim mapping are still what you expect.

:::

### The user's email domain is not mapped

**Log line:** `resolve idp - no identity provider is mapped for this user` or `resolve channel by domain - "..." is not mapped to any channel`

DronaHQ decides which IdP to use from the domain part of the email address. Add the domain to **Restricted Domain** in
the SSO configuration. Multiple domains are comma-separated, for example `domain1.com, domain2.com`.

:::caution

Keep this list short. The stored field is limited in length, and a very long comma-separated list can be truncated,
which silently drops the domains at the end.

:::

### The account has not been verified

**Log line:** `check membership - the account ... exists but is not verified`

The user exists but has never been activated. Activate them under **Manage Users**, or through the
[Activate Users API](/self-hosted-deployment/users-rest-api/activate-users).

### The user has no account and just-in-time provisioning is off

**Log line:** `provision user - ... has no account in channel "..." and just-in-time access is off`

Either add the user to the workspace manually, or enable just-in-time access for their domain so that a first successful
SSO sign-in creates the account.

### The IdP encrypts the assertion

**Log line:** `parse assertion - the IdP sent an EncryptedAssertion`

The End User Portal sign-in reads an `Assertion` that is not encrypted. Turn assertion encryption off for the DronaHQ application
at your IdP. Request signing is unaffected — only encryption of the assertion body needs to be disabled.

### The portal is served over plain HTTP

**Log line:** `The PROVIDER_DEST cookie cannot be sent back on the cross-site IdP callback over plain HTTP`

Browsers refuse to send the cookie that records which portal the sign-in started from on a cross-site callback unless the
connection is HTTPS. Serve your deployment over HTTPS — see
[Configure SSL and CA Certificates](/self-hosted-deployment/configure-ssl-and-ca-certificates).

### No license seat is available

**Log line:** `provision user - the license has no free user seat in channel "..."` with sub-code `LIC`

Just-in-time provisioning cannot create the account because the license is fully allocated. Free a seat by deactivating
an unused user, or contact DronaHQ support to increase your license.

## Get more detail with `SSO_DEBUG`

By default the trace masks email addresses (`j***@acme.com`) and reports an error's message without its stack. Set
`SSO_DEBUG=true` to add, for the sign-in flow only:

- full email addresses
- the claim names the IdP sent, alongside the claim mapping you configured
- the list of identity providers resolved for the user
- the full stack of any unexpected error

Add it to your `dronahq.env` file:

```shell
SSO_DEBUG=true
```

Then restart the application container:

```bash
sudo docker compose stop webapp
sudo docker compose rm webapp
sudo docker compose up -d webapp
```

:::caution

`SSO_DEBUG=true` prints real email addresses to your container log. Treat the output as personal data, and set it back
to `false` once you have captured the failing sign-in.

:::

### When to use `DEBUG` instead

`DEBUG=true` is a much broader flag. It covers the whole product, not just sign-in, and additionally prints every
database statement **together with its values**. It also switches on everything `SSO_DEBUG` does, so you never need
both.

```shell
DEBUG=true
```

:::caution

`DEBUG=true` is very verbose and writes query values to the log. Use it only when `SSO_DEBUG` has not been enough, and
turn it off immediately afterwards.

:::

Both variables are listed in the
[environment variables reference](/self-hosted-deployment/environment-variables#sso_debug).

:::note

If you have set `IGNORE_CALLBACK_MESSAGES=true`, DronaHQ replaces the IdP's own error description with a generic message
on screen. The real description is still written to the log, but while you are troubleshooting it is easier to leave that
variable unset.

:::

## What to send to DronaHQ support

If the trace does not lead to a fix, send all of the following. It lets support diagnose without a second round-trip:

1. The **trace id** of a failed attempt, from the `dronahq-sso-trace` response header.
2. Every `sso login` line carrying that trace id, captured with `SSO_DEBUG=true`.
3. Your self-hosted **version**, from the `image:` tag in your `docker-compose.yml`.
4. Your **IdP and protocol** — for example Okta with SAML, or Microsoft Entra ID with OIDC.
5. Whether the sign-in is to the **End User Portal** or the **Creator Portal**.

## Related

- [Configuring SSO with SAML](/sso/configuring-sso-with-saml)
- [Configuring SSO with OIDC](/sso/configuring-sso-with-oidc)
- [Configuring SSO with custom JWT](/sso/configure-sso-with-custom-jwt)
- [Environment Variables](/self-hosted-deployment/environment-variables)
- [Audit Logging](/audit-logger/overview) — to ship these logs to an external system instead of reading the container log
