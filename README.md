# Overview
So I’ve got a Viomi S9 robot vacuum and while I’m happy with its own app interface and notifications, I figured I can get even more out of it via integration with HA. After a couple of months of playing around, here’s what I’ve managed to achieve.

### Ditch the mihome app as I have (almost) complete control and visibility of the vacuum via HA mobile app
a) mobile vacuum dashboard

<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/45d8bdb4-a505-4d82-8799-551c3069d1c8" width="230" height="500">
<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/758e34b1-2260-4ca7-aeb8-e2e51e49e721" width="230" height="500">


b) Changing clean type to rooms, zone or point

<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/dbac5fcf-e389-4f2f-953e-ce7c3d8fb8df" width="230" height="500">

c) Changing clean mode to vacuum, vacuum & mop or just mop

<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/1862c9f1-82fe-4ec7-8513-1ce1a546e973" width="230" height="500">


d) Changing vacuum and mop options (relevant options are shown depending on clean mode)

<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/30ea4570-0e22-46d9-b775-5448f9d153b2" width="230" height="500">
<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/0382ea3b-1c51-43b0-be9b-5afa62806b3d" width="230" height="500">
<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/dca5a383-ecf5-4347-a78a-f39930084681" width="230" height="500">
<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/4a9e44b4-d890-49f4-afb3-6bad598d97a8" width="230" height="500">


### Get useful mobile (and watch!) notifications
<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/9f2d85a4-1e1d-4e26-8b8b-8ebdb96cbef4" width="230" height="500">


a) Summary of stats upon job completion including map screenshot (so I can see if the vac missed any areas)

<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/4441d236-f410-417f-a405-00ff58486121" width="230" height="500">
<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/c4a4a684-1d1a-4033-9eaa-963d1ff69c22" width="230" height="500">

<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/f7eb2a56-5763-4661-bc7c-402a9d89003a" width="20%">
<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/2a8f8f98-2bcb-4cbe-ac0e-789c0feb1111" width="21%">


b) Daily vacuum reminder - sent at 1pm only if I haven’t run the vacuum in the morning (bonus - I can start the cleaning right from the notification! Tapping the notification itself launches the Vacuum Dashboard in the HA app.)

<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/e4ea18e8-f6e5-40c5-90e9-f8dbd6111226" width="50%">


### Full control and visibility of the robot vacuum on my desktop (unlike the native Viomi interface which is mobile app only)
Since HA is accessible via the web, I now have full access to the controls and visibility of the cleaning progress while I'm on my desktop.

<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/788767c3-20b5-40c4-b9df-596d34bedc1b" width="90%">


### Monitor and control the robot vacuum from my Google Nest Hub
For the non-tech-savvy members of the household, they can access the same UI on the Google Nest Hub instead of the default basic vacuum UI. I have also created an automation helper that will show the vacuum dashboard on the Nest Hub (if its not playing other media) every time a cleaning job starts so we can monitor the cleaning process.

<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/c019eafb-a7f8-42a4-8387-c9bcad8110c5" width="50%">


### Switch between Vacuum and Mop modes automatically when mop pad is installed / removed
Speaking of automation, I also have an automation helper that will change the clean mode (i.e. vacuum or mop) automatically when the mop pad is physically installed/removed. IMHO, this is the most useful feature out of all the above as I can simply install/remove the pad and say “Hey Google, start the vacuum” knowing that the right job will run! (Before this, I almost always forget to change the mode in the app before hitting the start button)

# How did I do it?

