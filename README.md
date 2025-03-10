# Lovelace Soft UI 

[![License is MIT](https://img.shields.io/github/license/N-l1/home-assistant-config?style=flat-square)](https://github.com/N-l1/lovelace-soft-ui/blob/master/LICENSE) 
[![This is new user_friendly](https://img.shields.io/badge/new%20user-friendly-brightgreen?style=flat-square&)](#) 
[![The maintainer is N-l1](https://img.shields.io/badge/maintainer-N--l1-blue?style=flat-square)](https://github.com/N-l1)

**Hey there!** First and foremost, thank you for finding your way to my Home Assistant repo. Here you will find my custom Neumorphic/Soft UI syled Lovelace (UI of Home Assistant). I hope you like it!

**Click** [**here**](docs/inspiration.md) **for some examples and inspiration** 

**Don't hesitate to ask for help [**here**](https://community.home-assistant.io/t/lovelace-soft-ui-simple-and-clean-lovelace-configuration) on the Home Assistant forum** 
  
![Lovelace Soft UI light theme](docs/images/soft_ui_light.jpg)
![Lovelace Soft UI dark theme](docs/images/soft_ui_dark.jpg)

# Let's do it!

### Alternatives
**[@Savjee](https://github.com/Savjee)'s [Button Text Card](https://github.com/Savjee/button-text-card).** If you are only looking to implement this style on a button, this is the way. It is very easy to install and set up. The downside, however, is that you will not be able to implement this style on any other card.

**[@KTibow](https://github.com/KTibow)'s [dark](https://github.com/KTibow/lovelace-dark-soft-ui-theme/) and [light](https://github.com/KTibow/lovelace-light-soft-ui-theme/) Soft UI themes.** If you are looking for a quick and simple way to implement this style universally to all your cards, this is the way. KTibow's themes are easier to implement, faster to set up, and will still work with any of the custom cards inside this repo. However, using the way described in this repo provides more flexibility and customizability. 

### 1. Install card-mod
OK so you've decided to redo your UI, don't worry, your UI will look as great as the screenshots in 5 simple steps! First of all, you will need to install [**card-mod**](https://github.com/thomasloven/lovelace-card-mod). It is a custom card available on [HACS](https://hacs.xyz) (the Home Assistant Community Store). Please read HACS's [documentation](https://hacs.xyz) and install it.

### 2. sun.sun
For the cards to switch automatically to dark/light themes, please make sure you have the `sun.sun` entity (should come preinstalled). If you don't have it, add the following to your `configuration.yaml`.

```yaml
# Example configuration.yaml entry
sun:
```
### 3. Themes
This style works best with custom themes. You can also find and install them with [HACS](https://hacs.xyz). **Please note that themes with pure white/black backgrounds will not work.** Light themes with a milky white background work well, and dark themes with a dark grey background work well. 

The themes used in the screenshots of this repo are the [Clear](https://github.com/naofireblade/clear-theme) and [Slate](https://github.com/seangreen2/slate_theme) theme by [**@naofireblade**](https://github.com/naofireblade) and [**@seangreen2**](https://github.com/seangreen2) (they are both available on HACS). If you decide to use the Clear theme, please make sure to remove the `lovelace-background` line from the theme YAML.

### 4. Automation
You will need to set up an automation to automatically switch to a dark theme at sunset and back to a light theme at sunrise. If you don't have one, please add the following to your `automations.yaml`.

<details><summary><b>Show code</b></summary>

```yaml
# Example automations.yaml entry
- id: set_theme
  alias: Set Theme
  trigger:
  - platform: homeassistant
    event: start
  - platform: state
    entity_id: sun.sun
  action:
  - choose:
    - conditions:
      - condition: state
        entity_id: sun.sun
        state: "above_horizon"
      sequence:
      - service: frontend.set_theme
        data:
# Change this to the name of your light theme
          name: Name of your light theme
    - conditions:
      - condition: state
        entity_id: sun.sun
        state: "below_horizon"
      sequence:
      - service: frontend.set_theme
        data:
# Change this to the name of your light theme
          name: Name of your dark theme
```
</details>

### 5. Done!
We are done! Use this code to add the Soft UI style to any card. 

```yaml
# Example entry
style: |
  ha-card {
      background-color: var(--primary-background-color);
      border-radius: 15px;
      margin: 10px;
      box-shadow: 
        {% if is_state('sun.sun', 'above_horizon') %}
          -8px -8px 8px 0 rgba(255,255,255,.5),8px 8px 8px 0 rgba(0,0,0,.03);
        {% elif is_state('sun.sun', 'below_horizon') %}
          -8px -8px 8px 0 rgba(50, 50, 50,.5),8px 8px 8px 0 rgba(0,0,0,.15);
        {% endif %}   
   }      
```

# Advanced Usage
Here are some cards I created using this style. All cards are added using the UI. Click on the three dots on the top right, go to `Configure UI`, then click on the `+` on the bottom right, and select `Manual`. Paste in the appropriate code for each card.

## Compact Header
<p align="left">
  <img src="docs/images/slate_theme_with_colored_header.png" alt="Slate theme with color header" width="425">
  <img src="docs/images/slate_theme.png" alt="Slate theme without color header" width="425">
  <br/>
  <a href="https://github.com/seangreen2/slate_theme"><b>Slate theme</b></a> header with and without color.
</p>

This makes the original Home Assistant header "compact" while also matching it with the background color. To add this card, click on the three dots on the top right, then go to `Configure UI` then `Raw config editor` and add the following to the top:

**Required Custom Cards:**

* [**Custom Header**](https://maykar.github.io/custom-header), by [**@maykar**](https://github.com/maykar)


<details><summary><b>Show code</b></summary>
  
```yaml
# Example entry
custom_header:
  # Makes header compact
  compact_mode: true
  # Makes background transparent
  background: transparent
  # Hide help entry
  hide_help: true
  # Makes tabs the same color as the text color
  elements_color: var(--primary-text-color)
  # Makes line under selected tab color same as sidebar selected panel color
  tab_indicator_color: var(--sidebar-selected-icon-color)
  # Makes selected tab color same as sidebar selected panel color
  active_tab_color: var(--sidebar-selected-icon-color)
  # Make mobile view notification dot color same as selected panel color
  notification_dot_color: var(--sidebar-selected-icon-color)
```
</details>

## Text Header Card
<p align="left">
  <img src="docs/images/text_header_card_light.png" alt="Text header card light theme" width="425">
  <img src="docs/images/text_header_card_dark.png" alt="Text header card dark theme" width="425">
  <br/>
</p>

This card displays texts with transparent background.

**Required Custom Cards:**

* [**Card Mod**](https://github.com/thomasloven/lovelace-card-mod), by [**@thomasloven**](https://github.com/thomasloven)

<details><summary><b>Show code</b></summary>

```yaml
# Example entry
content: |
  # Enter what you want to display here
style:
  .: |
    ha-card {
      --ha-card-background: none !important;
      box-shadow: none !important;
    }
  ha-markdown:
    $: |
      h1 {
        font-size: 20px;
        font-weight: bold;
        font-family: Helvetica;
        letter-spacing: '-0.01em';
      }
type: markdown
```
</details>

## Text Header Card with Subheader

<p align="left">
  <img src="docs/images/text_subheader_card_light.png" alt="Text subheader card light theme" width="425">
  <img src="docs/images/text_subheader_card_dark.png" alt="Text subheader card dark theme" width="425">
  <br/>
</p>

This card displays texts with smaller texts underneath with transparent background.

**Required Custom Cards:**

* [**Card Mod**](https://github.com/thomasloven/lovelace-card-mod), by [**@thomasloven**](https://github.com/thomasloven)

<details><summary><b>Show code</b></summary>

```yaml
# Example entry
cards:
  - content: |
      # Enter what you want to display here
    style:
      .: |
        ha-card {
          --ha-card-background: none !important;
          box-shadow: none !important;
          height: 20px; 
        }
      ha-markdown:
        $: |
          h1 {
            font-size: 20px;
            font-weight: bold;
            font-family: Helvetica;
            letter-spacing: '-0.01em';
          }
    type: markdown
  - content: |
      # Enter what you want to display in the small text
    style:
      .: |
        ha-card {
          --ha-card-background: none !important;
          box-shadow: none !important;
          height: 50px; 
        }
      ha-markdown:
        $: |
          h1 {
            font-size: 15px;
            font-weight: thin;
            font-family: Helvetica;
            letter-spacing: '-0.01em';
          }
    type: markdown
type: vertical-stack
```
</details>

## Vertical Buttons Card
<p align="left">
  <img src="docs/images/vertical_button_card_light.png" alt="Vertical button card light theme" width="425">
  <img src="docs/images/vertical_button_card_dark.png" alt="Vertical button card dark theme" width="425">
  <br/>
</p>

This card contains three buttons lined up vertically: lights, alarm clock, and irrigation. Each of the buttons will redirect you to a Lovelace tab with the corresponding name, i.e. lovelace/lights. You can change the icons, names, tap action, and texts beside them. 

Wondering how the card is able to count how many lights are on? Here is the template. To use this template as a light count sensor, use the Home Assistant [template sensor component](https://www.home-assistant.io/integrations/template/). 

```
{{ states | selectattr('entity_id','in', ['light.list_your_lights_here','switch.example_1','switch.example_2'] )|selectattr('state','eq','on') | list | count }}
```

**Required Custom Cards:**

* [**Button Card**](https://github.com/custom-cards/button-card), by [**@RomRider**](https://github.com/RomRider)
* [**Card Mod**](https://github.com/thomasloven/lovelace-card-mod), by [**@thomasloven**](https://github.com/thomasloven)

<details><summary><b>Show code</b></summary>

```yaml
# Example entry
cards:
  - cards:
      - cards:
# Icon to display - First button
          - icon: 'mdi:lightbulb-multiple'
            show_icon: true
            show_name: false
            style: |
              ha-card {
                margin: 10px;
                box-shadow: 
                  {% if is_state('sun.sun', 'above_horizon') %}
                    -8px -8px 8px 0 rgba(255,255,255,.5),8px 8px 8px 0 rgba(0,0,0,.03);
                  {% elif is_state('sun.sun', 'below_horizon') %}
                    -8px -8px 8px 0 rgba(50, 50, 50,.5),8px 8px 8px 0 rgba(0,0,0,.15);
                  {% endif %}                 
              }
            styles:
              card:
                - width: 80px
                - height: 80px
                - border-radius: 15px
                - background-color: var(--primary-background-color)
              icon:
                - color: var(--primary-text-color)
# Action to perform - First button
            tap_action:
              action: navigate
              navigation_path: /lovelace/lights
              haptic: light
            type: 'custom:button-card'
          - cards:
# Big text to display - First button       
              - content: |
                  # Lights
                style:
                  .: |
                    ha-card {
                      --ha-card-background: none !important;
                      box-shadow: none !important;
                      height: 20px; 
                      margin-top: 15px;
                    }
                  ha-markdown:
                    $: |
                      h1 {
                        font-size: 20px;
                        font-weight: bold;
                        font-family: Helvetica;
                        letter-spacing: '-0.01em';
                      }
                type: markdown
# Small text to display - First button             
              - content: >              
                  # There are  {% if is_state('sensor.lights_on', '0') %}
                  currently no  {% else %}  {{states('sensor.lights_on')}}  {%
                  endif %} lights on
                style:
                  .: |
                    ha-card {
                      --ha-card-background: none !important;
                      box-shadow: none !important;
                    }
                  ha-markdown:
                    $: |
                      h1 {
                        font-size: 15px;
                        font-weight: thin;
                        font-family: Helvetica;
                        letter-spacing: '-0.01em';
                      }
                type: markdown                   
            type: vertical-stack
        type: horizontal-stack
      - cards:
# Icon to display - Second button
          - icon: 'mdi:alarm'
            show_icon: true
            show_name: false
            style: |
              ha-card {
                margin: 10px;
                box-shadow: 
                  {% if is_state('sun.sun', 'above_horizon') %}
                    -8px -8px 8px 0 rgba(255,255,255,.5),8px 8px 8px 0 rgba(0,0,0,.03);
                  {% elif is_state('sun.sun', 'below_horizon') %}
                    -8px -8px 8px 0 rgba(50, 50, 50,.5),8px 8px 8px 0 rgba(0,0,0,.15);
                  {% endif %}    
              }
            styles:
              card:
                - width: 80px
                - height: 80px
                - border-radius: 15px
                - background-color: var(--primary-background-color)
              icon:
                - color: var(--primary-text-color)
# Action to perform - Second button
            tap_action:
              action: navigate         
              navigation_path: /lovelace/alarm
              haptic: light
            type: 'custom:button-card'
          - cards:
# Big text to display - Second button    
              - content: |           
                  # Alarm clock
                style:
                  .: |
                    ha-card {
                      --ha-card-background: none !important;
                      box-shadow: none !important;
                      height: 20px; 
                      margin-top: 15px;
                    }
                  ha-markdown:
                    $: |
                      h1 {
                        font-size: 20px;
                        font-weight: bold;
                        font-family: Helvetica;
                        letter-spacing: '-0.01em';
                      }
                type: markdown
# Small text to display - Second button   
              - content: >            
                  # The weekday alarm is  {% if
                  is_state('input_boolean.sleep_cycle_weekday', 'on') and
                  is_state('input_boolean.alarm_weekday_enabled', 'on')%}  on
                  sleep cycle mode   {% elif
                  is_state('input_boolean.alarm_weekday_enabled', 'on') %}   set
                  for {{states('sensor.alarm_weekday_time')}}  {% else %}  not
                  set  {% endif %}
                style:
                  .: |
                    ha-card {
                      --ha-card-background: none !important;
                      box-shadow: none !important;
                    }
                  ha-markdown:
                    $: |
                      h1 {
                        font-size: 15px;
                        font-weight: thin;
                        font-family: Helvetica;
                        letter-spacing: '-0.01em';
                      }
                type: markdown  
            type: vertical-stack
        type: horizontal-stack
      - cards:
# Icon to display - Third button         
          - icon: 'mdi:pine-tree'
            show_icon: true
            show_name: false
            style: |
              ha-card {
                margin: 10px;
                box-shadow: 
                  {% if is_state('sun.sun', 'above_horizon') %}
                    -8px -8px 8px 0 rgba(255,255,255,.5),8px 8px 8px 0 rgba(0,0,0,.03);
                  {% elif is_state('sun.sun', 'below_horizon') %}
                    -8px -8px 8px 0 rgba(50, 50, 50,.5),8px 8px 8px 0 rgba(0,0,0,.15);
                  {% endif %}   
              }
            styles:
              card:
                - width: 80px
                - height: 80px
                - border-radius: 15px
                - background-color: var(--primary-background-color)
              icon:
                - color: var(--primary-text-color)
# Action to perform - Third button
            tap_action:
              action: navigate
              navigation_path: /lovelace/sprinklers/
              haptic: light
            type: 'custom:button-card'
          - cards:
# Big text to display - Third button
              - content: |             
                  # Irrigation
                style:
                  .: |
                    ha-card {
                      --ha-card-background: none !important;
                      box-shadow: none !important;
                      height: 20px; 
                      margin-top: 15px;
                    }
                  ha-markdown:
                    $: |
                      h1 {
                        font-size: 20px;
                        font-weight: bold;
                        font-family: Helvetica;
                        letter-spacing: '-0.01em';
                      }
                type: markdown
# Small text to display - Third button      
              - content: |          
                  # The irrigation system is not activated
                style:
                  .: |
                    ha-card {
                      --ha-card-background: none !important;
                      box-shadow: none !important;
                    }
                  ha-markdown:
                    $: |
                      h1 {
                        font-size: 15px;
                        font-weight: thin;
                        font-family: Helvetica;
                        letter-spacing: '-0.01em';
                      }
                type: markdown  
            type: vertical-stack
        type: horizontal-stack
    type: vertical-stack
type: vertical-stack
```
</details>

## Horizontal Buttons Card
<p align="left">
  <img src="docs/images/horizontal_button_card_light.png" alt="Horizontal button card light theme" width="425">
  <img src="docs/images/horizontal_button_card_dark.png" alt="Horizontal button card dark theme" width="425">
  <br/>
</p>

This card is five buttons lined up horizontally. When the state of the entity is 'on', the button will be pressed in. When the entity is 'off' the button will be released (like normal). 

**Required Custom Cards:**

* [**Button Card**](https://github.com/custom-cards/button-card), by [**@RomRider**](https://github.com/RomRider)
* [**Card Mod**](https://github.com/thomasloven/lovelace-card-mod), by [**@thomasloven**](https://github.com/thomasloven)

<details><summary><b>Show code</b></summary>

```yaml
# Example entry
cards:
# Entity to control - First button
  - entity: light.family_room_lamp
# Icon to display - First button
    icon: 'mdi:lamp'
    show_icon: true
    show_name: false
    state:
      - styles:
          icon:
            - color: 'var(--paper-item-icon-active-color)  '
        value: 'on'
# Please change the entities inside to match the entity to be controlled
    style: |
      ha-card {
        margin: 5px;  
        margin-left: 6.5px;
        box-shadow: 
         {% if is_state('sun.sun', 'above_horizon') and is_state('light.family_room_lamp', 'on') %}
           inset -4px -4px 8px 0 rgba(255,255,255,.5), inset 4px 4px 8px 0 rgba(0,0,0,.03);
         {% elif is_state('sun.sun', 'above_horizon') and is_state('light.family_room_lamp', 'off') %}                      
           -5px -5px 8px 0 rgba(255,255,255,.5),5px 5px 8px 0 rgba(0,0,0,.03);
         {% elif is_state('sun.sun', 'below_horizon') and is_state('light.family_room_lamp', 'on') %}                      
           inset -4px -4px 10px 0 rgba(50, 50, 50,.5), inset 4px 4px 12px 0 rgba(0,0,0,.3); 
         {% elif is_state('sun.sun', 'below_horizon') and is_state('light.family_room_lamp', 'off') %}   
           -5px -5px 8px 0 rgba(50, 50, 50,.5),5px 5px 8px 0 rgba(0,0,0,.15);
         {% endif %}                      
      }
      @media only screen and (max-width: 600px) {
         ha-card {
           margin: 3px; 
           margin-left: 4px;                   
         }
      }
      @media only screen and (min-width: 1200px) {
         ha-card {
           margin: 8px;  
           margin-left: 11px;               
         }
      }
    styles:
      card:
        - width: 60px
        - height: 60px
        - border-radius: 15px
        - background-color: var(--primary-background-color)
      icon:
        - color: var(--primary-text-color)
    tap_action:
      action: toggle
      haptic: light      
    hold_action: 
      action: more-info
      haptic: medium
    type: 'custom:button-card'
# Entity to control - Second button
  - entity: switch.kitchen_island_light
# Icon to display - Second button
    icon: 'mdi:vanity-light'
    show_icon: true
    show_name: false
    state:
      - styles:
          icon:
            - color: 'var(--paper-item-icon-active-color)  '
        value: 'on'
# Please change the entities inside to match the entity to be controlled
    style: |
      ha-card {
        margin: 5px;  
        margin-left: 6.5px;
        box-shadow: 
          {% if is_state('sun.sun', 'above_horizon') and is_state('switch.kitchen_island_light', 'on') %}
            inset -4px -4px 8px 0 rgba(255,255,255,.5), inset 4px 4px 8px 0 rgba(0,0,0,.03);
          {% elif is_state('sun.sun', 'above_horizon') and is_state('switch.kitchen_island_light', 'off') %}                      
            -5px -5px 8px 0 rgba(255,255,255,.5),5px 5px 8px 0 rgba(0,0,0,.03);
          {% elif is_state('sun.sun', 'below_horizon') and is_state('switch.kitchen_island_light', 'on') %}                      
            inset -4px -4px 10px 0 rgba(50, 50, 50,.5), inset 4px 4px 12px 0 rgba(0,0,0,.3); 
          {% elif is_state('sun.sun', 'below_horizon') and is_state('switch.kitchen_island_light', 'off') %}   
            -5px -5px 8px 0 rgba(50, 50, 50,.5),5px 5px 8px 0 rgba(0,0,0,.15);
          {% endif %}                      
      }
      @media only screen and (max-width: 600px) {
         ha-card {
           margin: 3px; 
           margin-left: 4px;                   
         }
      }
      @media only screen and (min-width: 1200px) {
         ha-card {
           margin: 8px;  
           margin-left: 11px;               
         }
      }
    styles:
      card:
        - width: 60px
        - height: 60px
        - border-radius: 15px
        - background-color: var(--primary-background-color)
      icon:
        - color: var(--primary-text-color)
    tap_action:
      action: toggle
      haptic: light      
    hold_action: 
      action: more-info
      haptic: medium
    type: 'custom:button-card'
# Entity to control - Third button
  - entity: switch.dining_area
# Icon to display - Third button
    icon: 'mdi:ceiling-light'
    show_icon: true
    show_name: false
    state:
      - styles:
          icon:
            - color: 'var(--paper-item-icon-active-color)  '
        value: 'on'
# Please change the entities inside to match the entity to be controlled  
    style: |
      ha-card {
        margin: 5px;                
        box-shadow: 
          {% if is_state('sun.sun', 'above_horizon') and is_state('switch.dining_area', 'on') %}
            inset -4px -4px 8px 0 rgba(255,255,255,.5), inset 4px 4px 8px 0 rgba(0,0,0,.03);
          {% elif is_state('sun.sun', 'above_horizon') and is_state('switch.dining_area', 'off') %}                      
            -5px -5px 8px 0 rgba(255,255,255,.5),5px 5px 8px 0 rgba(0,0,0,.03);
          {% elif is_state('sun.sun', 'below_horizon') and is_state('switch.dining_area', 'on') %}                      
            inset -4px -4px 10px 0 rgba(50, 50, 50,.5), inset 4px 4px 12px 0 rgba(0,0,0,.3); 
          {% elif is_state('sun.sun', 'below_horizon') and is_state('switch.dining_area', 'off') %}   
            -5px -5px 8px 0 rgba(50, 50, 50,.5),5px 5px 8px 0 rgba(0,0,0,.15);
          {% endif %}                           
      }
      @media only screen and (max-width: 600px) {
         ha-card {
           margin: 3px; 
           margin-left: 4px;                   
         }
      }
      @media only screen and (min-width: 1200px) {
         ha-card {
           margin: 8px;  
           margin-left: 11px;               
         }
      }
    styles:
      card:
        - width: 60px
        - height: 60px
        - border-radius: 15px
        - background-color: var(--primary-background-color)
      icon:
        - color: var(--primary-text-color)
    tap_action:
      action: toggle
      haptic: light      
    hold_action: 
      action: more-info
      haptic: medium
    type: 'custom:button-card'
# Entity to control - Forth button
  - entity: light.family_room_light
# Icon to display - Forth button
    icon: 'mdi:light-switch'
    show_icon: true
    show_name: false
    state:
      - styles:
          icon:
            - color: 'var(--paper-item-icon-active-color)  '
        value: 'on'
# Please change the entities inside to match the entity to be controlled
    style: |
      ha-card {
        margin: 5px;  
        box-shadow: 
          {% if is_state('sun.sun', 'above_horizon') and is_state('light.family_room_light', 'on') %}
            inset -4px -4px 8px 0 rgba(255,255,255,.5), inset 4px 4px 8px 0 rgba(0,0,0,.03);
          {% elif is_state('sun.sun', 'above_horizon') and is_state('light.family_room_light', 'off') %}                      
            -5px -5px 8px 0 rgba(255,255,255,.5),5px 5px 8px 0 rgba(0,0,0,.03);
          {% elif is_state('sun.sun', 'below_horizon') and is_state('light.family_room_light', 'on') %}                      
            inset -4px -4px 10px 0 rgba(50, 50, 50,.5), inset 4px 4px 12px 0 rgba(0,0,0,.3); 
          {% elif is_state('sun.sun', 'below_horizon') and is_state('light.family_room_light', 'off') %}   
            -5px -5px 8px 0 rgba(50, 50, 50,.5),5px 5px 8px 0 rgba(0,0,0,.15);
          {% endif %}     
      }
      @media only screen and (max-width: 600px) {
         ha-card {
           margin: 3px; 
           margin-left: 4px;                   
         }
      }
      @media only screen and (min-width: 1200px) {
         ha-card {
           margin: 8px;  
           margin-left: 11px;               
         }
      }
    styles:
      card:
        - width: 60px
        - height: 60px
        - border-radius: 15px
        - background-color: var(--primary-background-color)
      icon:
        - color: var(--primary-text-color)
    tap_action:
      action: toggle
      haptic: light      
    hold_action: 
      action: more-info
      haptic: medium
    type: 'custom:button-card'
# Entity to control - Fifth button
  - entity: switch.dining_table
# Icon to display - Fifth button
    icon: 'mdi:table-chair'
    show_icon: true
    show_name: false
    state:
      - styles:
          icon:
            - color: 'var(--paper-item-icon-active-color)  '
        value: 'on'
# Please change the entities inside to match the entity to be controlled  
    style: |
      ha-card {
        margin: 5px;            
        box-shadow: 
          {% if is_state('sun.sun', 'above_horizon') and is_state('switch.dining_table', 'on') %}
            inset -4px -4px 8px 0 rgba(255,255,255,.5), inset 4px 4px 8px 0 rgba(0,0,0,.03);
          {% elif is_state('sun.sun', 'above_horizon') and is_state('switch.dining_table', 'off') %}                      
            -5px -5px 8px 0 rgba(255,255,255,.5),5px 5px 8px 0 rgba(0,0,0,.03);
          {% elif is_state('sun.sun', 'below_horizon') and is_state('switch.dining_table', 'on') %}                      
            inset -4px -4px 10px 0 rgba(50, 50, 50,.5), inset 4px 4px 12px 0 rgba(0,0,0,.3); 
          {% elif is_state('sun.sun', 'below_horizon') and is_state('switch.dining_table', 'off') %}   
            -5px -5px 8px 0 rgba(50, 50, 50,.5),5px 5px 8px 0 rgba(0,0,0,.15);
          {% endif %}     
      }
      @media only screen and (max-width: 600px) {
         ha-card {
           margin: 3px; 
           margin-left: 4px;                   
         }
      }
      @media only screen and (min-width: 1200px) {
         ha-card {
           margin: 8px;  
           margin-left: 11px;               
         }
      }
    styles:
      card:
        - width: 60px
        - height: 60px
        - border-radius: 15px
        - background-color: var(--primary-background-color)
      icon:
        - color: var(--primary-text-color)
    tap_action:
      action: toggle
      haptic: light      
    hold_action: 
      action: more-info
      haptic: medium
    type: 'custom:button-card'
type: horizontal-stack            
```
</details>

## Remote Card
<p align="left">
  <img src="docs/images/remote_card_light.png" alt="Remote card light theme" width="425">
  <img src="docs/images/remote_card_dark.png" alt="Remote card dark theme" width="425">
  <br/>
</p>

This card mimics a TV remote. Each button is customizable to execute your desired actions. 

**Required Custom Cards:**

* [**Button Card**](https://github.com/custom-cards/button-card), by [**@RomRider**](https://github.com/RomRider)
* [**Card Mod**](https://github.com/thomasloven/lovelace-card-mod), by [**@thomasloven**](https://github.com/thomasloven)

<details><summary><b>Show code</b></summary>

```yaml
entities:
  - cards:
      - icon: 'mdi:power'
        show_icon: true
        show_name: false
        style: |
          ha-card {
            margin: 10px 10px 10px 155px;
            box-shadow: 
              {% if is_state('sun.sun', 'above_horizon') %}
                -5px -5px 5px 0 rgba(255,255,255,.5),5px 5px 5px 0 rgba(0,0,0,.03);
              {% elif is_state('sun.sun', 'below_horizon') %}
                -5px -5px 5px 0 rgba(50, 50, 50,.5),5px 5px 5px 0 rgba(0,0,0,.15);
              {% endif %}                
          }                
        styles:
          card:
            - width: 60px
            - height: 60px
            - border-radius: 100px
            - background-color: var(--primary-background-color)
          icon:
            - color: var(--primary-text-color)
        tap_action:
          action: call-service
# Please change this to a service you call to toggle the TV/device
          service: remote.send_command
          service_data:
            command: power
            entity_id: remote.xiaomi
        type: 'custom:button-card'
    type: 'custom:hui-horizontal-stack-card'
  - cards:
      - entities:
          - cards:
              - icon: 'mdi:menu-up'
                show_icon: true
                show_name: false
                size: 100%
                styles:
                  card:
                    - margin-left: 69px
                    - box-shadow: none
                    - width: 50px
                    - height: 50px
                    - background-color: var(--primary-background-color)
                  icon:
                    - color: var(--primary-text-color)
                tap_action:
                  action: call-service
# Please change this to a service you call to go 'up' on the TV/device                                            
                  service: remote.send_command
                  service_data:
                    command: up
                    entity_id: remote.xiaomi
                type: 'custom:button-card'
            type: 'custom:hui-horizontal-stack-card'
          - cards:
              - icon: 'mdi:menu-left'
                show_icon: true
                show_name: false
                size: 100%
                styles:
                  card:
                    - margin-left: 11px
                    - box-shadow: none
                    - width: 50px
                    - height: 50px
                    - background-color: var(--primary-background-color)
                  icon:
                    - color: var(--primary-text-color)
                tap_action:
                  action: call-service
# Please change this to a service you call to go 'left' on the TV/device                                              
                  service: remote.send_command
                  service_data:
                    command: left
                    entity_id: remote.xiaomi
                type: 'custom:button-card'
              - name: OK
                show_icon: false
                show_name: true
                style: |
                  ha-card {
                    box-shadow: 
                      {% if is_state('sun.sun', 'above_horizon') %}
                        -4px -4px 4px 0 rgba(255,255,255,.5),4px 4px 4px 0 rgba(0,0,0,.03);
                      {% elif is_state('sun.sun', 'below_horizon') %}
                        -4px -4px 4px 0 rgba(50, 50, 50,.5),4px 4px 4px 0 rgba(0,0,0,.15);
                      {% endif %}                
                  }
                styles:
                  card:
                    - width: 50px
                    - height: 50px
                    - border-radius: 100px
                    - background-color: var(--primary-background-color)
                  name:
                    - font-size: 20px
                    - font-weight: bold
                    - font-family: Helvetica
                    - letter-spacing: '-0.01em'
                tap_action:
# Please change this to a service you call to 'enter' on the TV/device                                            
                  action: call-service
                  service: remote.send_command
                  service_data:
                    command: enter
                    entity_id: remote.xiaomi
                type: 'custom:button-card'
              - icon: 'mdi:menu-right'
                show_icon: true
                show_name: false
                size: 100%
                styles:
                  card:
                    - box-shadow: none
                    - width: 50px
                    - height: 50px
                    - background-color: var(--primary-background-color)
                  icon:
                    - color: var(--primary-text-color)
                tap_action:
                  action: call-service
# Please change this to a service you call to go 'right' on the TV/device                                             
                  service: remote.send_command
                  service_data:
                    command: right
                    entity_id: remote.xiaomi
                type: 'custom:button-card'
            type: 'custom:hui-horizontal-stack-card'
          - cards:
              - icon: 'mdi:menu-down'
                show_icon: true
                show_name: false
                size: 100%
                styles:
                  card:
                    - margin-left: 69px
                    - box-shadow: none
                    - width: 50px
                    - height: 50px
                    - background-color: var(--primary-background-color)
                  icon:
                    - color: var(--primary-text-color)
                tap_action:
                  action: call-service
# Please change this to a service you call to go 'down' on the TV/device                                             
                  service: remote.send_command
                  service_data:
                    command: down
                    entity_id: remote.xiaomi
                type: 'custom:button-card'
            type: 'custom:hui-horizontal-stack-card'
        show_header_toggle: false
        style: |
          ha-card {                           
            box-shadow: 
              {% if is_state('sun.sun', 'above_horizon') %}
                inset -4px -4px 5px 0 rgba(255,255,255,.7), inset 4px 4px 5px 0 rgba(0,0,0,.07);
              {% elif is_state('sun.sun', 'below_horizon') %}
                inset -4px -4px 10px 0 rgba(50, 50, 50,.5), inset 4px 4px 12px 0 rgba(0,0,0,.3); 
              {% endif %}                    
            border-radius: 30px;
            background-color: var(--primary-background-color)
          }
        type: 'custom:hui-entities-card'
    type: 'custom:hui-horizontal-stack-card'
  - show_icon: false
    show_name: false
    style: |
      ha-card {
        --ha-card-background: none !important;
        box-shadow: none !important;
      }
    styles:
      card:
        - width: 10px
        - height: 10px
    type: 'custom:button-card'
  - cards:
      - entities:
          - cards:
              - icon: 'mdi:minus'
                show_icon: true
                show_name: false
                size: 100%
                styles:
                  card:
                    - margin-left: 30px
                    - box-shadow: none
                    - width: 30px
                    - height: 30px
                    - background-color: var(--primary-background-color)
                  icon:
                    - color: var(--primary-text-color)
                tap_action:
                  action: call-service
# Please change this to a service you call to 'volume down' on the TV/device                                              
                  service: remote.send_command
                  service_data:
                    command: volume_down_sony
                    entity_id: remote.xiaomi
                type: 'custom:button-card'
              - name: VOL
                show_icon: false
                show_name: true
                styles:
                  card:
                    - margin-left: 10px
                    - box-shadow: none
                    - width: 30px
                    - height: 30px
                    - border-radius: 100px
                    - background-color: var(--primary-background-color)
                  name:
                    - font-size: 13px
                    - font-weight: bold
                    - font-family: Helvetica
                    - letter-spacing: '-0.01em'
                type: 'custom:button-card'
              - icon: 'mdi:plus'
                show_icon: true
                show_name: false
                size: 100%
                styles:
                  card:
                    - margin-left: 10px
                    - box-shadow: none
                    - width: 30px
                    - height: 30px
                    - background-color: var(--primary-background-color)
                  icon:
                    - color: var(--primary-text-color)
                tap_action:
                  action: call-service
# Please change this to a service you call to 'volume up' on the TV/device                                             
                  service: remote.send_command
                  service_data:
                    command: volume_up_sony
                    entity_id: remote.xiaomi
                type: 'custom:button-card'
            type: 'custom:hui-horizontal-stack-card'
        show_header_toggle: false
        style: |
          ha-card { 
            background-color: var(--primary-background-color);
            border-radius: 15px;
            box-shadow: 
              {% if is_state('sun.sun', 'above_horizon') %}
                  -5px -5px 5px 0 rgba(255,255,255,.5),5px 5px 5px 0 rgba(0,0,0,.03);
              {% elif is_state('sun.sun', 'below_horizon') %}
                -5px -5px 5px 0 rgba(50, 50, 50,.5),5px 5px 5px 0 rgba(0,0,0,.15);
              {% endif %}                
          }
        type: 'custom:hui-entities-card'
    type: 'custom:hui-horizontal-stack-card'
  - cards:
# The first button in the bottom, you can change the icon here. In my case it is 'home'                            
      - icon: 'mdi:home'
        show_icon: true
        show_name: false
        style: |
          ha-card {
            box-shadow: 
              {% if is_state('sun.sun', 'above_horizon') %}
                -5px -5px 5px 0 rgba(255,255,255,.5),5px 5px 5px 0 rgba(0,0,0,.03);
              {% elif is_state('sun.sun', 'below_horizon') %}
                -5px -5px 5px 0 rgba(50, 50, 50,.5),5px 5px 5px 0 rgba(0,0,0,.15);
              {% endif %}                
          }
        styles:
          card:
            - margin-top: 10px
            - margin-left: 5px
            - width: 60px
            - height: 60px
            - border-radius: 15px
            - background-color: var(--primary-background-color)
          icon:
            - color: var(--primary-text-color)
        tap_action:
          action: call-service
# Please change this to a service you want to call on the first button                              
          service: remote.send_command
          service_data:
            command: home
            entity_id: remote.xiaomi
        type: 'custom:button-card'
# The second button in the bottom, you can change the icon here. In my case it is 'return'                                    
      - icon: 'mdi:keyboard-return'
        show_icon: true
        show_name: false
        style: |
          ha-card {
            box-shadow: 
              {% if is_state('sun.sun', 'above_horizon') %}
                -5px -5px 5px 0 rgba(255,255,255,.5),5px 5px 5px 0 rgba(0,0,0,.03);
              {% elif is_state('sun.sun', 'below_horizon') %}
                -5px -5px 5px 0 rgba(50, 50, 50,.5),5px 5px 5px 0 rgba(0,0,0,.15);
              {% endif %}               
          }
        styles:
          card:
            - margin-top: 10px
            - margin-left: 8px
            - width: 60px
            - height: 60px
            - border-radius: 15px
            - background-color: var(--primary-background-color)
          icon:
            - color: var(--primary-text-color)
        tap_action:
          action: call-service
# Please change this to a service you want to call on the second button                                     
          service: remote.send_command
          service_data:
            command: return
            entity_id: remote.xiaomi
        type: 'custom:button-card'
# The third button in the bottom, you can change the icon here. In my case it is a set top box.                                    
      - icon: 'mdi:set-top-box'
        show_icon: true
        show_name: false
        style: |
          ha-card {
            box-shadow: 
              {% if is_state('sun.sun', 'above_horizon') %}
                -5px -5px 5px 0 rgba(255,255,255,.5),5px 5px 5px 0 rgba(0,0,0,.03);
              {% elif is_state('sun.sun', 'below_horizon') %}
                -5px -5px 5px 0 rgba(50, 50, 50,.5),5px 5px 5px 0 rgba(0,0,0,.15);
              {% endif %}                
          }
        styles:
          card:
            - margin-top: 10px
            - margin-left: 8px
            - width: 60px
            - height: 60px
            - border-radius: 15px
            - background-color: var(--primary-background-color)
          icon:
            - color: var(--primary-text-color)
        tap_action:
          action: call-service
# Please change this to a service you want to call on the third button                                      
          service: script.turn_on
          service_data:
            entity_id: script.mi_box
        type: 'custom:button-card'
    type: 'custom:hui-horizontal-stack-card'
show_header_toggle: false
style: |
  ha-card {
    background-color: var(--primary-background-color);
    width: 250px;
    border-radius: 10px;
    margin: 10px auto;
    box-shadow:
      {% if is_state('sun.sun', 'above_horizon') %}
        -8px -8px 8px 0 rgba(255,255,255,.5),8px 8px 8px 0 rgba(0,0,0,.03);
      {% elif is_state('sun.sun', 'below_horizon') %}
        -8px -8px 8px 0 rgba(50, 50, 50,.5),8px 8px 8px 0 rgba(0,0,0,.15);
      {% endif %}   
  }
type: entities
```
</details>

## Thank you!

Developed, maintained, and based on the Lovelace of [**@N-l1**](https://github.com/N-l1) ✨
