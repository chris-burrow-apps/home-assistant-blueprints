# Immich Kiosk Mode

## Introduction

While converting my Google Hubs to no longer use Google Photos. I discovered an app called [Immich](https://immich.app/).
This is a community driven application that basically looks and acts like Google Photos but being able to host locally.

![Immich Homepage](/images/immich-homepage.jpg)

The learning curve I had to work out was how to display those images within a nice pretty wrapper. Which is how I then started testing Beta builds for [immich-kiosk](https://github.com/damongolding/immich-kiosk)

![Immich Kiosk Example](/images/immich-kiosk-fullscreen.jpg)

Immich-Kiosk is so nice to use; can be behind a password, add fade to your photos, time/date, so different images on different frames and so on. To help contribute, I have been going through requests and when people ask to check a build, I have been doing so and contributing my own ideas on how to improve it.
https://github.com/damongolding/immich-kiosk/discussions/95#discussioncomment-10835550

The only limitation I saw now was how to get from Immich-Kiosk to Google Home Hub. Blueprints was the only way I could see this working while making it simple for new users.

### Preparation

You need to have the following setup:

* [Home Assistant](https://www.home-assistant.io)
* [Immich](https://immich.app/)
* [immich-kiosk](https://github.com/damongolding/immich-kiosk)
* [Setup Trusted IPs](https://www.home-assistant.io/docs/authentication/providers/)

### Trusted IPs Example

Within the configuration.yml of Home Assistant, you will need to include Trusted Provider setup so when you cast dashboards, you will not need to login on every device. Here is an example setup:

```
homeassistant:
  external_url: "https://example-url.com/home-assistant"
  auth_providers:
    - type: trusted_networks
      trusted_networks:
        - 192.168.1.5/32 # Google Displays url
      trusted_users:
        192.168.1.5: abcdefghijklmnopqrstuvwxyz  # ip and user id
      allow_bypass_login: true
    - type: homeassistant
```
---
## Versions

There is 2 different ways I worked out how to set this up locally:

### DashCast
Highly configurable but that can be its downfall. It is a 3rd party add on for Home Assistant that can be linked with [BrowserMod](https://github.com/thomasloven/hass-browser_mod) or [Kiosk-mode](https://github.com/NemesisRE/kiosk-mode) which and create some crazy kiosk setups. 
Unfortunately I have found it to be quite buggy with BrowserMod when it comes to hiding the side and top navigation but I thought I would include this script anyway in case you use it.

### Native Cast functionality
Basic and simple. This will always be supported by Home Assistant but due to the way it is written, it is quite basic in its features but that can also be an advantage that no updates should hopefully be needed.

---

## Common setup

For both the versions there is a common setup of setting up a hidden dashboard showing an iframe with your immich-kiosk url. So far I have not been able to work out how to get it to self adjust to each screen size so you will have to create different dashboards for each screen size.

First create a 1 panel view with the visibility of all the users hidden. (hint, create a view and then click on the edit button at the top and click 'visibility', un-tick all the users so it's hidden from navigation)

![Home Assistant 1 Panel Setup](/images/immich-home-assistant-one-panel-config.jpg)

Within the config below, please update with your own setup:
* Immich Kiosk URL : https://example-immich-kiosk-url.com/?album=shared


* Screen size: height="600px"
  * Google Home Hub Size: 600px
  * Google Nest Hub Max Size: 800px


* Aspect ratio: aspect_ratio: '128:75'
  * This is a required config when setting up the webview. Otherwise, the view would add scroll bars and would make the frame display outside the normal view. 
  * Google Home Hub Ratio: "128:75"
  * Google Nest Hub Max Ratio: "8:5"

The next section can be entirely removed, but I thought I would include it in case you want to tap on an image on the photo frame and navigate to a different view.

* Home Assistant Logo Image: /path/to/image.jpg
  * I have included a logo from Home Assistant like I have used in the example.


* Home Assistant Dashboard Link: /lovelace/default_view
  * The URL of the dashboard you would like to navigate to once you click on the image.

```
views:
...

  - type: panel
    path: picture-frame-small
    cards:
      - type: picture-elements
        card_mod:
          style: |
            ha-card {
              background: transparent !important;
              padding: 0px !important;
              border: 0px;
              }
        elements:
          - type: custom:html-card
            card_mod:
              style: |
                #htmlCard {
                  background: transparent !important;
                  padding: 0px !important;
                  }
            style:
              position: absolute
              transform: translate(-0%, -100%)
              width: 100%
              height: 100%
            content: >
              <iframe
              src="https://example-immich-kiosk-url.com/?album=shared"
              style="border:0px #ffffff none;" name="pictureFrame"
              scrolling="no" frameborder="0" marginheight="0px"
              marginwidth="0px" height="600px" width="100%"
              allowfullscreen></iframe>
          - type: image
            image: /path/to/image.jpg
            tap_action:
              action: navigate
              navigation_path: /lovelace/default_view
            style:
              left: 90%
              top: 20%
              width: 10%
              height: 20%
              z-index: 9999
        aspect_ratio: '128:75'
    icon: mdi:panorama-variant
    subview: false
    visible: []
    title: Picture Frame Small
```

It should then look something like this

![Example Photo Frame view](/images/immich-home-assistant-example.png)

---

## Native Cast

To setup native cast within Home Assistant, you can include this blueprint.

https://github.com/chris-burrow-apps/home-assistant-blueprints/blob/main/google_home_hub/cast_to_google_home_hub.yaml

[![Add Blueprint](/images/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fchris-burrow-apps%2Fhome-assistant-blueprints%2Frefs%2Fheads%2Fmain%2Fgoogle_home_hub%2Fcast_to_google_home_hub.yaml)

![Automation Example](/images/blueprint-native-cast-dashboard.jpg)

### Native Cast - Configuration

* Google Home Hub: Which display would you like the picture frame to cast to


* Lovelace Dashboard: Path to the lovelace dashboard you would like to cast from


* Lovelace View: Name of the view you have just created


* Lovelace dashboard Name: This is the name of the view you have just created.
  * This is needed as if you navigate away from the picture frame using the home assistant button included, the hub would get stuck on the new dashboard and never show the picture frame again


* By default when casting a lovelace view, the google Home Hub displays the view at high brightness which isn't nice if it's placed in a bedroom. So I added the functionality to disable the cast and allow the hub to dim and then start again in the morning.
  * Start trigger
  * End trigger

---

## DashCast

To setup dash cast within Home Assistant, you have to first set it up from HAC or manually:
https://github.com/AlexxIT/DashCast

https://github.com/chris-burrow-apps/home-assistant-blueprints/blob/main/google_home_hub/dashcast_to_google_home_hub.yaml

[![Add Blueprint](/images/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fchris-burrow-apps%2Fhome-assistant-blueprints%2Frefs%2Fheads%2Fmain%2Fgoogle_home_hub%2Fdashcast_to_google_home_hub.yaml)

![Automation Example](/images/blueprint-dash-cast-dashboard.png)

### Dashcast - Configuration

* Google Home Hub: Which display would you like the picture frame to cast to


* Lovelace Dashboard URL: URL to the lovelace dashboard you would like to cast


* By default when casting a lovelace view, the google Home Hub displays the view at high brightness which isn't nice if it's placed in a bedroom. So I added the functionality to disable the cast and allow the hub to dim and then start again in the morning.
  * Start trigger
  * End trigger

### Dashcast - Bonus setup

Next install one of these plugins to add extra functionality:
* [Browser Mod 2](https://github.com/thomasloven/hass-browser_mod)
* [Kiosk-mode](https://github.com/NemesisRE/kiosk-mode)

By default, using dashcast, it will show everything not just the view you want to show. This includes the sidebar & top bar which allows the user to navigate anywhere. Instead you could use one of these plugins and hide everything or add custom functionality. 