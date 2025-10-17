# markvis

This project is inspired by [markwhen](https://markwhen.com/) but uses the [visjs-timeline](https://visjs.org/) library as the base for rendering timelines.

## User Interface
The application is a single web page, with all the code included. By default, it looks like this:
![Main interface empty](/doc/main_interface_empty.png)

At the top bar, you can find buttons for different functions:
- The **Edit timeline** button opens the left drawer to edit the timeline items.
- The **Export** button allows exporting the timeline content to a markdown file.
- The **Zoom** and **Scale** buttons adjust the zoom level and scale settings, respectively.
- The **Plugins** button opens the bottom drawer with plugin tabs.
- The **Configuration** button opens the right drawer with configuration options.

### Edit Timeline Drawer
The <kbd>Edit timeline</kbd> button opens a drawer on the left side with a text area for writing the markdown that describes the timeline elements.
![Main interface edit timeline](/doc/main_interface_edit.png)

You can adjust the drawer's size by holding the <kbd>ALT</kbd> key and using the left or right arrow keys.
You can also resize the drawer by dragging the handle at its bottom-right corner.

### Configuration Drawer
The <kbd>Configuration</kbd> button opens a drawer on the right side for configuring the timeline display.
![Main interface configuration](/doc/main_interface_config.png)

In this drawer, you can configure:
- Group stacking.
- Default group folding.
- Localization settings.

You can also enable developer mode.

### Plugins Drawer
The <kbd>Plugins</kbd> button opens a bottom drawer containing a list of tabs, each corresponding to a plugin. See the **Plugins** section for more details.

## Syntax
The Markvis Timeline markdown syntax allows you to describe groups, subgroups, and events in a timeline using a simple, readable format.
Note that the syntax is still under development and may change in the future.

### Groups
A new group is represented by a new line in the timeline. The syntax for creating a group is as follows:

```
# group title [|<color>]
// You can add a group with a specific ID
# group title ; group_id [|<color>]
```

You can specify a `<color>` at the end of the line to apply it to all items in the group.

Groups can be nested using multiple `#` characters:
Currently three levels are supported

```
## group title (2nd level)

### group title (3rd level)
```

### Items

Point and box-type items are created as follows:
```
// Point item
 . item title ; <date> [| color]

// Box item
 ! item title ; <date> [| color]
```

#### Icons
Point items can be customized with an icon using one of the following syntaxes:
```
*<provider>:<icon_name> item title ; <date> [|color]
```

The following icon providers are supported:
 * Google Material Icons, with the provider prefix 'm'
 * Bootstrap Icons, with the provider prefix 'b'
 Note that you can omit the provider prefix, the default provider is Google then.
 That can be changed in the markdown file metadata (feature to come)

##### Google Material Icons
For Google Symboles, use one of the names available [here](https://fonts.google.com/icons?icon.set=Material+Icons).

##### Bootstrap Icons
For Bootstrap icons,  use one of the names available [here](https://icons.getbootstrap.com/).

### Range and Background Items
Range and background items are created as follows:
```
// Range item
 ] item title ; <start date> / <end date> [| color]
 ] item title ; <start date> /+<duration> [| color]
```

```
// Background item
 = item title ; <start date> / <end date> [| color]
 = item title ; <start date> / +<duration> [|color]
```

### Date Format
The date format follows the [Luxon.js](https://moment.github.io/luxon/#/parsing) library and uses ISO 8601 format.

Use the link above to see all the available forms of syntax, but in short, you can use the two following forms:
```
// standard format with year/month/day
2025-01-01

// or the format with year/week, with optional day of the week after an optional hyphen
2025W01-2

//after these two form, an optional time can be used after a 'T' character
2025-01-01T08:30
2025W01-2T08:30
```

#### Duration format
For range and background items, you can specify a duration instead of an explicit end date.
The duration must start with a + and can use the [ISO 8601 duration format](https://en.wikipedia.org/wiki/ISO_8601#Durations).

##### Exemples
```
] Task ; 2025-09-22 / +3d      // 3 days
] Sprint ; 2025-09-22 / +2w    // 2 weeks
= Vacation ; 2025-09-22 / +1M  // 1 month
```

##### Supported units:

<!-- - `m` = minutes // not yet supported -->
- `h` = hours
- `d` = days
- `w` = weeks
- `M` = months
- `y` = years

You can also combine units, for example: +1w3d means 1 week and 3 days.

##### Full syntax:

```
<start date> / +<duration>
```

Where `<duration>` is a combination of numbers and units, always starting with +.

### Subgroups
Items within a group can be organized into subgroups using parentheses:
Subgroups syntax use the rounded brackets like in the following example
```
# group
 (subgroup1_name
   . item in group 1 ; <date>
   ] range item in group 1 ; <start date> ; <end date>
 )
 (subgroup2_name
  . item in group 2 ; <date>
  . item in group 2 ; <date>
 )
```
Be careful to avoid mixing items in subgroup and not in subgroup within a group as it leads to display bugs.

## Plugins

### Json plugin

The JSON plugin allows batch creation of items from JSON content.\
The json shall be an array of object with keys for item's title and date.

![json plugin image](/doc/plugin_json.png)

You need to fill the different fields in order to have the items generated:
 * The id of the group where to generate the items. See the groups part of the documentation to see how to specify an id for a group.
 * The key in the json content which contains the title of the items
 * The key in the json date which contains the date of the items

If a key doesn't contains exactly the date format needed, you can extract it with regexp,
 - either for the complete date or date/time by filling the _Date/time regexp_ field only
 - or by filling the _date/time regexp_ field for the date, **and** the _time specific regexp_ for the time specifically.
 
Note that if you fill both of field, a "T" character is inserted between the date and the time part of the string which represents the date and time to suits the ISO 8601 format. An exemple is provided below:

![json plugin regexp example](/doc/plugin_json_regexp.png)

A link to the website regex101 is also available to test the regular expressions.

### API Plugin
The API plugin fetches items from an API endpoint.

Currently, only GET requests are supported.

The API shall returns an json array of objects containing a key for the item's title and another for the date.

![api plugin interface](/doc/plugin_api.png)

You need to fill the different fields in order to have the items generated:
 * The id of the group where to generate the items. See the groups part of the documentation to see how to specify an id for a group.
 * The key in the json content which contains the title of the items
 * The key in the json date which contains the date of the items
 * The API endpoint that return the json.

## Version History

<details>
<summary>History</summary>

v0.1 First usable version\
v0.2 Added support for icons and colors\
v0.3 Added support for subgroups\
v0.4 Added JSON plugin\
v0.5 Added Bootstrap Icons provider\
v0.6 Fixes and enhancements\
v0.7 Added the ability to resize the content drawer\
v0.8 Added the ability to set a color for an entire group\
v0.9 Enhanced the configuration drawer\
v0.10 Added folded group options in configuration\
v0.11 Added developer mode (editable items)\
v0.12 Added API plugin\
v0.13 Moved zoom and scale settings to the top bar\
v0.14 Added dynamic color management and custom CSS rules for icons\
v0.15 Improved icons management and fixed bugs

</details>