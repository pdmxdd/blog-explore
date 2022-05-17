---
title: Python
weight: 105
date: 2022-05-17
---

Alright. I get a CSV file every 5 minutes that contains the last 60 minutes of geological data.

I need Python to:

1. open the file
1. hash each record of the file
1. compare the hashed record to a file of hashed records
    1. if hashed record isn't in the hash store it's unique and we need to save it
    1. add hash to hash store
    1. add full record to new CSV file

## Open & Read File

Straight forward:

```python
def read_requested_file():
    data = []
    with open(f"{DATA_DIR}eq-data.csv", 'r') as csvfile:
        the_reader = reader(csvfile)
        for row in the_reader:
            data.append(",".join(row))
    key_data = data.pop(0)
    return key_data, data
```

The file in question contains a header row with the columns of the file, which explains the `key_data = data.pop(0)` line which removes the first row from the list and stores it in its own variable.

## Hash Records

I had to lookup the python `hashlib` module to figure out how to do this, but the result was very simple. Thanks Python!

```python
def hash_full_row(full_row):
    return hashlib.sha256(bytes(full_row, 'utf-8')).hexdigest()
```

Using this function:

The following CSV row of:

```bash
'2022-05-17T21:39:29.970Z,38.7639999,-122.7316666,-0.87,0.38,md,7,121,0.0151,0.11,nc,nc73735131,2022-05-17T21:47:11.931Z,"3km SE of The Geysers, CA",earthquake,0.34,3.36,,1,automatic,nc,nc'
```

Becomes:

```bash
'c284c647fbced327097d84dc7089392643f9b9d15c2ae8678ff3630caca545dd'
```

But when you have even one character change, like slightly changing the time by 1/1000 of a second you get a completely different result:

```bash
'2022-05-17T21:39:29.971Z,38.7639999,-122.7316666,-0.87,0.38,md,7,121,0.0151,0.11,nc,nc73735131,2022-05-17T21:47:11.931Z,"3km SE of The Geysers, CA",earthquake,0.34,3.36,,1,automatic,nc,nc'
```

1/1000 of a second added to the first timestamp becomes:

```bash
'b9a54ea076d7e436f1aa8f29ed278e94ce504dcc465d60950100c8cd6c19929a'
```

The power of hashing comes through again!

I'll need some way to save these hashes so I can compare them each time.

## Save Hash

`DATA_DIR` is a global constant I created to store the location for where I want to store all of the requested data. For me I chose to put everything in a hidden directory off of my home dir: `/home/paul/.earthquake-data`. That's what the F-string is doing on line 2.

```python
def save_hash(the_hash):
    with open(f'{DATA_DIR}hashes', 'a') as write_csv_file:
        the_writer = writer(write_csv_file)
        the_writer.writerow([the_hash])
```

After saving hashes I will need some way to load all hashes into a list for comparisons.

## Get All Saved Hashes

```python
def get_existing_hashes():
    existing_hashes = []

    with open(f'{DATA_DIR}hashes', newline='\n') as csvfile:
        the_reader = reader(csvfile)
        for row in the_reader:
            existing_hashes.append(row[0])
    return existing_hashes
```

Well this looks suspiciously close to step one. The only difference being the `hashes` file does not have a header row.

## Convert Records as Strings to a Dictionary

Before writing my `main()` function I will need one final function to convert the list of string records into a list of dictionary records. This way I can use the `DictWriter` class to write my file.

`DictWriter` and it's companion `DictReader` will read/write files to and from Python dictionaries instead of as raw strings. This is much nicer to deal with in my opinion. The only reason I didn't use that strategy for the reads is because I wanted the raw string so I could perform a **hash** and comparison of the full raw string record.

But since the comparison has already gone through, and now we are trying to write to a new location I'd prefer to use the `DictWriter` class.

```python
def keys_file_data_to_dict(str_keys, events_as_strs):
    the_dict = {}
    keys_list = str_keys.split(",")
    for event_str in events_as_strs:
        event_as_list = event_str.split(",")
        the_dict[hash_full_row(event_str)] = {
            keys_list[i]: event_as_list[i] for i in range(len(keys_list))
        }
    return the_dict
```

The final step is appending these dictionaries to a file, but I'll handle that in main for now.

## Main Entry point

```python
if __name__ == "__main__":
    existing_hashes = get_existing_hashes()

    file_keys, events_as_str_in_list = read_requested_file()

    event_by_hash_dict = keys_file_data_to_dict(file_keys, events_as_str_in_list)

    for hash, data_dict in event_by_hash_dict.items():
        if hash not in existing_hashes:
            # save_hash(hash)
            save_hash(hash)
            # write the full line to the other CSV file
            with open(f'{DATA_DIR}exactly-once-eq-data.csv', 'a') as csvfile:
                dict_writer = DictWriter(csvfile, fieldnames=data_dict.keys())
                dict_writer.writerow(data_dict)
```

The `main()` function follows my initial logic very closely:

1. get existing hashed records as strings in a list
1. get the file_keys (column values) and events as raw strings in a list
1. convert event string list into event dictionary list
1. loop through all events as dictionaries (so we have access to key and value) using `.items()`
    1. if the hash **is not** in the `existing_hashes` list:
        1. save the hash
        1. append the record to the new csv file: `exactly-once-eq-data.csv`