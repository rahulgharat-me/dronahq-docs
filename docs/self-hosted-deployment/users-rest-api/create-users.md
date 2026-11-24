---
sidebar_position: 4
---

# Create Users

Create up to 50 users in a single request: as SSO users, by registering them directly with a password, or by sending them an invitation email.

<div class="apidocs-header">
    <div class="method post">POST</div>
    <div class="endpoint">/api/public/users</div>
</div>

#### Headers
<table>
    <tr>
        <th>Key</th>
        <th>Value</th>
    </tr>
    <tr>
        <td>Content-Type</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>Authorization</td>
        <td>Bearer &lt;Account API Key&gt;</td>
    </tr>
</table>

### Modes

`auth_type` selects how the users will sign in. Within the default `password` type, `pre_register` then selects whether a password is set for them or they are emailed an invitation.

<table>
    <tr>
        <th>Mode</th>
        <th>Behavior</th>
    </tr>
    <tr>
        <td><code>auth_type: "sso"</code></td>
        <td>Users are created as SSO users and added to the given groups. They sign in through your identity provider. No password is set and no email is sent.</td>
    </tr>
    <tr>
        <td><code>auth_type: "password"</code> (or omitted) with <code>pre_register: true</code></td>
        <td>Users are created and activated immediately with the supplied <code>user_password</code> and added to the given groups. No email is sent.</td>
    </tr>
    <tr>
        <td><code>auth_type: "password"</code> (or omitted) with <code>pre_register: false</code> (or omitted)</td>
        <td>Users receive an invitation email and set their own password from the activation link. They are added to the given groups on activation.</td>
    </tr>
</table>

:::caution
Pre-registering users with preset passwords should only be used where invitation emails cannot be delivered. See [Adding users to your account](/user-management/adding-users-to-your-account) for guidance.
:::

### Request body

<table>
    <tr>
        <th>Field</th>
        <th>Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>auth_type (optional)</td>
        <td>String</td>
        <td><code>"sso"</code> or <code>"password"</code>. Default <code>"password"</code>. Any other value returns <code>400</code>.</td>
    </tr>
    <tr>
        <td>pre_register (optional)</td>
        <td>Boolean</td>
        <td>Selects the mode described above. Default <code>false</code>. Ignored when <code>auth_type</code> is <code>"sso"</code>.</td>
    </tr>
    <tr>
        <td>invitee_user (required)</td>
        <td>Array</td>
        <td>1 to 50 user objects to create (see below).</td>
    </tr>
    <tr>
        <td>admin_id (invite mode)</td>
        <td>Integer</td>
        <td>User id of the administrator sending the invitation.</td>
    </tr>
    <tr>
        <td>admin_name (invite mode)</td>
        <td>String</td>
        <td>Name of the administrator, shown in the invitation email.</td>
    </tr>
</table>

#### `invitee_user` object

<table>
    <tr>
        <th>Field</th>
        <th>Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>user_name (required)</td>
        <td>String</td>
        <td>Full name of the user.</td>
    </tr>
    <tr>
        <td>user_email (required)</td>
        <td>String</td>
        <td>Email address. Must not already exist in the account. In SSO mode the domain must belong to a configured identity provider.</td>
    </tr>
    <tr>
        <td>user_group_name (required)</td>
        <td>Array of String</td>
        <td>Names of existing groups to add the user to. Pass an empty array for none.</td>
    </tr>
    <tr>
        <td>user_password (pre-register mode)</td>
        <td>String</td>
        <td>Initial password for the user. Must <b>not</b> be sent when <code>auth_type</code> is <code>"sso"</code>.</td>
    </tr>
</table>

### SSO mode

With `auth_type: "sso"` each user is created as an SSO user — verified, active, with no password — and added to the groups you name. They can sign in through your identity provider straight away and already hold the right group memberships, so you can onboard everyone from your own admin application instead of using the **Manage Users** section of Studio.

Requirements and behavior:

