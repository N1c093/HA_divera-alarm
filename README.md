# ARCHIVED
As [@fwmarcel](http://github.com/fwmarcel) created a custom integration for Home Assistant I'm going to archive my workaround. It's much easier to use his custom integration over here: https://github.com/fwmarcel/home-assistant-divera


# Home Assistant Divera Alarm

I currently noticed that there isn't any documented way on how to get the [Divera](https://www.divera247.com/) alarms into Home Assistant. Sadly I'm not able to create my own integration for it, but I show you my workaround to get the alarms into Home Assistant


## 1. Get your user accesskey from Divera
1. Login to the [Divera](https://www.divera247.com/) website
2. Open the settings
3. Inside the "debug" tab you can find your user-accesskey

![image](https://user-images.githubusercontent.com/59510296/138457798-ff1e89a6-ef7b-4f7c-ab14-0422969d16c5.png)

## 2. Create two rest sensors
See [sensor.yaml](sensor.yaml)

Replace the `<ACCESSSKEY>` three times (line 9, 34, 36) with your user-accesskey, which you noted in the first step. 

**Explanation:**

***sensor.divera_id***

The first sensor just checks, if a new alarm-id is available. This sensor is updated every 60 seconds. If you need to check more often, you can set the value down to 30 seconds.
The state of this sensor is mostly empty and only shows the alarm-id when there is an alarm which isn't archived.

***sensor.divera_alarm***

The second sensor get the details for the new alarm. To get the details for the  alarm, we need to know the current alarm-id, so we use the state from `sensor.divera_id` (from the first sensor) inside the second rest call.
This sensor is only updated automatically once a week, because we trigger the update of this sensor via the blueprint/automation. 

This sensor `sensor.divera_alarm` show the following values:

![image](https://user-images.githubusercontent.com/59510296/139063301-47bedc11-0178-43fe-91b2-35b82d8d7cec.png)

```
State: Shows the title of the alarm
Attributes:  
  text: return the text of the alarm. Inside the app this attribute is called "Nachricht"
  address: address of the alarm location
  new: Was the alarm already read inside divera app?
  lat: GPS latitude of the alarm location
  lng: GPS longitude of the alarm location
  closed: is the alarm already closed?
  duration: show the duration, after the alarm is closed
```

## 3. Create an automation

### Preffered/easy way: Use my prebuild blueprint


Click this link:

<a href="https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FN1c093%2FHA_divera-alarm%2Fblob%2Fmain%2Fblueprint_divera_alarm.yaml" target="_blank"><img src="https://my.home-assistant.io/badges/blueprint_import.svg" alt="Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled." /></a>

Or add the blueprint manually:

Go to **Configuration** and then **Blueprints**. Click on the blue “Import Blueprint" button in the bottom right.
Add the following URL: https://github.com/N1c093/HA_divera-alarm/blob/main/blueprint_divera_alarm.yaml

### Alternative way: Use my automation

See [automation.yaml](automation.yaml)

## Additional template sensor

If you want to display the alarm text or the alarm location in the lovelace view, you can create some template sensor.
I created two template sensor. One sensor for the alarm location and one for the alarm text.

The sensor `sensor.divera_alarm_location` can be used inside the lovelace map, so you can see the alarm location on the map.

See [configuration.yaml](configuration.yaml)


## Have fun with your Divera alarms inside Home Assistant
If there is anything unclear with my explanation or you find a problem, just open a new discussion or an issue.
