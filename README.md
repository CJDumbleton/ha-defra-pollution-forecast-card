# Home Assistant -<br>DEFRA UK air pollution forecast card

<img width="516" alt="defra-card" src="https://github.com/user-attachments/assets/0a779056-d905-4964-8042-cbca3c51d9a9" />

I've created a Home Assistant (lovelace) dashboard card which you can add to your own dashboard.

It uses data from DEFRA's UK air pollution RSS feed which is only available for the UK.

---
## Table of Contents
<!-- TOC -->
  * [Installation](#installation)
  * [Card](#card)
  * [Credits](#credits)
<!-- TOC -->

---

## Installation

Here are the steps to install this HACS `Kleenex Pollen Radar` integration.

* Use this button to add the [Kleenex pollen radar / Scottex](https://github.com/MarcoGos/kleenex_pollenradar) integration:
 
  [![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=MarcoGos&repository=kleenex_pollenradar&category=Integration)
* Click `Add integration`\
  Now you get one search result.
* Select the Kleenex integration
* Click on the detail page, in the right bottom corner on `Download`

Now is the integration added, but not yet installed. 
* Click this button to install the integration:

  [![Open your Home Assistant instance and start setting up a new integration.](https://my.home-assistant.io/badges/config_flow_start.svg)](https://my.home-assistant.io/redirect/config_flow_start/?domain=kleenex_pollenradar)
* Select your country and press the `Submit` button.

  <a href="images_kleenex/kleenex_setup.png">
  <img src="images_kleenex/kleenex_setup.png" alt="setup Kleenex" width="250px">
  </a>

Now you have these five new sensors.

<a href="images_kleenex/kleenex_sensors.png">
<img src="images_kleenex/kleenex_sensors.png" alt="Kleenex sensors" width=250px">
</a>

---

### Subtypes in attributes

Each sensor contains also in it's `attribute` value extra information about different subtypes and a forecast for the upcoming days.\
These subtypes are possible:

To see this subtype and forecast data use this button to go to the `Developer tools` and filter the entities on with the keyword `kleenex`.
<br>

[![Open your Home Assistant instance and show your state developer tools.](https://my.home-assistant.io/badges/developer_states.svg)](https://my.home-assistant.io/redirect/developer_states/)

Click on the image to see all the forecast and subtype details which are stored in the `attribute` data. 

<a href="images_kleenex/kleenex_forecast.png">
<img src="images_kleenex/kleenex_forecast.png" alt="Kleenex forecast" width=250px">
</a>

### Card

This presentation uses English levels and has bigger icons.\
No need to create extra helper sensors.

<a href="images_kleenex/kleenex_mushroom_presentation.png">
<img src="images_kleenex/kleenex_mushroom_presentation.png" alt="kleenex presentation with mushroom card" width="400px">
</a>

This presentation required the HACS integration [lovelace-mushroom](https://github.com/piitaya/lovelace-mushroom) to create this custom presentation.\
Install the integration via this button:

[![Open your Home Assistant instance and show the add-on store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=piitaya&repository=lovelace-mushroom&category=integration)

<details>
  <summary><b>> Click here to see the corresponding dashboard YAML code >></b></summary>

```yaml
{% raw %}
# Sourcecode by vdbrink.github.io
type: grid
cards:
  - type: custom:mushroom-template-card
    primary: |-
      Weeds:
      {% set level = states('sensor.kleenex_pollen_radar_huis_weeds')|int(0) %}
      {% if level == 0 %} None
      {% elif level <= 20 %} Low
      {% elif level <= 77 %} Moderate 
      {% elif level <= 266 %} High
      {% else %} very High
      {% endif %}
    secondary: "{{ states('sensor.kleenex_pollen_radar_huis_weeds') }} ppm"
    icon: mdi:flower-pollen
    icon_color: |-
      {% set level =
      states('sensor.kleenex_pollen_radar_huis_weeds')|int(0) %} {% if level ==
      0 %} green {% elif level <= 95 %} yellow {% elif level <= 207 %} orange  {%
      elif level <= 703 %} red {% else %} maroon {% endif %}
    layout: vertical
    entity: sensor.kleenex_pollen_radar_huis_weeds
    multiline_secondary: false
    tap_action:
      action: more-info
    layout_options:
      grid_columns: 1
      grid_rows: 2
    card_mod:
      style: |
        ha-card {
          --icon-size: 60px;
          background-color: hsla(0, 0%, 0%, 0);
        }
  - type: custom:mushroom-template-card
    primary: >-
      Grass: {% set level =
      states('sensor.kleenex_pollen_radar_huis_grass')|int(0) %} {% if level ==
      0 %} None {% elif level <= 29 %} Low {% elif level <= 60 %} Moderate  {%
      elif level <= 341 %} High {% else %} very High {% endif %}
    secondary: "{{ states('sensor.kleenex_pollen_radar_huis_grass') }} ppm"
    icon: mdi:grass
    icon_color: |-
      {% set level =
      states('sensor.kleenex_pollen_radar_huis_grass')|int(0) %} {% if level ==
      0 %} green {% elif level <= 95 %} yellow {% elif level <= 207 %} orange  {%
      elif level <= 703 %} red {% else %} maroon {% endif %}
    layout: vertical
    entity: sensor.kleenex_pollen_radar_huis_grass
    multiline_secondary: false
    tap_action:
      action: more-info
    layout_options:
      grid_columns: 1
      grid_rows: 2
    card_mod:
      style: |
        ha-card {
          --icon-size: 60px;
          background-color: hsla(0, 0%, 0%, 0);
        }
  - type: custom:mushroom-template-card
    primary: >-
      Trees: {% set level =
      states('sensor.kleenex_pollen_radar_huis_trees')|int(0) %} {% if level ==
      0 %} None {% elif level <= 95 %} Low {% elif level <= 207 %} Moderate  {%
      elif level <= 703 %} High {% else %} very High {% endif %}
    secondary: "{{ states('sensor.kleenex_pollen_radar_huis_trees') }} ppm"
    icon: mdi:tree
    icon_color: |-
      {% set level =
       states('sensor.kleenex_pollen_radar_huis_trees')|int(0) %} {% if level ==
       0 %} green {% elif level <= 95 %} yellow {% elif level <= 207 %} orange  {%
       elif level <= 703 %} red {% else %} maroon {% endif %}
    layout: vertical
    entity: sensor.kleenex_pollen_radar_huis_trees
    multiline_secondary: false
    tap_action:
      action: more-info
    layout_options:
      grid_columns: 1
      grid_rows: 2
    card_mod:
      style: |
        ha-card {
          --icon-size: 60px;
          background-color: hsla(0, 0%, 0%, 0);
        }
{% endraw %}
```
</details>

---

## Credits

I based the card on [vd Brink's Kleenex pollen radar card]([url](https://vdbrink.github.io/homeassistant/homeassistant_hacs_kleenex))
If you have any questions, please raise an issue.
