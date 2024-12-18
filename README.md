# Home Assistant Blueprints

I have been using Home Assistant for 10+ years, it's about time I started to contribute to help other users.

If these saved you some time. Maybe think about [giving me a tip](https://ko-fi.com/chrisburrow1739)

<img src="/images/kofi_qrcode.png" href="https://ko-fi.com/chrisburrow1739" alt="Give me a tip" width="200"/>

I have these blueprints that may help some other people:

* [Cast to Google Hub](#cast-to-google-hub)
* [Immich Kiosk Mode](#immich-kiosk-mode)
* [Motion Lights - Any Brightness](#motion-lights-any)
* [Motion Lights - Sun/Lux dependant](#motion-lights-sun-lux)

## <a name="cast-to-google-hub"/> Cast to Google Hub
I created this script originally to cast my picture frame dashboard on my Google Home Hubs. But then I realised I could write it in a more generic way so it can be used with any dashboard instead.

https://github.com/chris-burrow-apps/home-assistant-blueprints/blob/main/google_home_hub/cast_to_google_home_hub.yaml

[![Add Blueprint](/images/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fchris-burrow-apps%2Fhome-assistant-blueprints%2Frefs%2Fheads%2Fmain%2Fgoogle_home_hub%2Fcast_to_google_home_hub.yaml)

![Automation Example](/images/blueprint-native-cast-dashboard.jpg)

## <a name="immich-kiosk-mode"/> Immich Kiosk Mode
With the [Cast to Google Hub](https://github.com/chris-burrow-apps/home-assistant-blueprints/blob/main/google_home_hub/cast_to_google_home_hub.yaml) I worked out you could apply this with a dashboard layout to show your immich photos instead of using Google Photos. 

Please follow the steps here: [https://github.com/chris-burrow-apps/home-assistant-blueprints/blob/main/immich/immich_kiosk_mode.md](https://github.com/chris-burrow-apps/home-assistant-blueprints/blob/main/immich/immich_kiosk_mode.md)

![Automation Example](/images/immich-home-assistant-example.png)
 
##  <a name="motion-lights-any"/> Motion Lights - Any Brightness
This is just a simple script I created to control my lights based upon motion.

https://github.com/chris-burrow-apps/home-assistant-blueprints/blob/main/google_home_hub/cast_to_google_home_hub.yaml

[![Add Blueprint](/images/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fchris-burrow-apps%2Fhome-assistant-blueprints%2Frefs%2Fheads%2Fmain%2Flights%2Fmotion_light_any.yaml)

![Automation Example](/images/blueprint-light-any.jpg)

## <a name="motion-lights-sun-lux"/> Motion Lights - Sun/Lux dependant
This is just a simple script I created to control my lights based upon motion/lux/sun rise/sun set.

https://github.com/chris-burrow-apps/home-assistant-blueprints/blob/main/lights/motion_light_sun_lux.yaml

[![Add Blueprint](/images/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fchris-burrow-apps%2Fhome-assistant-blueprints%2Frefs%2Fheads%2Fmain%2Flights%2Fmotion_light_sun_lux.yaml)

![Automation Example](/images/blueprint-light-lux.jpg)
