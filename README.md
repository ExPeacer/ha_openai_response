<picture>
  <source media="(prefers-color-scheme: dark)" srcset="/misc/dark_icon.png">
  <img src="/misc/icon.png"
            align="right"
            width="20%"
     style="float: right; margin: 10px 0px 20px 20px;">
</picture>

# Home Assistant OpenAI Response Sensor

[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg)](https://github.com/custom-components/hacs)

This custom component for Home Assistant allows you to generate text responses using OpenAI's GPT-3 model.

Based on **[Hassassistant's](https://github.com/Hassassistant/openai_response)** and **[Olen's](https://github.com/Olen/openai_response/tree/gpt-3.5-turbo)** work.
Updated to use latest changes on OpenAI API.


Head to **[This Link](https://platform.openai.com/account/api-keys)** to get you API key from OpenAI. 

<img src="https://raw.githubusercontent.com/ExPeacer/openai_response/main/misc/apikey.jpg"
     width="80%" />



## Installation
**1.** 
**(Manual)** Copy the **openai_response** folder to your Home Assistant's custom_components directory. If you don't have a **custom_components** directory, create one in the same directory as your **configuration.yaml** file.

**(HACS)** Add this repository to HACS. https://github.com/ExPeacer/openai_response

**2.** Add the following lines to your Home Assistant **configuration.yaml** file:

```yaml
sensor:
  - platform: openai_response
    api_key: YOUR_OPENAI_API_KEY
    model: "gpt-3.5-turbo" # Optional, defaults to "gpt-3.5-turbo"
    name: "hassio_openai_response" # Optional, defaults to "hassio_openai_response"
    mood: "You are a helpful assistant" #  Optional, defaults to "You are a helpful assistant"
```
Replace **YOUR_OPENAI_API_KEY** with your actual OpenAI API key.

**3.** Restart Home Assistant.

## Usage
Create an **input_text.gpt_input** entity in Home Assistant to serve as the input for the GPT-3 model. Add the following lines to your configuration.yaml file:

```yaml
input_text:
  gpt_input:
    name: GPT-3 Input
```
Note you can also create this input_text via the device helpers page!

If you are creating via YAML, you will need to restart again to activate the new entity,

To generate a response from GPT-3, update the **input_text.gpt_input** entity with the text you want to send to the model. The generated response will be available as an attribute of the **sensor.hassio_openai_response** entity.

You can also use a service call to send a request:

```yaml
service: openai_response.openai_input
data:
  prompt: Tell a joke
  mood: You are a joker     # Optional, will use the configured or default mood if not specified
  model: gpt-3.5-turbo      # Optional, will use the configured or default model if not specified
```

## Example
To display the GPT-3 input and response in your Home Assistant frontend, add the following to your **ui-lovelace.yaml** file or create a card in the Lovelace UI:

```yaml
type: grid
square: false
columns: 1
cards:
  - type: entities
    entities:
      - entity: input_text.gpt_input
  - type: markdown
    content: '{{ state_attr(''sensor.hassio_openai_response'', ''response_text'') }}'
    title: ChatGPT Response
```
Now you can type your text in the GPT-3 Input field, and the generated response will be displayed in the response card.

<img src="https://github.com/ExPeacer/openai_response/blob/main/misc/ha_card.png"
     width="50%" />

## License
This project is licensed under the MIT License - see the **[LICENSE](https://chat.openai.com/LICENSE)** file for details.

**Disclaimer:** This project is not affiliated with or endorsed by OpenAI. Use the GPT-3 API at your own risk, and be aware of the API usage costs associated with the OpenAI API.
