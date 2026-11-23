<h1 style="border-bottom: none;">
  <img src="../assets/icon.png" width="50" valign="middle" alt="" />
  xbit
</h1>

Application introduces a fundamentally new approach to VPN connectivity. The xbit client enables VPN providers to deliver a complete end-to-end user experience, from a unified account across all devices with seamless synchronization to VPN connection, and also connected devices control; providers store; posts and notifications; payment processing.

We are committed to continuously improving and expanding the app. If you have any questions, suggestions, or feedback, feel free to contact us in our <a href="https://telegram.me/xbitsu_support"><img src="https://img.shields.io/badge/-support-blue?style=flat&logo=Telegram&logoColor=white" alt="Telegram (EN/RU)"></a>.

## Binaries

<table style="width: 100%; border-collapse: collapse; text-align: left;">
    <thead>
        <tr>
            <th style="width: 180px; text-align: left;">OS</th>
            <th style="text-align: left;">Download</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Android<br><a href="https://github.com/xbitsu/android/releases">Releases</a></td>
            <td>
                <a href="https://github.com/xbitsu/android/releases/latest/download/xbit.apk"><img src="https://img.shields.io/badge/APK-Stable-044d29.svg?logo=android" alt="APK"></a>
            </td>
        </tr>
        <tr>
            <td>Android TV<br><a href="https://github.com/xbitsu/android/releases">Releases</a></td>
            <td>
                <a href="https://github.com/xbitsu/android/releases/latest/download/xbit.apk"><img src="https://img.shields.io/badge/APK-Stable-044d29.svg?logo=android" alt="APK"></a>
            </td>
        </tr>
        <tr>
            <td>Windows<br><a href="https://github.com/xbitsu/windows/releases">Releases</a></td>
            <td>
                <a href="https://github.com/xbitsu/windows/releases/latest/download/xbit-amd64-installer.zip"><img src="https://img.shields.io/badge/EXE%20amd64-Stable-044d29.svg?logo=windows" alt="amd64"></a>
                <a href="https://github.com/xbitsu/windows/releases/latest/download/xbit-arm64-installer.zip"><img src="https://img.shields.io/badge/EXE%20arm64-Stable-044d29.svg?logo=windows" alt="arm64"></a>
            </td>
        </tr>
        <tr>
            <td>macOS<br><a href="https://github.com/xbitsu/macos/releases">Releases</a></td>
            <td>
                <a href="https://github.com/xbitsu/macos/releases/latest/download/xbit-arm64.dmg"><img src="https://img.shields.io/badge/DMG%20arm64-Stable-044d29.svg?logo=macos" alt="arm64"></a>
                <a href="https://github.com/xbitsu/macos/releases/latest/download/xbit-amd64.dmg"><img src="https://img.shields.io/badge/DMG%20amd64-Stable-044d29.svg?logo=macos" alt="amd64"></a>
            </td>
        </tr>
        <tr>
            <td>Linux<br><a href="https://github.com/xbitsu/linux/releases">Releases</a></td>
            <td>
                <a href="https://github.com/xbitsu/linux/releases/latest/download/xbit-amd64.deb"><img src="https://img.shields.io/badge/DEB%20amd64-Stable-044d29.svg?logo=linux" alt="deb amd64"></a>
                <a href="https://github.com/xbitsu/linux/releases/latest/download/xbit-arm64.deb"><img src="https://img.shields.io/badge/DEB%20arm64-Stable-044d29.svg?logo=linux" alt="deb arm64"></a>
                <a href="https://github.com/xbitsu/linux/releases/latest/download/xbit-amd64.rpm"><img src="https://img.shields.io/badge/RPM%20amd64-Stable-044d29.svg?logo=linux" alt="rpm amd64"></a>
                <a href="https://github.com/xbitsu/linux/releases/latest/download/xbit-arm64.rpm"><img src="https://img.shields.io/badge/RPM%20arm64-Stable-044d29.svg?logo=linux" alt="rpm arm64"></a>
                <a href="https://github.com/xbitsu/linux/releases/latest/download/xbit-amd64.pkg.tar.zst"><img src="https://img.shields.io/badge/ARCH%20amd64-Stable-044d29.svg?logo=linux" alt="arch amd64"></a>
                <a href="https://github.com/xbitsu/linux/releases/latest/download/xbit-arm64.pkg.tar.zst"><img src="https://img.shields.io/badge/ARCH%20arm64-Stable-044d29.svg?logo=linux" alt="arch arm64"></a>
            </td>
        </tr>
        <tr>
            <td>iOS</td>
            <td>Soon</td>
        </tr>
    </tbody>
