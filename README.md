# PresetJSON

This is a simple standard for an extended JSON format for OpenAI GPT-3 presets. Presets are what OpenAI calls the set of parameters necessary to submit [a request to the completion endpoint](https://beta.openai.com/docs/api-reference/completions/create). 

## Motivation

 1. This data model provides a standard you can adopt to separate content from logic in your applications, to make it easier to construct complex prompts, and to make it easier to share detailed presets with other developers.
   

 2. Separating preset metadata from program logic makes it easier to add and maintain both prompts and code.  For example, in a Flask view file called *launcher.py*, I define a route function called 'launcher' that can open any one of a number of json files that I have created.

```
@main_blueprint.route('/launcher/<prompt_name>', methods=['GET', 'POST'])
@login_required  # Limits access to authenticated users
def launcher(prompt_name):

# opens json file <prompt_name>.json that contains specific preset values
# submits completion request to OpenAI using those presets

```

3. Content experts can focus on defining useful prompts without depending on programmers.  You can have end users create preset files and upload them to your applications, which run them.


## Example PresetJSON file
```
[
    {
        "preset_name" : "Display Name for UI Widget",
        "preset_author": "Fred Zimmerman",
        "preset_active": "True",
        "preset_instructions": "Enter a topic below:",
        "preset_description": "This preset generates ... ",
        "pre_user_input" : "Instructions or examples that precede user input in prompt",
        "post_user_input": "Instructions or examples that follow user input in prompt",
        "engine": "davinci",
        "prompt": None,
        "temperature": 0.52,
        "max_tokens": 100,
        "top_p": 1,
        "frequency_penalty": 0.52,
        "presence_penalty": 0.52,
        "stop_sequence": "7.",
        "echo_on": true,
        "completion_heading" : "Response or something specific to the completion, e.g. Ideas",
        "user" = "wfzimmerman",
        "organization" = "openAIorgcode"
    }
]

```

The OpenAI Playground provides the fields from "engine to "presence penalty" in JSON format, so you can simply paste those in to replace the ones in this template.

## PresetJSON fields

All are optional unless specified.  Program logic must be able to parse JSON even if fields are missing. Program should have reasonable defau,ts.

**preset_name:** String.  As you wish the preset name to be presented to the user. This is what appears above the UI Widget.

**preset_author:** String.  As you wish the author name to be presented to the user.

**preset_active:** Boolean.  Convenient if you want to be able to hide test presets.

**preset_description:** String.  What you want the user to know about the preset.  What it's for, rather than how to use it.

**preset_instructions:** String. Directions to the user on how to enter input.

**pre_user_input:** String.  Text to be prepended to the user input as part of the prompt.  New lines should be escaped.

**post_user_input:** String. Text to be appended to user input as part of the prompt.

**completion_heading:** String.  Optionally prefixed to title of the response object as displayed in the UI.  For example, `{... "completion_heading" = "Ode to a " ...}` would allow the response to a poetry preset with the prompt "Nightingale" to be displayed  as "Ode to a Nightingale".

**engine** ... through **echo on**: as in the OpenAI completion syntax.

## Uploading PresetJSON Files to Your Application

This can be done using git or via file upload applications such as FileZilla.  Your application is responsible for finding, displaying, and opening any available PresetJSON files.  In my application, this is done with a for loop over a directory called `app/presets`, or by opening a particular preset file.

