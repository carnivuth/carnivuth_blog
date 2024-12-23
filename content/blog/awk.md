---
title: Awk personal guide
date: 2024-11-16
draft: true
tags:
  - oneliners
  - command guide
---

`awk` is a language for text parsing and manipulation, it's often used as a tokenizer in bash pipes but it can do a lot more of that here i will document my knowledge about it

> before starting i write down the most common use case of awk
>```bash
>some_command_that_prints_on_stdout | awk -F'[SEPARATOR]'  '{print $[FIELD]}'
>```

## PROCESSING MODEL

awk starts by loading user defined functions than execute `BEGIN` block that process text one record at a time (*default behavior is line filter*) :

{{< mermaid >}}
flowchart TD
A[load functions]
B[initial setup\n by running the BEGIN block]
C[read line]
D[process line]
E[clean up\by running END block]
A --> B --> C
C --> D
D --> C
D --> E
{{< /mermaid >}}

## SYNTAX

Blocks are delimited by `{}` each line contains an isntruction, instructoins can be separated by `;`

```awk
BEGIN{  }
{  }
END{  }
```

## REGEX FILTERS AND ~ OPERATOR

lines can be regex parsed using a variable with a regular expression and then filter the input using the `~` operator

```awk
BEGIN{
filter="REGEX"
}


$0 ~ filter{
    # operation on matched records
}
```

## MATCH FUNCTION

match regex element and put beckrefs in an array

```awk
    match($0, /.* — (.*) ft\. .*/, arr)
    print arr[1]
```

## ONELINERS

- print all token except first one

```bash
awk '{$1=""; print $0}'
```