</table>

## Providers API
A provider connects to the project by implementing the required API routes and registering its details on xbit.su.

### Response provider headers
```
Content-Type: application/json
Accept: application/json
```

### Response provider body
```json
{
  "success": [boolean|required],
  "msg": [string|nullable],
  "data": [array|object|nullable]
}
```

**success** - flag indicating whether the request succeeded or failed.
**msg** - message that should be shown to the user after the request is executed.
**data** - payload data.

### Add provider
The route must implement the standard registration on your website (bot), create a user account, optionally create a trial key, and perform any additional actions specific to the particular provider.

**Response body**
```json
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": [array|object|nullable]
}
```

**Example**
```json
{
  "success": true,
  "msg": "My VPN added",
  "data": []
}
```

### Get provider
The route must return the provider's current tariffs. xbit calls it when loading the user's providers. 

**Response body**
```json
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": {
    "tariffs": {
      "month": [
        {
          "tariff_id": [string|required],
          "tariff_type": "month",
          "header": [string|nullable],
          "description": [string|nullable],
          "tariff": [
            {
              "hwid_device_limit": [integer|required],
              "month": [integer|required],
              "traffic_gb": [integer|nullable],
              "price": [integer|required]
            }
          ]
        }
      ],
      "traffic": [
        {
          "tariff_id": [string|required],
          "tariff_type": "traffic",
          "header": [string|nullable],
          "description": [string|nullable],
          "tariff": [
            {
              "traffic_gb": [integer|required],
              "price": [integer|required]
            }
          ]
        }
      ]
    }
  }
}
```

**Example**
```json
{
  "success": true,
  "msg": "",
  "data": {
    "tariffs": {
      "month": [
        {
          "tariff_id": "for_1_device",
          "tariff_type": "month",
          "header": "1 device",
          "description": null,
          "tariff": [
            {
              "hwid_device_limit": 1,
              "month": 1,
              "traffic_gb": null,
              "price": 200
            },
            {
              "hwid_device_limit": 1,
              "month": 12,
              "traffic_gb": null,
              "price": 1680
            }
          ]
        }
      ],
      "traffic": [
        {
          "tariff_id": "extra_traffic",
          "tariff_type": "traffic",
          "header": "Traffic",
          "description": null,
          "tariff": [
            {
              "traffic_gb": 10,
              "price": 50
            }
          ]
        }
      ]
    }
  }
}
```

**month** - a time-based subscription. 
**traffic** - extra traffic. 
**tariff_id** - the same value returned by *Get subscriptions* for that subscription.

### Delete provider API
The route must implement the deletion of a user from the provider.

**Response body**
```json
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": [array|object|nullable]
}
```

**Example**
```json
{
  "success": true,
  "msg": "My VPN deleted",
  "data": []
}
```

### Get subscriptions API
The route must return the list of the user's subscriptions.

**Response body**
```json
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
        }
      ]
    }
  ]
}
```

**Example**
```json
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
        }
      ]
    }
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
```json
{
  "success": [boolean],
  "msg": [string],
  "data": [array|object|nullable]
}
```

**Example**
```json
{
  "success": true,
  "msg": "Subscription deleted",
  "data": []
}
```

### Get devices API
The route must return the list of devices on which the subscription is installed. Analogous to **Get subscriptions**.

**Response body**
```json
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
    }
  ]
}
```

**Example**
```json
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
      }
  ]
}
```

### Delete device API
The route must delete a specific user device from the subscription.

**Response body**
```json
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": {
    "deleted": [boolean]
  }
}
```

**Example**
```json
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
```json
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": {
    "link": [string]
  }
}
```

**Example**
```json
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
```json
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": {
    "link": [string]
  }
}
```

**Example**
```json
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
```json
{
  "success": [boolean],
  "msg": [string|nullable],
  "data": {
    "link": [string]
  }
}
```

**Example**
```json
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

The `add` may also contain a `link` GET parameter as a fallback method for importing key.

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
            <td>Telegram (EN/RU)</td>
            <td>
                <a href="https://telegram.me/xbitsu_support"><img src="https://img.shields.io/badge/-support-blue?style=flat&logo=Telegram&logoColor=white" alt="Support (EN/RU)"></a>
            </td>
        </tr>
    </tbody>
</table>
</div>

