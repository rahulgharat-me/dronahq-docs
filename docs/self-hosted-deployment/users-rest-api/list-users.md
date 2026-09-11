---
sidebar_position: 2
---

# List Users

Retrieve the users of your account, newest first. Results can be filtered by group, status or a search keyword, and paginated with `ulimit` and `max_uid`.

<div class="apidocs-header">
    <div class="method get">GET</div>
    <div class="endpoint">/api/public/users</div>
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

### Query parameters

<table>
    <tr>
        <th>Parameter</th>
        <th>Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>ulimit (optional)</td>
        <td>Integer</td>
        <td>Maximum number of users to return. Default <code>25</code>.</td>
    </tr>
    <tr>
        <td>max_uid (optional)</td>
        <td>Integer</td>
        <td>Only return users whose <code>user_id</code> is lower than this value. To fetch the next page, pass the smallest <code>user_id</code> from the previous response.</td>
    </tr>
    <tr>
        <td>gid (optional)</td>
        <td>Integer</td>
        <td>Only return users that belong to this group id.</td>
    </tr>
    <tr>
        <td>search (optional)</td>
        <td>String</td>
        <td>Keyword to match against user name and email.</td>
    </tr>
    <tr>
        <td>show_stats (optional)</td>
        <td>Boolean</td>
        <td><code>true</code> to populate the <code>stats</code> object for each user.</td>
    </tr>
    <tr>
        <td>list_type (optional)</td>
        <td>String</td>
        <td>Filter by status: <code>active</code>, <code>inactive</code> or <code>moderate</code>. Returns all users when omitted.</td>
    </tr>
</table>

### Example cURL

```bash
curl --location '<BUILDER_URL>/api/public/users?ulimit=10&search=jane&list_type=active' \
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
        <td>Success. Returns an array of user objects (empty array if no users match).</td>
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

### Sample response

200 : Success

```json
[
    {
        "stats": {
            "status": "",
            "available_content": 0,
            "consumed_content": 0,
            "app_timespent": 0
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
        "user_last_app_activity": null,
        "user_group": [
            {
                "grp_id": 5,
                "grp_name": "Sales"
            },
            {
                "grp_id": 9,
                "grp_name": "Default"
            }
        ],
        "is_admin": false
    }
]
```
