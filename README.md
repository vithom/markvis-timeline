# markvis

This project comes from the inspiration from [markwhen](https://markwhen.com/) but with the [visjs-timeline](https://visjs.org/) library as a base for rendering the timeline.

## Syntax
Here are the details for the markdown syntax.

## Groups
A new group, is represented by a new separated line in the timeline. The syntax to create a new group is the following

```
# group title

## sub-group title

### subsub-group title
```

### Items
Point and box type items are created like the following
```
// point item
 . item title ; <date> [| color]
```

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
The date format used follows the [Luxon.js](https://moment.github.io/luxon/#/parsing) library.
In short, you can use the two following forms
```
// standard format with year/month/day
2025-01-01

// or the format with year/week, with optional day of the week after an optional hyphen
2025W01-2

//after these two form, an optional time can be used after a 'T' character
2025-01-01T08:30
2025W01-2T08:30
```

## Version history

<details>
<summary>History</summary>
v0.1 first usable version
</details>