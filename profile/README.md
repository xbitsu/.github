<h1 style="border-bottom: none;">
  <img src="../assets/icon.png" width="50" valign="middle" alt="" />
  xbit
</h1>

Application introduces a fundamentally new approach to VPN connectivity. The xbit client enables VPN providers to deliver a complete end-to-end user experience, from a unified account across all devices with seamless synchronization to VPN connection and payment processing.

We are committed to continuously improving and expanding the app. If you have any questions, suggestions, or feedback, feel free to contact us in our Telegram chat.

Accepted just JSON format subscriptions.

## Binaries

<div align="left">
<table>
    <thead align="left">
        <tr>
            <th>OS</th>
            <th>Download</th>
        </tr>
    </thead>
    <tbody align="left">
        <tr>
            <td>Android (<a href="https://github.com/xbitsu/android/releases">Releases</a>)</td>
            <td>
                <a href="https://github.com/xbitsu/android/releases/latest/download/xbit.apk"><img src="https://img.shields.io/badge/APK-Stable-044d29.svg?logo=android" alt="APK Stable"></a>
            </td>
        </tr>
        <tr>
            <td>Android TV (<a href="https://github.com/xbitsu/android/releases">Releases</a>)</td>
            <td>
                <a href="https://github.com/xbitsu/android/releases/latest/download/xbit.apk"><img src="https://img.shields.io/badge/APK-Stable-044d29.svg?logo=android" alt="APK Stable"></a>
            </td>
        </tr>
        <tr>
            <td>Windows (<a href="https://github.com/xbitsu/windows/releases">Releases</a>)</td>
            <td>
                <a href="https://github.com/xbitsu/windows/releases/latest/download/xbit-amd64-installer.zip"><img src="https://img.shields.io/badge/Setup-x64-2d7d9a.svg?logo=windows" alt="Setup x64"></a>
                <a href="https://github.com/xbitsu/windows/releases/latest/download/xbit-arm64-installer.zip"><img src="https://img.shields.io/badge/Setup-arm64-2d7d9a.svg?logo=windows" alt="Setup arm64"></a>
            </td>
        </tr>
    </tbody>
</table>
</div>

## Providers API

To connect to the project, a provider must contact the project support and implement certain API routes.

### Response provider headers
```
Content-Type: application/json
Accept: application/json
```

### Response provider body
```
{
  "success": [boolean|required],
  "msg": [string|nullable],
  "data": [array|object|nullable]
}
```

**success** - flag indicating whether the request succeeded or failed.

**msg** - message that should be shown to the user after the request is executed.

**data** - payload data.

### Add provider API
The route must implement the standard registration on your website (bot), create a user account, optionally create a trial key, and perform any additional actions specific to the particular provider.

**Response body**
```
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": [array|object|nullable]
}
```

**Example**
```
{
  "success": true,
  "msg": "My VPN added",
  "data": []
}
```

### Delete provider API
The route must implement the deletion of a user from the provider.

**Response body**
```
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": [array|object|nullable]
}
```

**Example**
```
{
  "success": true,
  "msg": "My VPN deleted",
  "data": []
}
```

### Get subscriptions API
The route must return the list of the user's subscriptions.

**Response body**
```
{
  "success": [boolean],
  "msg": [string],
  "data": [
    {
      "url": [string|required],
      "header": [string|nullable],
      "tariff_id": [string|required],
      "devices": [
        {
          [
            "hwid": [string|nullable],
            "platform": [string|nullable],
            "os_version": [string|nullable],
            "device_model": [string|nullable],
            "user_agent": [string|nullable],
            "created_at": [string|nullable],
            "updated_at": [string|nullable]
          ]
        },
        ...
      ]
    },
    ...
  ]
}
```

**Example**
```
{
  "success": true,
  "msg": "",
  "data": [
    {
      "url": "https://sub.mydomain.com/xxxxxxxxx",
      "header": "Subscription",
      "tariff_id": "for_1_device",
      "devices": [
        {
          [
            "hwid": "123",
            "platform": "Android",
            "os_version": "13",
            "device_model": "SM-A325F",
            "user_agent": "Some user-agent",
            "created_at": "2026-06-19 14:09:12",
            "updated_at": "2026-06-20 08:19:28"
          ]
        },
        ...
      ]
    },
    ...
  ]
}
```

**Subscription (profile) response headers**
```
profile-title: Server Title
subscription-userinfo: upload=999; download=999; total=999; expired=946663200
```

**tariff_id** - your unique tariff ID.

**subscription-userinfo** - upload, download, total - bytes, expired - timestamp.

### Delete subscription API
The route must implement the deletion of each individual subscription.

**Response body**
```
{
  "success": [boolean],
  "msg": [string],
  "data": [array|object|nullable]
}
```

**Example**
```
{
  "success": true,
  "msg": "Subscription deleted",
  "data": []
}
```

### Get devices API
The route must return the list of devices on which the subscription is installed. Analogous to **Get subscriptions**.

**Response body**
```
{
  "success": [boolean],
  "msg": [string],
  "data": [
    {
      [
        "hwid": [string|nullable],
        "platform": [string|nullable],
        "os_version": [string|nullable],
        "device_model": [string|nullable],
        "user_agent": [string|nullable],
        "created_at": [string|nullable],
        "updated_at": [string|nullable]
      ]
    },
    ...
  ]
}
```

**Example**
```
{
  "success": true,
  "msg": "",
  "data": [
    {
        [
          "hwid": "123",
          "platform": "Android",
          "os_version": "13",
          "device_model": "SM-A325F",
          "user_agent": "Some user-agent",
          "created_at": "2026-06-19 14:09:12",
          "updated_at": "2026-06-20 08:19:28"
        ]
      },
      ...
  ]
}
```

### Delete device API
The route must delete a specific user device from the subscription.

**Response body**
```
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": {
    "deleted": [boolean]
  }
}
```

**Example**
```
{
  "success": true,
  "msg": "Device deleted",
  "data": [
    "deleted": true
  ]
}
```

### Buy subscription API
The route must implement the purchase of a new subscription and return a link to the invoice page.

**Response body**
```
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": {
    "link": [string]
  }
}
```

**Example**
```
{
  "success": true,
  "msg": "",
  "data": [
    "link": "https://mypaymentservice.com/xxxxxxxxx"
  ]
}
```

### Extend subscription API
The route must implement the extension of a subscription and return a link to the invoice page.

**Response body**
```
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": {
    "link": [string]
  }
}
```

**Example**
```
{
  "success": true,
  "msg": "",
  "data": [
    "link": "https://mypaymentservice.com/xxxxxxxxx"
  ]
}
```

### Buy traffic API
The route must implement the purchase of traffic and return a link to the invoice page.

**Response body**
```
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": {
    "link": [string]
  }
}
```

**Example**
```
{
  "success": true,
  "msg": "",
  "data": [
    "link": "https://mypaymentservice.com/xxxxxxxxx"
  ]
}
```

## URL schema

Add provider from shop to user.
```
xbit://add/<provider_id>
```

Add subscription without shop to user.
```
xbit://import?link=<subsciption_url>
```

## Contact us

<div align="left">
<table>
    <thead align="left">
        <tr>
            <th>Platform</th>
            <th>Contact</th>
        </tr>
    </thead>
    <tbody align="left">
        <tr>
            <td>Telegram (RU/EN)</td>
            <td>
                <a href="https://telegram.me/xbitsu_support"><img src="https://img.shields.io/badge/-support-blue?style=flat&logo=Telegram&logoColor=white" alt="Support (RU/EN)"></a>
            </td>
        </tr>
    </tbody>
</table>
</div>

