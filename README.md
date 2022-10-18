# Create Your Own VPN Server (ShadowsocksR & V2Ray)

- [Prerequisites](https://github.com/jacob-schmitt/liberty#prerequisites)
- [Get a VPS](https://github.com/jacob-schmitt/liberty#get-a-vps)
- [Method 1: ShadowsocksR](https://github.com/jacob-schmitt/liberty#method-1-shadowsocksr)
  - [Server](https://github.com/jacob-schmitt/liberty#server)
  - [Client](https://github.com/jacob-schmitt/liberty#client)
    - [Android](https://github.com/jacob-schmitt/liberty#android)
    - [iOS](https://github.com/jacob-schmitt/liberty#ios)
- [Method 2: V2Ray](https://github.com/jacob-schmitt/liberty#method-2-v2ray)
  - [Server](https://github.com/jacob-schmitt/liberty#server-1)
  - [Client](https://github.com/jacob-schmitt/liberty#client-1)
    - [Android](https://github.com/jacob-schmitt/liberty#android-1)
- [Credits](https://github.com/jacob-schmitt/liberty#credits)

## Prerequisites
- Basic to intermediate knowledge of Linux.
- Basic to intermediate knowledge of networking.
- A Virtual Private Server (VPS) running preferably Ubuntu with **Docker** pre-installed.
- If **Docker** is not already installed on your VPS, you must do it yourself.
- Access to credit card **or** PayPal account **or** Bitcoin payment

## Get a VPS
You need to get yourself a VPS as the first step. I can recommend the following providers:
- [OVHcloud](https://www.ovhcloud.com)
  - :dollar: Starting at €3.5/month
  - :outbox_tray: 100 Mbps unlimited outbound
- [PulseHeberg](https://pulseheberg.com/)
  - :dollar: Starting at €4/month
  - :credit_card: Accepts Bitcoin payments!
  - :outbox_tray: 250 Mbps unlimited outbound
  - :white_check_mark: If you're a __student__, you can get a huge **discount**!
- [DigitalOcean](https://digitalocean.com/)
  - :dollar: Starting at $4/month
  - :outbox_tray: Starting at 512 GB of free outbound transfer
  - :white_check_mark: If you're s __student__, you can get a $200 coupon via [GitHub Student Developer Pack](https://education.github.com/pack) program valid for 1 year!
- [Vultr](https://www.vultr.com)
  - :dollar: Starting at $6/month
  - :outbox_tray: 2 TB of free outbound transfer.

Then, access your cloud server using a terminal. It is expected that you already know the how-to. So, I'm not going to go into details.

## Method 1: ShadowsocksR
### Server
- Pull the ShadowsocksR's Docker image using the following command:
```
docker pull alibo/shadowrocket-docker
```
- Run the following command to create and run a container:
```
docker run -d --restart=always -e "SSR_PORT=1443" -e "SSR_PASSWORD=12345678" -e "SSR_METHOD=chacha20-ietf" -e "SSR_OBFS=http_post_compatible" -p 80:1443 -p 80:1443/udp alibo/shadowrocket-docker
```
:warning: Make sure to change the value of the field ```SSR_PASSWORD```!

### Client
#### Android
- Download the APK file from [here](https://github.com/shadowsocksrr/shadowsocksr-android/releases/download/3.5.4/shadowsocksr-android-3.5.4.apk).
- Install the app on your phone and open it.
- Touch the "ShadowsocksR" at the top left. Then, touch the "+" sign at the bottom right corner and choose __Manual Settings__.
- Now you need to manually enter the details. Here's a quick guide:
  - Group Name: Optional
  - Profile Name: Whatever you like
  - Server: The external IP address of your VPS
  - Remote Port: `80` (By default it is set to `8388`. When you click on this number, the keyboard will pop up and you can enter `80` more easily.)
  - Local Port: `1080`
  - Password: Your chosen password (same as `SSR_PASSWORD`)
  - Encrypt Method: `chacha20-ietf`
  - Protocol: `auth_aes128_md5`
  - Protocol Param: Leave empty
  - Obfs: `http_post`
  - Obfs Param: Leave empty
  - Route: `All`
  - UDP Forwarding: `ON`
  - Leave the remaining options in their default states.
- All set! Now connect to the server using the paper rocket icon at the top right and check whether you can browse the internet.
- Inside the app, touch the "ShadowsocksR" at the top left again. You must be able to see a "Share" icon in front of your profile name. Touch it and choose __Copy URL__.
- This is the URL (starting with `ssr://`) that you should give out to your friends and family.
- Now, you should try to prepare easy-to-follow instructions for the others to install the app and import the configuration. What they'll need to do is to copy 
the URL you got from the previous step, and import it into the app using the "+" sign. They'll have to choose __Import from Clipboard__.

#### iOS
- Download the application from [here](https://apps.apple.com/us/app/potatso-lite/id1239860606). It's called **Potatso Lite**.
- Open the app and click on **Add a Proxy**.
- From the **Manual Input** section, click on **Add**. 
- Now you need to manually enter the details. Here's a quick guide:
  - Type: `ShadowsocksR`
  - Host: The external IP address of your VPS
  - Port: `80`
  - Encryption: `chacha20-ietf`
  - Password: Your chosen password (same as `SSR_PASSWORD`)
  - Remark: Whatever name you like
  - Protocol: `auth_aes128_md5`
  - Protocol Param: Leave empty
  - Obfs: `http_post`
  - Obfs Param: Leave empty
- All set! Click on **Done**. Now connect to the server using the "Play" icon at the bottom right and check whether you can browse the internet.
- Inside the app, select **Manage**. Drag the profile from right to left and choose **Share**. Now select **Copy URI to Clipboard**.
- This is the URI (starting with `ssr://`) that you should give out to your friends and family.
- Now, you should try to prepare easy-to-follow instructions for the others to install the app and import the configuration. What they'll need to do is to copy 
the URI you got from the previous step, open the app, click on **Add a Proxy**, from the **Auto Import** section choose **Proxy URI** and paste the URI there.

## Method 2: V2Ray
### Server
- Pull the V2Ray's Docker image using the following command:
```
docker pull teddysun/v2ray
```
- Create a `config.json` file in the `/etc/v2ray/`directory with the following content. If the directory doesn't exist, make one.
```json
{
  "dns": {
    "servers": [
      "1.1.1.1",
      "8.8.8.8"
    ]
  },
  "inbounds": [{
    "port": 8080,
    "protocol": "vmess",
    "settings": {
      "clients": [
        {
          "id": "18d99b5a-9069-41c0-8d3f-65c03deb005d",
          "level": 1,
          "alterId": 0,
          "security": "chacha20-poly1305"
        }
      ]
    },
    "streamSettings": {
        "network": "tcp",
        "tcpSettings": {
          "header": {
            "type": "http",
            "response": {
              "version": "1.1",
              "status": "200",
              "reason": "OK",
              "headers": {
                "Content-Type": ["application/x-msdownload", "text/html", "application/xml"],
                "Transfer-Encoding": ["chunked"],
                "Connection": ["keep-alive"],
                "Pragma": "no-cache"
              }
            }
          }
        }
      }
  }],
  "outbounds": [{
    "protocol": "freedom",
    "settings": {}
  }]
}
```
- Run the following command to create and run a container:
```
docker run -d -p 8080:8080 --name v2ray --restart=always -v /etc/v2ray:/etc/v2ray teddysun/v2ray
```
:warning: Make sure to change the `id` value under the `client` field! You can generate a random UID [here](https://www.uuidgenerator.net/).

### Client
#### Android
- Download the **v2rayNG** application from [here](https://github.com/2dust/v2rayNG/releases/download/1.7.20/v2rayNG_1.7.20.apk).
- Install the app on your phone and open it.
- At the top right, click on the "+" sign and choose **Type manually[Vmess]**.
- Now you need to manually enter the details. Here's a quick guide:
  - Remarks: Whatever name you like
  - Address: The external IP address of your VPS
  - Port: `8080`
  - `id`: Your chosen UID (same as `uid` under the field `client` in the server settings)
  - `alterId`: `0`
  - Security: `chacha20-poly1305`
  - Transport: `tcp`
  - Head type: `http`
- All set! Now connect to the server and check whether you can browse the internet.
- Inside the app, you must be able to see a "Share" icon in front of your profile name. Touch it and choose **Export to clipboard**.
- This is the URL (starting with `vmess://`) that you should give out to your friends and family.

## Credits
- [ShadowsocksR](https://github.com/shadowsocksrr/shadowsocksr)
- [ShadowsocksR's Docker Image](https://github.com/alibo/shadowrocket-docker)
- [ShadowsocksR Android App Repo](https://github.com/shadowsocksrr/shadowsocksr-android/)


