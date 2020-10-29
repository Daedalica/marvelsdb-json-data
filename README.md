MarvelsDB cards JSON data
=========

The goal of this repository is to store marvelsdb card data in a format that can be easily updated by multiple people and their changes reviewed.

## Validating and Formatting JSON

Steps have been verified with Python 2.7.x and 3.7.x.

#### Set Up Python Environment

The validation script requires python package `jsonschema` which can be installed using `pip` via `pip install jsonschema`. It is recommended that you use a virtual environment, but this is strictly optional.

```bash
# use a virtual environment (optional)
virtualenv venv
source venv/bin/activate
pip install jsonschema
```

If you see an error like _"No module named jsonschema"_ when running the script, you have not installed the `jsonschema` package correctly.

#### Run the Validation Script

To check the JSON files:

```bash
./validate.py

# or for more detailed output, include the --verbose flag

./validate.py --verbose
```

To check and apply formatting to JSON files:

```bash
./validate.py --fix_formatting

# or for more detailed output, include the --verbose flag

./validate.py --verbose --fix_formatting
```


#### Pack schema

* **code** - identifier of the pack. The acronym of the pack name, with matching case, except for Core Set. Examples: `"Core"` for Core Set.
* **name** - properly formatted name of the pack. Examples: `"Core Set"`
* **position** - number of the pack within the cycle. Examples: `1` for Core Set
* **released** - date when the pack was officially released by FFG. When in doubt, look at the date of the pack release news on FFG's news page. Format of the date is YYYY-MM-DD. May be `null` - this value is used when the date is unknown. Examples: `"2016-10-08"` for Core Set
* **size** - number of different cards in the pack. May be `null` - this value is used when the pack is just an organizational entity, not a physical pack.  Examples: `120` for Core Set

#### Card schema

* agility - Agility value of investigator or number of Agility icons for use in skill checks
* **code** - 5 digit card identifier. Consists of two zero-padded numbers: first two digits are the cycle position, last three are position of the card within the cycle (printed on the card).
* cost - Play cost of the card. Relevant for all cards except agendas and titles. May be `null` - this value is used when the card has a special, possibly variable, cost.
* **deck_limit**
* deck_options - Investigator only - Special string describing the card options for an investigator. e.g. "faction:guardian:0:5"
* deck_requirements - Investigator only - Special string describing the card requirements for an investigator. e.g. "size:30" "card:01007" "random:subtype:basicweakness"
* **faction_code**
* flavor
* health - Health of Investigator or Ally Asset
* illustrator
* **is_unique**
* lore - Lore value of investigator or number of Lore icons for use in skill checks
* **name**
* octgn_id
* **pack_code**
* **position**
* **quantity**
* restrictions - Special string describing what decks this is restricted to e.g. investigaor:01001 will limit it to investigator with id 01001
* sanity - Sanity of Investigator or Ally Asset
* slot - Asset only - Which item slot it takes up, 1-Handed, 2-Handed, Amulet, Arcane, Ally
* strength - Strength value of investigator or number of Strength icons for use in skill checks
* subtype_code - Subtype of card, e.g. basicweakness or weakness.
* text
* traits
* **type_code** - Type of the card. Possible values: `"asset"`, `"event"`, `"skill"`, `"treachery"`, `"investigator"`
* wild - Number of wild icons for use in skill checks
* will - Will value of investigator or number of Will icons for use in skill checks

## JSON text editing tips

Full description of (very simple) JSON format can be found [here](http://www.json.org/), below there are a few tips most relevant to editing this repository.

#### Non-ASCII symbols

When symbols outside the regular [ASCII range](https://en.wikipedia.org/wiki/ASCII#ASCII_printable_code_chart) are needed, UTF-8 symbols come in play. These need to be escaped using `\u<4 letter hexcode>`.

To get the 4-letter hexcode of a UTF-8 symbol (or look up what a particular hexcode represents), you can use a UTF-8 converter, such as [this online tool](http://www.ltg.ed.ac.uk/~richard/utf-8.cgi).

#### Quotes and breaking text into multiple lines

To have text spanning multiple lines, use `\n` to separate them. To have quotes as part of the text, use `\"`.  For example, `"flavor": "\"Winter is Fghghghghfhgh.\"\n-Eddard Stark"` results in following flavor text:

> *"Winter is Fghghghghfhgh."*
> *-Eddard Stark*

#### Arkham LCG Game Symbols

These can be used in a card's `text` section.

* `[reaction]`
* `[action]`
* `[free]`
* `[eldersign]`
* `[will]`
* `[lore]`
* `[strength]`
* `[agility]`
* `[health]`
* `[sanity]`

#### Translations

To merge new changes in default language in all locales, run the CoffeeScript script `update_locales`.

Pre-requisites:
 * `node` and `npm` installed
 * `npm -g install coffee-script`

Usage: `coffee update_locales.coffee`
