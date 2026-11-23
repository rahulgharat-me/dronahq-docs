---
sidebar_position: 5
---

# Assign or Remove Groups

Add a user to one or more groups and/or remove them from one or more groups in a single call. Groups are referenced by name.

<div class="apidocs-header">
    <div class="method put">PUT</div>
    <div class="endpoint">/api/public/users/&#123;user_id&#125;/actions/change_group</div>
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

#### Path Parameter

- `user_id` (required): The numeric id of the user. Use [List Users](./list-users.md) or [Get a User](./get-user.md) to look it up from an email address.

### Request body

At least one of `assign_group` or `remove_group` must contain a group name.

<table>
    <tr>
        <th>Field</th>
        <th>Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>assign_group (optional)</td>
        <td>Array of String</td>
        <td>Names of groups to add the user to.</td>
    </tr>
    <tr>
        <td>remove_group (optional)</td>
        <td>Array of String</td>
        <td>Names of groups to remove the user from. Pass <code>["*"]</code> to remove the user from every group.</td>
    </tr>
</table>

Group names must match existing groups in the account exactly, as shown under **Admin Console → Users → Groups** (see [Grouping users](/user-management/grouping-users)). Names that do not match any group are reported in `invalid_groups` and the rest of the request is still processed.

```json
{
    "assign_group": ["Sales", "Support"],
    "remove_group": ["Trial"]
}
```

### Example cURL

Assign and remove groups:

```bash
curl --location --request PUT '<BUILDER_URL>/api/public/users/83/actions/change_group' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXX' \
--data-raw '{
    "assign_group": ["Sales", "Support"],
    "remove_group": ["Trial"]
}'
```

Remove the user from all groups:

```bash
curl --location --request PUT '<BUILDER_URL>/api/public/users/83/actions/change_group' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXX' \
--data-raw '{
    "remove_group": ["*"]
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
        <td>Request processed. Check <code>invalid_groups</code> and <code>failed_groups</code> in the body for entries that were not applied.</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>400</td>
        <td>Both <code>assign_group</code> and <code>remove_group</code> are empty</td>
        <td>application/json</td>
    </tr>
    <tr>
        <td>401</td>
        <td>Unauthorized</td>
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

#### Response fields

<table>
    <tr>
        <th>Field</th>
        <th>Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>user_id</td>
        <td>String</td>
        <td>The user id from the request.</td>
    </tr>
    <tr>
        <td>assigned_to</td>
        <td>Array of String</td>
        <td>Group names the user was successfully added to.</td>
    </tr>
    <tr>
        <td>removed_from</td>
        <td>Array of String</td>
        <td>Group names the user was successfully removed from. Contains <code>"*"</code> when the wildcard was used.</td>
    </tr>
    <tr>
        <td>invalid_groups</td>
        <td>Array of String</td>
        <td>Group names that do not exist in the account.</td>
    </tr>
    <tr>
        <td>failed_groups</td>
        <td>Array of Integer</td>
        <td>Ids of groups where the operation failed.</td>
    </tr>
</table>

### Sample response

200 : Success

```json
{
    "user_id": "83",
    "assigned_to": ["Sales", "Support"],
    "removed_from": ["Trial"],
    "invalid_groups": [],
    "failed_groups": []
}
```

200 : One of the group names does not exist

```json
{
    "user_id": "83",
    "assigned_to": ["Sales"],
    "removed_from": [],
    "invalid_groups": ["Suport"],
    "failed_groups": []
}
```

404 : User not found

```json
{
    "error": 404,
    "message": "User not found.",
    "reason": ""
}
```
