# markvis

This project comes from the inspiration of [markwhen](https://markwhen.com/) but with the [visjs-timeline](https://visjs.org/) library as a base for rendering the timeline.

## User Interface
The application is a single web-page, with all the code included in it.
It looks like this by default
![Main interface empty](/doc/main_interface_empty.png)

The button [Edit timeline] opens a drawer, on the left side, with a textarea to write the markdown that describes the elements to render on the timeline.
![Main interface edit timeline](/doc/main_interface_edit.png)

The button Configuration opens another drawer, on the right side, to configure the timeline display.
![Main interface configuration](/doc/main_interface_config.png)

## Syntax
Here are the details for the markdown syntax.
Be careful that the syntax is still a work in progress and can be changed at anytime.

### Groups
A new group, is represented by a new separated line in the timeline. The syntax to create a new group is the following

```
# group title
// you can add a group with a specific id you provide
# group title ; group_id_you_choose
```

Groups can be nested with several ```#``` characters

```
## group title (2nd level)

### group title (3rd level)
```

### Items
Point and box type items are created like the following
```
// point item
 . item title ; <date> [| color]
```

Point items can also be customize with an icon, with the following syntax
```
 *<icon> item title ; <date> [|color]
```

The \<icon> term can be any of 'icon name' from google Material Icons, available [here](https://fonts.google.com/icons?icon.set=Material+Icons)

After clicking on an icon, the right drawer shows the icon name.
(image)

```
// box item
 ! item title ; <date> [| color]
```

Range and background type items are created like the following
```
// range item
 ] item title ; <start date> ; <end date> [| color]
```

```
// background item
 = item title ; <start date> ; <end date> [| color]
```

### Date format
The date format used follows the [Luxon.js](https://moment.github.io/luxon/#/parsing) library and use the  ISO 8601 format.

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

### subgroups

Within a group, items can be grouped together with a _subgroup_.\
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
Avoid mixing items in subgroup and not in subgroup within a group as it leads to display bugs.

## plugins

### Json plugin

The json plugin allows to create items in batch from a json content.

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

## Version history

<details>
<summary>History</summary>
 
v0.1 first usable version\
v0.2 Add support for icons and colors\
v0.3 Add support for subgroups\
v0.4 Add json plugin
</details>