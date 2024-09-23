# Serialization

Packets can primarily be serialized into network data in two ways:
FantaCode or JSON.

## FantaCode

In this method, the serialization starts with the header,
followed by `#`, then the values separated by `#`.  
A value may also be subdivided by `&`. It is terminated by `#%` 
```
<header>#<value1#subvalue1&subvalue2&subvalue3&#value3#%
```
The key of the values are implicitly given from the position.

For example, an OOC Message may look like this:
```
CT#Phoenix#Objection!#%
```
Where the `name` is given as `Phoenix` and the `message` is given as `Objection!`.

Pseudocode for serializing a simple packet:
```
header + '#' + '#'.join(values) + '#%'
```

Note that values must be encoded as UTF-8.

### Escapes
As certain characters is used as control characters, they must be
escaped if they appear in values.

| Character | Escape code |
|-----------|-------------|
| `#`       | `<pound>`   |
| `&`       | `<and>`     |
| `%`       | `<percent>` |
| `$`       | `<dollar>`  |

Pseudocode for escaping all characters in a value:
```
value
.replaceAll('#', '<num>')
.replaceAll('&', '<and>')
.replaceAll('%', '<percent>')
.replaceAll('$', '<dollar>');
```

Vice versa for unescaping.

## JSON

Packets can also be serialized as JSON using this format:
```(json)
{
    "header": "<header>",
    "data": {
        "key1": <value1>,
        "key2": <value2>,
        ...
    }
}
```

Header must always be a string, but the rest of the values
can have any type permitted by the JSON standard.
Note that for the serialization to be considered valid,
the values must adhere to what's specified in Network Protocol.
Invalid packets may be dropped.
