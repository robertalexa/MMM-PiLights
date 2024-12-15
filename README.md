# MMM-PiLights
MagicMirror Module to control a led strip attached to a Raspberry Pi


## Requirements

This module is specifically meant for the Raspberry Pi system, using SPI and a WS2801
LED strip.

If using Raspbian, you will need to enable SPI:

```bash
sudo raspi-config
```

then navigate to `Advanced` options and enable `SPI`


## Module installation

Clone the module and npm install:

```bash
cd ~/MagicMirror/modules
git clone https://github.com/robertalexa/MMM-PiLights.git
cd MMM-PiLights
npm install
```

Add the module config to `~/MagicMirror/config/config.js`

```javascript
modules: [
    {
        module: 'MMM-PiLights',
        config: {
            ledCount: 64,
            brightness: 1.0 // between 0.0 and 1.0
        }
    }
]
```


## Module Configuration Options

<table width="100%">
    <thead>
        <tr>
            <th>Option</th>
            <th>Type</th>
            <th>Default</th>
            <th width="100%">Description</th>
        </tr>
    <thead>
    <tbody>
        <tr>
            <td><code>ledCount</code></td>
            <td>Integer</td>
            <td><code>64</code></td>
            <td>Number LEDs on your strip</td>
        </tr>
        <tr>
            <td><code>brightness</code></td>
            <td>Float</td>
            <td><code>0.0</code></td>
            <td>The LED strip brightness (between 0.0 and 1.0)</td>
        </tr>
    </tbody>
</table>


## Trigger from another module

If you're developing a module and want to trigger a light sequence, here's an example of
what you'd use in your module:

```javascript
this.sendNotification('PILIGHTS_SEQUENCE', 'blue_pulse');
```

My other MagicMirror module is able to trigger light sequences with notifications:
[MMM-IFTTT](https://github.com/jc21/MMM-IFTTT)

## Trigger from a endpoint

The GET endpoint is pretty simple:

`http://yourmagicmirror/PiLights?sequence=blue_pulse`

From the command line:

```bash
curl -X GET "http://yourmagicmirror/PiLights?sequence=blue_pulse"
```

## Available Sequences

- off
- white_pulse
- blue_pulse
- lightblue_pulse
- red_pulse
- green_pulse
- orange_pulse
- pink_pulse
- white_static
- warm_static
- blue_static
- red_static
- green_static
- orange_static
- pink_static
- rainbow
- christmas
- candy

## Note
I am not actively maintaining this, but happy to help anyone that will give this a go.

The code is not in a clean shape, not having enough time... one day i will improve it, hopefully :)

The code is slightly bespoke to my use case, where I am matching this with MMM-GoogleAssistant. When called, the
assistant will trigger the lightblue_pulse notification. In case there was already an effect, once the pulse finishes,
it gets restored.

## MMM-GoogleAssistant Recipe
```js
/**   WS-2801 LED Strip Control  		**/
/**   Vocal commands script             **/
var recipe = {
    plugins: {
        onActivate: "LED_LBLUE_PULSE"
    },

    commands: {
        "LED_LBLUE_PULSE": {
            notificationExec: {
                notification: "PILIGHTS_SEQUENCE",
                payload: {
                    sequence: "lightblue_pulse",
                    iterations: "2"
                }
            }
        },
    }
}
```

## Disclaimer
All credits go to the original author and original repository https://github.com/jc21/MMM-PiLights

I have only updated packages and made changes that I have been using for years, just never publicised.