- SSO must be configured for the account. If no identity provider is configured, the whole request is rejected with `400`, because the users created would otherwise be unable to sign in through any path.
- Each `user_email` domain must match a domain on one of the configured identity providers — the same check the **Manage Users** screen applies. Users whose domain does not match are reported with `error_code` `8`, and the rest of the batch is still processed.
- `user_password` is rejected rather than ignored (`error_code` `9`), so a caller never assumes a password was set when none exists.
- Groups are given by name in `user_group_name` and must already exist. Create them under **Users → Groups** first — see [Grouping users](/user-management/grouping-users). Names that do not match an existing group are skipped.
- No invitation email is sent. Nothing has to be delivered to the user for onboarding to complete.

:::info
SSO users created this way have no password at all, which is what keeps them on the identity provider for sign-in. If you need a user to sign in with a password instead, create them with `auth_type: "password"`.
:::

### Example cURL

Create SSO users with groups pre-assigned:

```bash
curl --location --request POST '<BUILDER_URL>/api/public/users' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXX' \
--data-raw '{
    "auth_type": "sso",
    "invitee_user": [
        {
            "user_name": "Jane Doe",
            "user_email": "jane@example.com",
            "user_group_name": ["Sales", "Support"]
        },
        {
            "user_name": "John Smith",
            "user_email": "john@example.com",
            "user_group_name": ["Sales"]
        }
    ]
}'
```

Pre-register a user:

```bash
curl --location --request POST '<BUILDER_URL>/api/public/users' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXX' \
--data-raw '{
    "pre_register": true,
    "invitee_user": [
        {
            "user_name": "Jane Doe",
            "user_email": "jane@example.com",
            "user_password": "Str0ngPassw0rd!",
            "user_group_name": ["Sales", "Support"]
        }
    ]
}'
```

Invite users by email:

```bash
curl --location --request POST '<BUILDER_URL>/api/public/users' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXX' \
--data-raw '{
    "pre_register": false,
    "admin_id": 1,
    "admin_name": "Admin",
    "invitee_user": [
        {
            "user_name": "Jane Doe",
            "user_email": "jane@example.com",
            "user_group_name": ["Sales"]
        },
        {
            "user_name": "John Smith",
            "user_email": "john@example.com",
            "user_group_name": []
        }
    ]
}'
```

### Responses
<table>
    <tr>
        <th>Status Code</th>
        <th>Description</th>
        <th>Response</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Request processed. The body lists the users that could <b>not</b> be created; an empty array means every user was created.</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>400</td>
        <td><code>auth_type</code> is not <code>sso</code> or <code>password</code>, or <code>auth_type</code> is <code>sso</code> and the account has no identity provider configured</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>401</td>
        <td>Unauthorized, or <code>invitee_user</code> is empty</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>500</td>
        <td>Internal Server Error</td>
        <td>application/json</td>
    </tr>
</table>

### Sample response

200 : All users created

```json
[]
```

200 : Some users could not be created

```json
[
    {
        "user_email": "jane@example.com",
        "error_code": "3",
        "error_detail": "User already exists in the channel"
    },
    {
        "user_email": "john@example.com",
        "error_code": "6",
        "error_detail": "Licence expired"
    }
]
```

#### Error codes

<table>
    <tr>
        <th>error_code</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>2</td>
        <td>Not a valid email address.</td>
    </tr>
    <tr>
        <td>3</td>
        <td>A user with this email already exists in the account.</td>
    </tr>
    <tr>
        <td>6</td>
        <td>The account's user licence has expired or no seats are available.</td>
    </tr>
    <tr>
        <td>8</td>
        <td>SSO mode only. The email domain is not configured for SSO in this account.</td>
    </tr>
    <tr>
        <td>9</td>
        <td>SSO mode only. <code>user_password</code> was supplied, which is not allowed for an SSO user.</td>
    </tr>
    <tr>
        <td>10</td>
        <td>SSO mode only. The user could not be created as an SSO user — most often because the email already exists in another account, where the account-wide SSO setting cannot be changed for it.</td>
    </tr>
</table>
