---
sidebar_position: 4
---

# Create Users

Create up to 50 users in a single request, either by registering them directly with a password or by sending them an invitation email.

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

<table>
    <tr>
        <th>Mode</th>
        <th>Behavior</th>
    </tr>
    <tr>
        <td><code>pre_register: true</code></td>
        <td>Users are created and activated immediately with the supplied <code>user_password</code> and added to the given groups. No email is sent.</td>
    </tr>
    <tr>
        <td><code>pre_register: false</code> (or omitted)</td>
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
        <td>pre_register (optional)</td>
        <td>Boolean</td>
        <td>Selects the mode described above. Default <code>false</code>.</td>
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
        <td>Email address. Must not already exist in the account.</td>
    </tr>
    <tr>
        <td>user_group_name (required)</td>
        <td>Array of String</td>
        <td>Names of existing groups to add the user to. Pass an empty array for none.</td>
    </tr>
    <tr>
        <td>user_password (pre-register mode)</td>
        <td>String</td>
        <td>Initial password for the user.</td>
    </tr>
</table>

### Example cURL

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
        <td>3</td>
        <td>A user with this email already exists in the account.</td>
    </tr>
    <tr>
        <td>6</td>
        <td>The account's user licence has expired or no seats are available.</td>
    </tr>
</table>
