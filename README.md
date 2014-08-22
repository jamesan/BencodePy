#BencodePy
A small Python 3 library for encoding and decoding Bencode data licensed under the GPLv2.

##Overview
Although Bencoding is mainly, if not exclusively, used for BitTorrent metadata (.torrent) files, this library seeks to
provide a generic means of encoding/decoding Bencode from/to Python data structures independent of torrent files.

##Docs

### Installation
`pip install bencodepy`

### Encode

```python
from bencodepy import encode
mydata = { 'keyA': 'valueA' } #example data
bencoded_data = encode(mydata)
print (bencoded_data)
>>> b'd4:keyA6:valueAe'
```

Encode Mappings:

Python Type*  | Bencode Type
------------- | -------------
dict  | Dictionary
list  | List
tuple  | List
int  | Integer
str  | String
bytes  | String

*Includes subtypes thus both dict and OrderedDict would be represented as Bencode dictionary.

### Decode

From bytes...
```python
from bencodepy import decode
mydata = b'd4:KeyA6:valueAe'
my_ordred_dict = decode(mydata)
print(my_ordred_dict)
>>> OrderedDict([(b'KeyA', b'valueA')])
```

Alternatively from a file...
```python
from bencodepy import decode_from_file
my_file_path = 'c:\whatever'
my_ordred_dict = decode_from_file(my_file_path)
```

Decode Mappings:

Bencode Type | Python Type
------------- | -------------
Dictionary  | OrderedDict
List  | list
Integer  | int
String  | bytes

Bencode dictionaries are decoded as Python OrderedDict to preserve the order of elements. This is necessary to correctly
calculate certain hash values such as that of a torrent file's Info Dictionary.

Decode methods will always return an iterable. If the root element of the bencode data is not a dictionary or 
list, `decode()` will wrap the all bencode elements in a tuple. Thus input data of `b'5:ItemA5:ItemB'` would yield
a python tuple of `('ItemA', 'ItemB')`.
  

##TODO
1. Determine method of distributing the optimized (cythonized) version of bencodepy.
2. Consider async file read; I may hold off until someone creates a request on the Github issue tracker.

##License
Copyright © 2014 by Eric Weast

Licensed under the [GPLv2](https://www.gnu.org/licenses/gpl-2.0.html "gnu.org")