# Airzone Cloud

- [Airzone Cloud](#airzone-cloud)
  - [Presentation](#presentation)
    - [Abstract](#abstract)
    - [Module classes](#module-classes)
  - [Usage](#usage)
    - [Install](#install)
    - [Start api](#start-api)
    - [Get device status](#get-device-status)
    - [Get system status](#get-system-status)
    - [Get all zones status (on all devices / systems)](#get-all-zones-status-on-all-devices--systems)
    - [Control a specific zone](#control-a-specific-zone)
    - [HVAC mode](#hvac-mode)
      - [Available modes](#available-modes)
      - [Set HVAC mode on a system (and its sub-zones)](#set-hvac-mode-on-a-system-and-its-sub-zones)
  - [API doc](#api-doc)

## Presentation

### Abstract

Allow to communicate easily with Airzone Cloud to retrieve information or to send commands (on/off, temperature, HVAC mode, ...)

### Module classes

* **AirzoneCloud** : represent your AirzoneCloud account. Contains a list of your **devices** :
  * **Device** : represent one of your Airzone webserver registered. Contains a list of its **systems** :
    * **System** : represent your climate equipment (Mitsubishi, Daikin, ...). Contains a list of its **zones** :
      * **Zone** : represent a zone to control

## Usage

### Install

```bash
pip3 install AirzoneCloud
```

### Start api

```python
from AirzoneCloud import AirzoneCloud
api = AirzoneCloud("email@domain.com", "password")
```

### Get device status

```python
for device in api.devices:
    print(
        "Device name={}, status={}, id={}, mac={}, pin={}".format(
            device.name, device.status, device.id, device.mac, device.pin
        )
    )
```

Output :

<pre>
Device name=Home, status=activated, id=5bc8ae0c4149526af90c0000, mac=AA:BB:CC:DD:EE:FF, pin=1234
</pre>

### Get system status

```python
for system in api.device[0].systems:
    print(
        "System name={}, mode={}, eco={}, velocity={}, airflow={}".format(
            self.name,
            self.mode,
            self.eco,
            self.velocity,
            self.airflow,
        )
    )
```

Output :

<pre>
System name=Home, mode=heat-both, eco=eco-a, velocity=None, airflow=None
</pre>

### Get all zones status (on all devices / systems)

```python
for zone in api.all_zones:
    print(
        "Zone name={}, is_on={}, mode={}, current_temperature={}, target_temperature={}".format(
            zone.name,
            zone.is_on,
            zone.mode_name,
            zone.current_temperature,
            zone.target_temperature,
        )
    )
```

Output :

<pre>
Zone name=Baby bedroom, is_on=False, mode=heat-both, current_temperature=20.4, target_temperature=19.5
Zone name=Parents bedroom, is_on=False, mode=heat-both, current_temperature=21.1, target_temperature=17.0
Zone name=Living room, is_on=True, mode=heat-both, current_temperature=21.4, target_temperature=21.5
Zone name=Kitchen, is_on=False, mode=heat-both, current_temperature=21.2, target_temperature=19.0
</pre>

### Control a specific zone

```python
zone = api.all_zones[2]
print(zone)

# start zone
zone.turn_on()

# set temperature
zone.set_temperature(18.5)

print(zone)
```

Output :

<pre>
Zone(name=Living room, is_on=False, mode=heat-both, current_temp=21.6, target_temp=21.0)
Zone(name=Living room, is_on=True, mode=heat-both, current_temp=21.6, target_temp=18.5)
</pre>

### HVAC mode

#### Available modes

* **stop** : Stop
* **ventilate** : Ventilate
* **dehumidify** : Dry
* **heat-air** : Air heating
* **heat-radiant** : Radiant heating
* **heat-both** : Combined heating
* **cool-air** : Air cooling
* **cool-radiant** : Radiant cooling
* **cool-both** : Combined cooling

#### Set HVAC mode on a system (and its sub-zones)

```python
system = api.devices[0].systems[0]
print(system)

# set mode to heat-both
system.set_mode("heat-both")

print(system)
```

Output :

<pre>
System(name=Home, mode=stop, eco=eco-a, velocity=None, airflow=None)
System(name=Home, mode=heat-both, eco=eco-a, velocity=None, airflow=None)
</pre>

## API doc

[API full doc](blob/master/API.md)