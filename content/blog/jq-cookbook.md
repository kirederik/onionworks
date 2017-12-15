+++
date = "2017-02-27T13:38:47Z"
publishdate = "2017-02-27T13:38:47Z"
title = "jq cookbook"
+++

If you ever had to handle JSON in Bash, you probably used [jq](https://stedolan.github.io/jq/), the lightweight and portable cli to process JSON data. Like [awk](https://en.wikipedia.org/wiki/AWK) and [sed](https://en.wikipedia.org/wiki/Sed), jq has it's own [syntax](https://stedolan.github.io/jq/manual) and it can be quite intimidating the first time you see it.

Rather than trying to be a complete cookbook (or a manual, or a tutorial), in this post I'll share some useful snippets that I used in the past, and will keep updating it as I encounter new uses to the tool.

<!-- recipes -->

## Merging 

### Sum

```bash
$> json1='{"foo": "bar", "otherfoo": 1}'
$> json2='{"bar": "foo", "otherfoo": 2}'
$> echo "$json1" "$json2" | jq -s add #or '.[0] + .[1]
{
  "foo": "bar",
  "bar": "foo",
  "otherfoo": 2  # value on the right operand wins
}
```

### Recursive merge

```bash
$> json1='{"key": {"foo": "bar", "bar": "foo"}}'
$> json2='{"key": {"foo": "otherbar", "otherbar": "otherfoo"}}'
$> echo "$json1" "$json2" | jq -s '.[0] * .[1]'
{
  "key": {
    "foo": "otherbar", # value on the right operand wins
    "bar": "foo",
    "otherbar": "otherfoo"
  }
}
```

### Arrays

```bash
$> json1='{"foo": ["foo", "bar"]}'
$> json2='{"foo": ["otherfoo"]}'
$> echo "$json1" "$json2" | jq -s '
    def flatten: 
        reduce .[] as $i([]; 
            if $i | type == "array" then 
                . + ($i | flatten) 
            else  
                . + [$i] 
            end 
        );  
    [.[] | to_entries] | flatten | reduce .[] as $dot
    ({}; .[$dot.key] += $dot.value)'
{
  "foo": [
    "foo",
    "bar",
    "otherfoo"
  ]
}
```

