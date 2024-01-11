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
For the non-tech-savy members of the household, they can access the same UI on the Google Nest Hub instead of the default basic vacuum UI. I have also created an automation helper that will show the vacuum dashboard on the Nest Hub (if its not playing other media) every time a cleaning job starts so we can monitor the cleaning process.

<img src="https://github.com/bfg100k/Viomi-S9-HA-Integration/assets/2188087/c019eafb-a7f8-42a4-8387-c9bcad8110c5" width="50%">


### Switch between Vacuum and Mop modes automatically when mop pad is installed / removed
Speaking of automation, I also have an automation helper that will change the clean mode (i.e. vacuum or mop) automatically when the mop pad is physically installed/removed. IMHO, this is the most useful feature out of all the above as I can simply install/remove the pad and say “Hey Google, start the vacuum” knowing that the right job will run! (Before this, I almost always forget to change the mode in the app before hitting the start button)

# How did I do it?

## Dependencies
Xiaomi Miot Auto
Xiaomi Cloud Map Extractor
Xiaomi Vacuum Map Card
Card Mod
