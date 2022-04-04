---
title: "Better Print Statement"
date: "2022-04-03"
weight: 130
---

## Better Print Statement

I'd like each print statement to look more like a log, so the timestamp and user would be a great addition to each new line.

## Timestamp

### Imports

```python
from time import sleep, asctime, localtime
```

### Print update

```python
print(f"{asctime(localtime())}: {''.join(log)}")
```

### Validation

![Better Print Output](pictures/better-print.png)

## User

### Imports

```python
from os import getlogin
```

### Print update

```python
print(f"{getlogin()} -- {asctime(localtime())}: {''.join(log)}")
```

### Validation

![User Better Print Output](pictures/better-print-user.png)

Even better.

## Source Code Snapshot

[GitHub repo at this point in time](https://github.com/pdmxdd/pylogger/tree/211f75c76d9460b2240ec305292ae024a633de00)