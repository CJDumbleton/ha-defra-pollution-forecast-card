# Home Assistant -<br>DEFRA UK air pollution forecast card

<img width="516" alt="defra-card" src="https://github.com/user-attachments/assets/0a779056-d905-4964-8042-cbca3c51d9a9" />

I've created the above Home Assistant (lovelace) dashboard card which you can add to your own dashboard.

It uses data from DEFRA's UK air pollution RSS feed which is available for over 4000 locations across the UK.

---
## Table of Contents
<!-- TOC -->
  * [Find your local measuring station](#find-your-local-measuring-station)
  * [Filter the RSS feed](#filter-the-rss-feed)
  * [Install feedparser](#install-feedparser)
  * [Install the mushroom card](#install-the-mushroom-card)
  * [Install card-mod](#install-card-mod)
  * [Card](#card)
  * [Credits](#credits)
<!-- TOC -->

---

## Find your local measuring station

- Enter your post-code on [DEFRA's UK Air Pollution forecast website]([url](https://uk-air.defra.gov.uk/forecasting/)).
- Take note of the name of the nearest station given in the table that appears. (For this example, we're going to use 'Southwark')

---

## Filter the RSS feed

[DEFRA's RSS feed]([url](https://uk-air.defra.gov.uk/assets/rss/forecast.xml)) contains over 4000 entries. You only need one.
- Go to https://siftrss.com
- Enter the DEFRA RSS feed url: https://uk-air.defra.gov.uk/assets/rss/forecast.xml and your station name in capital letters.
<img width="961" alt="siftrss" src="https://github.com/user-attachments/assets/4f1a5315-59c5-4a2d-ba63-6c5a944c350a" />
- Click Feed me!
- Take note of the filtered RSS feed. (You can also click on it to test it is working correctly.)

---

## Install feedparser

Each 

---

## Install the mushroom card

Each 

---

## Install card-mod

Each 

---

## Card

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

Buy cups of coffee for:
- I based the card on [vd Brink's Kleenex pollen radar card]([url](https://vdbrink.github.io/homeassistant/homeassistant_hacs_kleenex))
- [siftrss]([url](https://siftrss.com))
If you have any questions, please raise an issue.
