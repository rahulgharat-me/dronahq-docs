---
sidebar_position: 3
---

# Get a User

Retrieve a single user by their user id or email address.

<div class="apidocs-header">
    <div class="method get">GET</div>
    <div class="endpoint">/api/public/users/&#123;user_id_or_email&#125;</div>
</div>

#### Headers
<table>
    <tr>
        <th>Key</th>
        <th>Value</th>
    </tr>
    <tr>
        <td>Authorization</td>
        <td>Bearer &lt;Account API Key&gt;</td>
    </tr>
</table>

#### Path Parameter

- `user_id_or_email` (required): The numeric user id, or the email address of the user.

### Query parameters

<table>
    <tr>
        <th>Parameter</th>
        <th>Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>stats (optional)</td>
        <td>Boolean</td>
        <td><code>true</code> to populate the <code>stats</code> object. Applies when looking up by user id.</td>
    </tr>
    <tr>
        <td>nonce (optional)</td>
        <td>String</td>
        <td>Single sign-on nonce issued to a container app. When supplied it is validated against the requested user; an invalid nonce returns <code>403</code>.</td>
    </tr>
</table>

### Example cURL

By user id:

```bash
curl --location '<BUILDER_URL>/api/public/users/83?stats=true' \
--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXX'
```

By email:

```bash
curl --location '<BUILDER_URL>/api/public/users/jane@example.com' \
--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXX'
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
        <td>Success. Returns the user object.</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>400</td>
        <td>The path value is neither a numeric id nor a valid email address</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>401</td>
        <td>Unauthorized</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>403</td>
        <td>Invalid <code>nonce</code></td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>404</td>
        <td>User not found in this account</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>500</td>
        <td>Internal Server Error</td>
        <td>application/json</td>
    </tr>
</table>

### Sample response

200 : Success

```json
{
    "stats": {
        "status": "active",
        "available_content": 12,
        "consumed_content": 7,
        "app_timespent": 5400
    },
    "user_id": 83,
    "user_name": "Jane Doe",
    "user_email": "jane@example.com",
    "user_desg": "Sales Manager",
    "user_image_url": "",
    "channel_id": 12,
    "channel_name": "acme",
    "app_name": "Acme Corp",
    "user_reg_date": "2026-03-14 10:22:41",
    "user_last_app_activity": "2026-09-10 08:15:03",
    "user_group": [
        {
            "grp_id": 5,
            "grp_name": "Sales"
        }
    ],
    "is_admin": false
}
```

404 : User not found

```json
{
    "error": 404,
    "message": "User Not Found.",
    "reason": ""
}
```