## Dependencies
The following is the list of dependent components to get the above working. For details on how to set them up, please refer to the respective component documentation.
### Backend
- [Xiaomi Miot Auto](https://github.com/al-one/hass-xiaomi-miot) - This component is required for HA to communicate with the Viomi S9 Robot Vacuum.
- [Xiaomi Cloud Map Extractor](https://github.com/PiotrMachowski/Home-Assistant-custom-components-Xiaomi-Cloud-Map-Extractor) - This component is required to extract the floor plan and retrieve the live view of the robot cleaning progress.
### Frontend
- [Lovelace Vacuum Map card](https://github.com/PiotrMachowski/lovelace-xiaomi-vacuum-map-card) - This is the main UI component for the vacuum dashboard.
- [Card Mod 3](https://github.com/thomasloven/lovelace-card-mod) - This component is required to display the map and controls panels side-by-side in landscape view.

## Configurations
### Xiaomi Cloud Map Extractor
The following configuration goes into the main HA configuration.yaml file.
```
camera:
  - platform: xiaomi_cloud_map_extractor
    host: !secret xiaomi_vacuum_host
    token: !secret xiaomi_vacuum_token
    username: !secret xiaomi_cloud_username
    password: !secret xiaomi_cloud_password
    colors:
      color_path: [29, 47, 207] #dark blue
    room_colors:
      3: [57, 228, 237]
    draw:
      - charger
      - cleaned_area
      - goto_path
      - ignored_obstacles
      - ignored_obstacles_with_photo
      - no_go_zones
      - no_mopping_zones
      - obstacles
      - obstacles_with_photo
      - path
      - predicted_path
      - vacuum_position
      - virtual_walls
      - zones
    map_transformation:
      scale: 2
    attributes:
      - calibration_points
      - rooms
```

### Lovelace Vacuum Map card
The configuration for the vacuum card is too long to list here so I've included it [here](https://github.com/bfg100k/Viomi-S9-HA-Integration/blob/main/viomi_s9_config.yaml)

### Text Input Helper - input_text.viomi_s9_script_helper
Create an invisible text input helper with entity id `input_text.viomi_s9_script_helper`. This field is used in a number of automations for
1. Tracking the type of cleaning job (i.e. Vacuuming, Vacuum & Mop or Mopping)
2. Holding the coordinates for Zone and Point cleaning (workaround as triggering the Zone and Point cleaning for the Viomi S9 vacuum requires multiple service calls and the Lovelace Vacuum Map card component is unable to handle that)

### Automations
- [Viomi S9 - cleaning job started helper](https://github.com/bfg100k/Viomi-S9-HA-Integration/blob/main/automations/clean_job_start.yaml) - This is a helper automation to 
  1. track the cleaning job being performed (i.e. vacuuming, vacuum & mop or mopping). The info is stored in the helper variable `input_text.viomi_s9_script_helper` and is used by the `Viomi S9 - job complete and device error notification` automation.
  2. show the vacuum dashboard on kitchen display if its not playing anything else.
- [Viomi S9 - job complete and device error notifications](https://github.com/bfg100k/Viomi-S9-HA-Integration/blob/main/automations/job_complete_error.yaml) - This is a helper automation that forward notifications from the Viomi S9 vacuum. Note the use of the helper variable `input_text.viomi_s9_script_helper` to store the type of the last job performed. V2 uses the MihomeMessageSensor entity for triggering notifications which appears to be quicker and more accurate. Also note that because this entity and the other S9 entities are updated at different times, do not reference any of the other S9 entities here as the state may not be correct. E.g. when this sensor triggers for error conditions, the entity `sensor.viomi_v18_155a_device_fault` may not be updated with the error details yet. As such we will need our own translation of the error codes (which may not be a bad thing since the original messages in the system is pretty basic and cryptic).
- [Viomi S9 - zone & point cleanup helper](https://github.com/bfg100k/Viomi-S9-HA-Integration/blob/main/automations/zone_point_cleanup_helper.yaml) - This is a helper automation script to start zone or point clean up. This script relies on the helper variable `input_text.viomi_s9_script_helper` to store the zone / point coordinates. Note that a scaling factor of 1/1000 needs to be applied to the coordinates in order to work with the Viomi S9 vacuum. Note also that for zone coordinates, the UI format is (x1,y1,x2,y2) whereas the vacuum expects (x1,y1,x1,y2,x2,y2,x2,y1). Note the use of raw JSON format for sending data with the `xiaomi_miot.set_miot_property` to avoid brackets being inserted in the value field (due to HA's auto type feature).
- [Viomi S9 - change clean mode based on presence of mop pad](https://github.com/bfg100k/Viomi-S9-HA-Integration/blob/main/automations/change_clean_mode.yaml) - This is a helper automation script to automatically change the vacuum clean mode between "vacuum" and "mop" depending on whether the mop pad is installed or not. (installed = mop). Note we should not change the clean mode if cleaning is in progress (by checking for paused and cleaning states). To handle the change of mop pad in those states, add a trigger for when the vacuum comes out of those states and perform the change then.
- [Viomi S9 - Daily vacuum reminder](https://github.com/bfg100k/Viomi-S9-HA-Integration/blob/main/automations/daily_reminder.yaml) - Sends an actionable notification to parent's mobiles if the vacuum hasn't been run today.

### Scripts
- [Show vacuum dashboard on Kitchen Display](https://github.com/bfg100k/Viomi-S9-HA-Integration/blob/main/scripts/cast_vacuum_dashboard.yaml) - Simple script to cast the vacuum dashboard to the kitchen display (i.e. Google Nest Hub). This script is used in 2 places -
  1. in the `Viomi S9 - cleaning job started helper` 
  2. in a Google Home automation so I access the vacuum dashboard on kitchen display when I say "Hey Google, show vacuum dashboard".
