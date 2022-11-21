# Pyparsing 

This library enables the parsing of strings inspired by regex. **Pyparsing can not be converted back into regex notation** !

```python
import pyparsing as pp

year = "2022"
month = (pp.Literal("09") | pp.Literal("10") | pp.Literal("11") | pp.Literal("12"))
day = pp.Word("[0-3][0-9]")
full_date = year + month + day

full_date.parse_string("20221109")
# ParseResults(['2022', '11', '09'], {})

full_date.create_diagram("diagram.html")

Pyparsing can not be converted back into regex notation
```

Main methods:
- Word
- Literal
- parse_String
