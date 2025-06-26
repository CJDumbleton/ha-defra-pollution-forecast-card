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
  * [Create a sensor](#create-a-sensor)
  * [Install the mushroom card](#install-the-mushroom-card)
  * [Install card-mod](#install-card-mod)
  * [Restart home assistant](#restart-home-assistant)
  * [Card](#card)
  * [Credits](#credits)
<!-- TOC -->

---

## Find your local measuring station

- Enter your post-code on [DEFRA's UK Air Pollution forecast website](https://uk-air.defra.gov.uk/forecasting/).
- Take note of the name of the nearest station given in the table that appears. (For this example, we're going to use 'Southwark')

---

## Filter the RSS feed

[DEFRA's RSS feed](https://uk-air.defra.gov.uk/assets/rss/forecast.xml) contains over 4000 entries. You only need one.
- Go to https://siftrss.com
- Enter the DEFRA RSS feed url: https://uk-air.defra.gov.uk/assets/rss/forecast.xml and your station name in capital letters.
<img width="961" alt="siftrss" src="https://github.com/user-attachments/assets/4f1a5315-59c5-4a2d-ba63-6c5a944c350a" />
- Click Feed me!
- Take note of the filtered RSS feed.
- Click on the filtered URL to check it contains only one entry. (If more than one entry contains your station name, you may need to use the 'matches regex' option in siftrss instead.)

---

## Install feedparser

Install [feedparser](https://github.com/custom-components/feedparser) into Home Assistant if you don't already have it.

---

## Create a sensor

In your `configuration.yaml` file, add the following sensor substituting in your filtered feed_url.

```yaml
sensor:
  - platform: feedparser
    name: Defra air quality forecast
    # Used siftrss to filter https://uk-air.defra.gov.uk/assets/rss/forecast.xml
    # to show only Southwark
    feed_url: https://siftrss.com/f/P19Z3RPQXLW
    date_format: '%a, %b %d %I:%M %p'
    scan_interval:
      hours: 8
```

---

## Install the mushroom card

Install the [mushroom card](https://github.com/piitaya/lovelace-mushroom) into Home Assistant if you don't already have it. 

---

## Install card-mod

Install [card mod](https://github.com/thomasloven/lovelace-card-mod) into Home Assistant if you don't already have it.  

---

## Restart home assistant

Restart home assistant.

---

## Card

<img width="516" alt="defra-card" src="https://github.com/user-attachments/assets/0a779056-d905-4964-8042-cbca3c51d9a9" />

Add a new manual card to your dashboard, delete the default text and paste in the following YAML code.

<details>
  <summary><b>> Click here to see the dashboard YAML code >></b></summary>

```yaml
# Sourcecode by CJDumbleton
square: true
type: grid
cards:
  - type: custom:mushroom-template-card
    primary: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {{ summary | regex_findall_index(find='\w\w\w', index=-5,
      ignorecase=False) }}
    icon: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-5,
      ignorecase=False) | int %}

      {% set icon = {
        1: 'mdi:numeric-1',
        2: 'mdi:numeric-2',
        3: 'mdi:numeric-3',
        4: 'mdi:numeric-4',
        5: 'mdi:numeric-5',
        6: 'mdi:numeric-6',
        7: 'mdi:numeric-7',
        8: 'mdi:numeric-8',
        9: 'mdi:numeric-9',
        10: 'mdi:numeric-10'} %}
      {{ icon.get(index, 'mdi:cloud-question' ) }}
    icon_color: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-5,
      ignorecase=False) | int %}

      {% set color = {
        1: 'teal',
        2: 'green',
        3: 'lime',
        4: 'yellow',
        5: 'amber',
        6: 'orange',
        7: 'red',
        8: 'pink',
        9: 'brown',
        10: 'purple'} %}
      {{ color.get(index, 'grey' ) }}
    layout: vertical
    entity: sensor.defra_air_quality_forecast
    multiline_secondary: false
    tap_action:
      action: url
      url_path: https://uk-air.defra.gov.uk/forecasting/?postcode=York%20racecourse
    layout_options:
      grid_columns: 1
      grid_rows: 2
    secondary: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-5,
      ignorecase=False) | int %}

      {% set level = {
        1: 'Low',
        2: 'Low',
        3: 'Low',
        4: 'Moderate',
        5: 'Moderate',
        6: 'Moderate',
        7: 'High',
        8: 'High',
        9: 'High',
        10: 'Very high'} %}
      {{ level.get(index, 'Unknown' ) }}
    card_mod:
      style: |
        ha-card {
          --icon-size: 60px;
          background-color: hsla(0, 0%, 0%, 0);
        }
  - type: custom:mushroom-template-card
    primary: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {{ summary | regex_findall_index(find='\w\w\w', index=-4,
      ignorecase=False) }}
    icon: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-4,
      ignorecase=False) | int %}

      {% set icon = {
        1: 'mdi:numeric-1',
        2: 'mdi:numeric-2',
        3: 'mdi:numeric-3',
        4: 'mdi:numeric-4',
        5: 'mdi:numeric-5',
        6: 'mdi:numeric-6',
        7: 'mdi:numeric-7',
        8: 'mdi:numeric-8',
        9: 'mdi:numeric-9',
        10: 'mdi:numeric-10'} %}
      {{ icon.get(index, 'mdi:cloud-question' ) }}
    icon_color: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-4,
      ignorecase=False) | int %}

      {% set color = {
        1: 'teal',
        2: 'green',
        3: 'lime',
        4: 'yellow',
        5: 'amber',
        6: 'orange',
        7: 'red',
        8: 'pink',
        9: 'brown',
        10: 'purple'} %}
      {{ color.get(index, 'grey' ) }}
    layout: vertical
    entity: sensor.defra_air_quality_forecast
    multiline_secondary: false
    tap_action:
      action: url
      url_path: https://uk-air.defra.gov.uk/forecasting/?postcode=York%20racecourse
    layout_options:
      grid_columns: 1
      grid_rows: 2
    secondary: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-4,
      ignorecase=False) | int %}

      {% set level = {
        1: 'Low',
        2: 'Low',
        3: 'Low',
        4: 'Moderate',
        5: 'Moderate',
        6: 'Moderate',
        7: 'High',
        8: 'High',
        9: 'High',
        10: 'Very high'} %}
      {{ level.get(index, 'Unknown' ) }}
    card_mod:
      style: |
        ha-card {
          --icon-size: 60px;
          background-color: hsla(0, 0%, 0%, 0);
        }
  - type: custom:mushroom-template-card
    primary: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {{ summary | regex_findall_index(find='\w\w\w', index=-3,
      ignorecase=False) }}
    icon: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-3,
      ignorecase=False) | int %}

      {% set icon = {
        1: 'mdi:numeric-1',
        2: 'mdi:numeric-2',
        3: 'mdi:numeric-3',
        4: 'mdi:numeric-4',
        5: 'mdi:numeric-5',
        6: 'mdi:numeric-6',
        7: 'mdi:numeric-7',
        8: 'mdi:numeric-8',
        9: 'mdi:numeric-9',
        10: 'mdi:numeric-10'} %}
      {{ icon.get(index, 'mdi:cloud-question' ) }}
    icon_color: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-3,
      ignorecase=False) | int %}

      {% set color = {
        1: 'teal',
        2: 'green',
        3: 'lime',
        4: 'yellow',
        5: 'amber',
        6: 'orange',
        7: 'red',
        8: 'pink',
        9: 'brown',
        10: 'purple'} %}
      {{ color.get(index, 'grey' ) }}
    layout: vertical
    entity: sensor.defra_air_quality_forecast
    multiline_secondary: false
    tap_action:
      action: url
      url_path: https://uk-air.defra.gov.uk/forecasting/?postcode=York%20racecourse
    layout_options:
      grid_columns: 1
      grid_rows: 2
    secondary: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-3,
      ignorecase=False) | int %}

      {% set level = {
        1: 'Low',
        2: 'Low',
        3: 'Low',
        4: 'Moderate',
        5: 'Moderate',
        6: 'Moderate',
        7: 'High',
        8: 'High',
        9: 'High',
        10: 'Very high'} %}
      {{ level.get(index, 'Unknown' ) }}
    card_mod:
      style: |
        ha-card {
          --icon-size: 60px;
          background-color: hsla(0, 0%, 0%, 0);
        }
  - type: custom:mushroom-template-card
    primary: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {{ summary | regex_findall_index(find='\w\w\w', index=-2,
      ignorecase=False) }}
    icon: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-2,
      ignorecase=False) | int %}

      {% set icon = {
        1: 'mdi:numeric-1',
        2: 'mdi:numeric-2',
        3: 'mdi:numeric-3',
        4: 'mdi:numeric-4',
        5: 'mdi:numeric-5',
        6: 'mdi:numeric-6',
        7: 'mdi:numeric-7',
        8: 'mdi:numeric-8',
        9: 'mdi:numeric-9',
        10: 'mdi:numeric-10'} %}
      {{ icon.get(index, 'mdi:cloud-question' ) }}
    icon_color: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-2,
      ignorecase=False) | int %}

      {% set color = {
        1: 'teal',
        2: 'green',
        3: 'lime',
        4: 'yellow',
        5: 'amber',
        6: 'orange',
        7: 'red',
        8: 'pink',
        9: 'brown',
        10: 'purple'} %}
      {{ color.get(index, 'grey' ) }}
    layout: vertical
    entity: sensor.defra_air_quality_forecast
    multiline_secondary: false
    tap_action:
      action: url
      url_path: https://uk-air.defra.gov.uk/forecasting/?postcode=York%20racecourse
    layout_options:
      grid_columns: 1
      grid_rows: 2
    secondary: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-2,
      ignorecase=False) | int %}

      {% set level = {
        1: 'Low',
        2: 'Low',
        3: 'Low',
        4: 'Moderate',
        5: 'Moderate',
        6: 'Moderate',
        7: 'High',
        8: 'High',
        9: 'High',
        10: 'Very high'} %}
      {{ level.get(index, 'Unknown' ) }}
    card_mod:
      style: |
        ha-card {
          --icon-size: 60px;
          background-color: hsla(0, 0%, 0%, 0);
        }
  - type: custom:mushroom-template-card
    primary: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {{ summary | regex_findall_index(find='\w\w\w', index=-1,
      ignorecase=False) }}
    icon: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-1,
      ignorecase=False) | int %}

      {% set icon = {
        1: 'mdi:numeric-1',
        2: 'mdi:numeric-2',
        3: 'mdi:numeric-3',
        4: 'mdi:numeric-4',
        5: 'mdi:numeric-5',
        6: 'mdi:numeric-6',
        7: 'mdi:numeric-7',
        8: 'mdi:numeric-8',
        9: 'mdi:numeric-9',
        10: 'mdi:numeric-10'} %}
      {{ icon.get(index, 'mdi:cloud-question' ) }}
    icon_color: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-1,
      ignorecase=False) | int %}

      {% set color = {
        1: 'teal',
        2: 'green',
        3: 'lime',
        4: 'yellow',
        5: 'amber',
        6: 'orange',
        7: 'red',
        8: 'pink',
        9: 'brown',
        10: 'purple'} %}
      {{ color.get(index, 'grey' ) }}
    layout: vertical
    entity: sensor.defra_air_quality_forecast
    multiline_secondary: false
    tap_action:
      action: url
      url_path: https://uk-air.defra.gov.uk/forecasting/?postcode=York%20racecourse
    layout_options:
      grid_columns: 1
      grid_rows: 2
    secondary: >-
      {% set summary =
      state_attr('sensor.defra_air_quality_forecast','entries')[0].summary %}

      {% set index = summary | regex_findall_index(find='\d+', index=-1,
      ignorecase=False) | int %}

      {% set level = {
        1: 'Low',
        2: 'Low',
        3: 'Low',
        4: 'Moderate',
        5: 'Moderate',
        6: 'Moderate',
        7: 'High',
        8: 'High',
        9: 'High',
        10: 'Very high'} %}
      {{ level.get(index, 'Unknown' ) }}
    card_mod:
      style: |
        ha-card {
          --icon-size: 60px;
          background-color: hsla(0, 0%, 0%, 0);
        }
title: DEFRA air pollution 5-day forecast
columns: 5
```
</details>

---

## Credits

Buy coffees for:
- I based the card on [vd Brink's Kleenex pollen radar card](https://vdbrink.github.io/homeassistant/homeassistant_hacs_kleenex)
- [siftrss](https://siftrss.com)
- [feedparser](https://github.com/custom-components/feedparser)
- [Mushroom card](https://github.com/piitaya/lovelace-mushroom)
- [Card mod](https://github.com/thomasloven/lovelace-card-mod)
