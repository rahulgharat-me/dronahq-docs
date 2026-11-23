---
sidebar_position: 6
---

# Activate Users

Reactivate up to 50 previously deactivated users, identified by email address. Activation is subject to the account's user license limit.
<div class="apidocs-header">
    <div class="method put">PUT</div>
    <div class="endpoint">/api/public/users/activate</div>
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

### Request body

<table>
    <tr>
        <th>Field</th>
        <th>Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>list_user_email (required)</td>
        <td>Array of String</td>
        <td>Email addresses of the users to activate. Only the first 50 entries are processed.</td>
    </tr>
</table>

```json
{
    "list_user_email": ["jane@example.com", "john@example.com"]
}
```

### Example cURL

```bash
curl --location --request PUT '<BUILDER_URL>/api/public/users/activate' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXX' \
--data-raw '{
    "list_user_email": ["jane@example.com", "john@example.com"]
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
        <td>Request processed. The body groups the supplied emails by outcome.</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>400</td>
        <td><code>list_user_email</code> is empty</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>401</td>
        <td>Unauthorized</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>500</td>
        <td>Internal Server Error</td>
        <td>application/json</td>
    </tr>
</table>

#### Response fields

<table>
    <tr>
        <th>Field</th>
        <th>Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>users_success</td>
        <td>Array of String</td>
        <td>Emails of users that were activated.</td>
    </tr>
    <tr>
        <td>users_invalid</td>
        <td>Array of String</td>
        <td>Entries that are not valid email addresses.</td>
    </tr>
    <tr>
        <td>users_not_exist</td>
        <td>Array of String</td>
        <td>Valid emails that do not belong to any user in this account.</td>
    </tr>
    <tr>
        <td>users_failed</td>
        <td>Array of String</td>
        <td>Users for whom activation failed.</td>
    </tr>
</table>

### Sample response

200 : Success

```json
{
    "users_success": ["jane@example.com"],
    "users_invalid": ["not-an-email"],
    "users_failed": [],
    "users_not_exist": ["ghost@example.com"]
}
```
