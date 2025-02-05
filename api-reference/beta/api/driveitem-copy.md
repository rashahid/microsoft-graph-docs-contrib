---
author: spgraph-docs-team
description: "Asynchronously create a copy of a driveItem (including any children) under a new parent item or with a new name."
ms.date: 09/10/2017
title: "driveItem: copy"
ms.localizationpriority: medium
ms.subservice: "sharepoint"
doc_type: apiPageType
---
# driveItem: copy

Namespace: microsoft.graph

[!INCLUDE [beta-disclaimer](../../includes/beta-disclaimer.md)]

Asynchronously create a copy of a [driveItem][item-resource] (including any children) under a new parent item or with a new name.

[!INCLUDE [national-cloud-support](../../includes/all-clouds.md)]

## Permissions

Choose the permission or permissions marked as least privileged for this API. Use a higher privileged permission or permissions [only if your app requires it](/graph/permissions-overview#best-practices-for-using-microsoft-graph-permissions). For details about delegated and application permissions, see [Permission types](/graph/permissions-overview#permission-types). To learn more about these permissions, see the [permissions reference](/graph/permissions-reference).

<!-- { "blockType": "permissions", "name": "driveitem_copy" } -->
[!INCLUDE [permissions-table](../includes/permissions/driveitem-copy-permissions.md)]

[!INCLUDE [app-permissions](../includes/sharepoint-embedded-app-driveitem-permissions.md)]
[!INCLUDE [app-permissions](../includes/sharepoint-embedded-app-permissions.md)]

## HTTP request

<!-- { "blockType": "ignored" } -->

```http
POST /drives/{driveId}/items/{itemId}/copy
POST /groups/{groupId}/drive/items/{itemId}/copy
POST /me/drive/items/{item-id}/copy
POST /sites/{siteId}/drive/items/{itemId}/copy
POST /users/{userId}/drive/items/{itemId}/copy
```
## Optional query parameters

This method supports the `@microsoft.graph.conflictBehavior` query parameter to customize the behavior when a conflict occurs.

| Value           | Description                                    |
|:----------------|:---------------------------------------------- |
| fail            | Default behavior is to report the failure.     |
| replace         | Overwrite existing item at the target site.    |
| rename          | Rename the item.                               |

>**Note:** The `conflictBehavior` parameter isn't supported for OneDrive Consumer.

## Request body

In the request body, provide a JSON object with the following parameters.

| Name            | Value                                          | Description                                                                                                 |
|:----------------|:-----------------------------------------------|:------------------------------------------------------------------------------------------------------------|
| parentReference | [ItemReference](../resources/itemreference.md) | Optional. Reference to the parent item the copy is created in.                                         |
| name            | string                                         | Optional. The new name for the copy. If this information isn't provided, the same name is used as the original.    |
| childrenOnly    | Boolean                                        | Optional. Default is `false`. If set to `true`, the children of the **driveItem** are copied but not the **driveItem** itself. Valid on folder items. |

>**Note:** The `parentReference` parameter should include the `driveId` and `id` parameters for the target folder.

## Response

Returns details about how to [monitor the progress](/graph/long-running-actions-overview) of the copy, upon accepting the request.

## Examples

### Example 1: Copy a file to a folder

The following example copies a file identified by `{item-id}` into a folder identified with a `driveId` and `id` value.
The new copy of the file is named `contoso plan (copy).txt`.

#### Request

# [HTTP](#tab/http)
<!-- { "blockType": "request", "name": "copy-item-1", "scopes": "files.readwrite", "target": "action" } -->

```http
POST https://graph.microsoft.com/beta/me/drive/items/{item-id}/copy
Content-Type: application/json

{
  "parentReference": {
    "driveId": "6F7D00BF-FC4D-4E62-9769-6AEA81F3A21B",
    "id": "DCD0D3AD-8989-4F23-A5A2-2C086050513F"
  },
  "name": "contoso plan (copy).txt"
}
```

# [C#](#tab/csharp)
[!INCLUDE [sample-code](../includes/snippets/csharp/copy-item-1-csharp-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [CLI](#tab/cli)
[!INCLUDE [sample-code](../includes/snippets/cli/copy-item-1-cli-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [Go](#tab/go)
[!INCLUDE [sample-code](../includes/snippets/go/copy-item-1-go-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [Java](#tab/java)
[!INCLUDE [sample-code](../includes/snippets/java/copy-item-1-java-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [JavaScript](#tab/javascript)
[!INCLUDE [sample-code](../includes/snippets/javascript/copy-item-1-javascript-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [PHP](#tab/php)
[!INCLUDE [sample-code](../includes/snippets/php/copy-item-1-php-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [PowerShell](#tab/powershell)
[!INCLUDE [sample-code](../includes/snippets/powershell/copy-item-1-powershell-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [Python](#tab/python)
[!INCLUDE [sample-code](../includes/snippets/python/copy-item-1-python-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

---

#### Response

The following example shows the response.

<!-- { "blockType": "response" } -->
```http
HTTP/1.1 202 Accepted
Location: https://contoso.sharepoint.com/_api/v2.0/monitor/4A3407B5-88FC-4504-8B21-0AABD3412717
```
The value of the `Location` header provides a URL for a service that returns the current state of the copy operation.
You can use this information to [determine when the copy finished](/graph/long-running-actions-overview).



### Example 2: Copy the children in a folder

The following example copies the children in a folder identified by `{item-id}` into a folder identified with a `driveId` and `id` value.
The new copy of the file is named `contoso plan (copy).txt`. The `childrenOnly` Boolean parameter is set to `true`.

#### Request 
# [HTTP](#tab/http)
<!-- { "blockType": "request", "name": "copy-item-2", "scopes": "files.readwrite", "target": "action" } -->

```http
POST https://graph.microsoft.com/beta/me/drive/items/{item-id}/copy
Content-Type: application/json

{
  "parentReference": {
    "driveId": "6F7D00BF-FC4D-4E62-9769-6AEA81F3A21B",
    "id": "DCD0D3AD-8989-4F23-A5A2-2C086050513F"
  },
  "name": "contoso plan (copy).txt",
  "childrenOnly": true
}
```

# [C#](#tab/csharp)
[!INCLUDE [sample-code](../includes/snippets/csharp/copy-item-2-csharp-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [CLI](#tab/cli)
[!INCLUDE [sample-code](../includes/snippets/cli/copy-item-2-cli-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [Go](#tab/go)
[!INCLUDE [sample-code](../includes/snippets/go/copy-item-2-go-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [Java](#tab/java)
[!INCLUDE [sample-code](../includes/snippets/java/copy-item-2-java-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [JavaScript](#tab/javascript)
[!INCLUDE [sample-code](../includes/snippets/javascript/copy-item-2-javascript-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [PHP](#tab/php)
[!INCLUDE [sample-code](../includes/snippets/php/copy-item-2-php-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [PowerShell](#tab/powershell)
[!INCLUDE [sample-code](../includes/snippets/powershell/copy-item-2-powershell-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

# [Python](#tab/python)
[!INCLUDE [sample-code](../includes/snippets/python/copy-item-2-python-snippets.md)]
[!INCLUDE [sdk-documentation](../includes/snippets/snippets-sdk-documentation-link.md)]

---

#### Response

The following example shows the response.

<!-- { "blockType": "response" } -->
```http
HTTP/1.1 202 Accepted
Location: https://contoso.sharepoint.com/_api/v2.0/monitor/4A3407B5-88FC-4504-8B21-0AABD3412717
```
The value of the `Location` header provides a URL for a service that returns the current state of the copy operation.
You can use this information to [determine when the copy finished](/graph/long-running-actions-overview).

### Remarks

In many cases, the copy action is performed asynchronously.
The response from the API indicates that the copy operation was accepted or rejected; for example, due to the destination filename already being in use.

[item-resource]: ../resources/driveitem.md

<!--
{
  "type": "#page.annotation",
  "description": "Create a copy of an existing item.",
  "keywords": "copy existing item",
  "section": "documentation",
  "tocPath": "Items/Copy",
  "suppressions": [
  ]
}
-->


