# Circle Sensor Card
[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg?style=for-the-badge)](https://github.com/custom-components/hacs)

Custom component for lovelace which can be used as a card or an element on a picture-elements card.

This is a mod, created wit ai. All credits to the original creator. I spent a night with claude sonet and fixed the bugs and added alot of features you and i wanted. Complete example at the bottom

![Circle Sensor Examples](circle-sensor.png)

**Note: When including this file in your `ui-lovelace.yaml` you must use `type: module`**

## Config

| Name | Type | Description | Default
| ---- | ---- | ----------- | -------
| type | string | `custom:circle-sensor-card` | **Required**
| entity | string | `sensor.temperature` | **Required**
| show_card | boolean | Render as a card | `false`
| name | string | Name to display above sensor value | none
| min | number | Minimum value | `0`
| max | number | Maximum value | `100`
| font_style | list | CSS style properities to apply to font | none
| fill | string | Background color of circle | `rgba(255, 255, 255, .75)`
| stroke_width | number | Width of circle value indication ring | `6`
| stroke_color | hex code | Default stroke color | `#03a9f4`
| stroke_bg_width | number | Width of background stroke | none
| stroke_bg_color | hex code | Default stroke bg color | `#999999`
| color_stops | object | Sensor value to color mapping (see below) | none
| gradient | boolean | Whether to smoothly transition between color stops | `false`
| units | string | Custom units of measurement | none
| attribute | string | Attribute element of entity to use instead of its state | none
| attribute_max | string | Use attribute element of entity as max | none
| show_max | boolean | Display the max value | `false`

### Color stops
A mapping from `value` to `color`. If `gradient` is set to true, mid-stop colors will be
calculated on a linear gradient from one stop to the next.

```yaml
color_stops:
  50: '#84B821'
  100: '#D43214' 
```

## Installation

### Step 1

Install `circle-sensor-card` by copying `circle-sensor-card.js`from this repo to `<config directory>/www/` on your Home Assistant instance.

**Example:**

```bash
wget https://raw.githubusercontent.com/custom-cards/circle-sensor-card/master/circle-sensor-card.js
mv circle-sensor-card.js /config/www/
```

### Step 2

Link `circle-sensor-card` inside you `ui-lovelace.yaml`.

```yaml
resources:
  - url: /local/circle-sensor-card.js?v=0
    type: module
```

### Step 3

Add a custom card or custom element in your `ui-lovelace.yaml` using `type: custom:circle-sensor-card`

## Example
```yaml
- type: custom:circle-sensor-card
  # Required - The entity to display
  entity: sensor.temperature
  
  # Optional - Display name above value
  name: "Temperature"
  
  # Optional - Render as a card instead of element
  show_card: true
  
  # Optional - Number of decimal places to show
  decimals: 1
  
  # Value Range Configuration
  # Can be static number, sensor (sensor:entity_id) or attribute (attr:attribute_name)
  # Supports arithmetic operations (sensor:entity_id+5)
  min: "sensor:input_number.min+5"
  max: "sensor:input_number.max-2"
  
  # Optional - Custom unit or attribute to display
  units: '°C'
  attribute: temperature
  
  # Circle Appearance
  stroke_width: 10          # Width of progress circle
  stroke_color: blue        # Default color if no stops defined
  stroke_bg_width: 2        # Width of background circle
  stroke_bg_color: '#999999'  # Color of background circle
  fill: 'rgba(255, 255, 255, .75)'  # Fill color inside circle
  stroke_linecap: round     # Line end style: 'butt', 'round', 'square'
  
  # Visual Effects
  use_gradient: true        # Enable gradient effect on circle
  use_shadow: true         # Enable shadow effect
  gradient: true           # Enable color blending between stops
  
  # Color Stops - Define colors at specific values
  # Supports: "<min", "<min+5", "min+10", "<max-5", "max"
  color_stops:
    "<min": blue           # Blue when value < min
    "<min+5": yellow       # Yellow when value < (min + 5)
    "min+10": green        # Green when value > (min + 10)
    "max-5": purple        # Purple when value > (max - 5)
    "max": red            # Red when value = max
  
  # Icon Configuration
  icon: 'mdi:thermometer'
  icon_position: 'middle'   # Position: 'left', 'right', 'middle', 'above', 'below'
  icon_gradient: true      # Enable color gradient for icon
  
  # Icon Styling
  icon_style:
    color: blue            # Default icon color
    '--mdc-icon-size': '24px'
    opacity: '0.9'
    filter: 'drop-shadow(0px 0px 2px rgba(0,0,0,0.2))'
    margin: '6px'
    # Icon Animation
    animation:
      duration: '2s'       # Time per rotation
      timing: 'linear'     # Animation curve
      iteration: 'infinite'  # Number of repeats
  
  # Icon Color Stops - Same syntax as circle color_stops
  icon_color_stops:
    "<min": blue
    "min+5": green
    "max": red
  
  # Text Styling
  font_style:
    value_size: '150%'     # Size of the value
    unit_size: '75%'      # Size of the unit
    color: 'red'
    font-weight: 'bold'
    font-family: 'Trebuchet MS'
    text-shadow: '2px 2px black'
    letter-spacing: '1px'
    text-transform: 'uppercase'
    opacity: '0.9'
  
  # Size & Position - Using CSS variables
  style:
    --circle-sensor-width: 200px
    --circle-sensor-height: 200px
```
