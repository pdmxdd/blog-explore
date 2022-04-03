---
title: "Modify on_press()"
date: "2022-04-02"
weight: 115
---

## Modify `on_press()`

The provided on_press() function is a great start. However, I don't want to track **all** key presses as many of the keys don't really matter to me, like the function keys, ctrl, alt, and d-pad.

I'm really interested in the following data:

- typed words (`alphanumeric`)
- typed numbers (`alphanumeric`)
- space bar (special: `Key.space`)
- tab (special: `Key.tab`)
- enter (special: `Key.enter`)
- backspace (special: `Key.backspace`)
- delete (special: `Key.delete`)

### Print only `alphanumeric`

### Also `Key.space` as `" "`

### Also `Key.tab` as `" "`

### Also `Key.enter` as `"<ENTER>"`

### Also `Key.delete` as `"<DELETE>"`

## `Key.backspace` Removes Last List Item