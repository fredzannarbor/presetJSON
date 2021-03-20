# PresetJSON

This is a simple standard for an extended JSON format for OpenAI GPT-3 presets. Presets are what OpenAI calls the set of parameters necessary to submit a request to the completion endpoint. 

## Motivation

 1. The [OpenAI Playground](https://beta.openai.com/playground) enables users to save presets and to export them as web links or as **Python** and **curl** functions.  That's very useful, but if you have a codebase in which you have defined gpt3 objects (which is quite likely), you are not going to be able to simply paste the exported presets into your code. In fact, it winds up being an exercise in copying and pasting seven or eight different numerals from the preset export into your own code.  For this reason, it would be very helpful to have an option to export presets from the Playground in JSON.
   
 2. Separating preset metadata from program logic makes it easier to add and maintain both prompts and code.  For example, in a Flask view file called *launcher.py*, I define a route function called 'launcher' that can open any one of a number of json files that I have created.

```
@main_blueprint.route('/launcher/<prompt_name>', methods=['GET', 'POST'])
@login_required  # Limits access to authenticated users
def launcher(prompt_name):

# opens json file <prompt_name>.json that contains specific preset values
# submits completion request to OpenAI using those presets

```

3. Separating preset metadata from program  enables content experts to focus on defining useful prompts without depending on programmers.

4. Experience indicates that the preset json needs some additional fields beyond the parameters defined in the completion endpoint.  These additional fields make it easier to guide the user through the process of creating a well-structured prompt.  They are explained below.


## Example PresetJSON file
```
[
    {
        "preset_name" : "MARC Subject Headings",
        "preset_author": "Fred Zimmerman",
        "preset_active": "True",
        "preset_launched": "20210220",
        "preset_description": "This preset generates subject headings in the MAchine-Readable Catalog (MARC) format used by the Library of Congress.",
        "preset_instructions": "Enter a topic below:",
        "pre_user_input": "MARC Subject Headings \n\n1. ",
        "post_user_input": "\n", 
        "completion_heading" : "Response",
        "engine": "davinci",
        "temperature": 0.52,
        "max_tokens": 100,
        "top_p": 1,
        "fp": 0.52,
        "pp": 0.52,
        "stop_sequence": "7.",
        "echo_on": true
    }
]

```
## PresetJSON fields

All are optional unless specified.  Program logic must be able to parse JSON even if fields are missing. Program should have reasonable defau,ts.

**preset_name:** String.  As you wish the preset name to be presented to the user.

**preset_author:** String.  As you wish the author name to be presented to the user.

**preset_active:** Boolean.  Convenient if you want to be able to hide test presets.

**preset_launched:** Date.  Convenient if you want to be able to sort or filter presets by date.

**preset_description:** String.  What you want the user to know about the preset.

**preset_instructions: **String. Directions to the user on how to enter input.

**pre_user_input:** String.  Text to be prepended to the user input as part of the prompt.  New lines should be escaped.

**post_user_input:** String. Text to be appended to user input as part of the prompt.

**completion_heading:** String.  Optionally prefixed to title of the response object as displayed in the UI.  For example, `{... "completion_heading" = "Ode to a " ...}` would allow the response to a poetry preset with the prompt "Nightingale" to be displayed  as "Ode to a Nightingale".

**engine** ... through **echo on**: as in the OpenAI completion syntax.

## Uploading PresetJSON Files to Your Application

This can be done using git or via file upload applications such as FileZilla.  Your application is responsible for finding, displaying, and opening PresetJSON files.  In my application, this is done with a for loop over a directory called `app/presets`.

## How to Submit Your Own Presets to Nimble AI

Use the online form, or use the file picker to upload a valid JSON file
