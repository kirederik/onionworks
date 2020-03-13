+++
title = "When nil is not nil"
date = "2018-07-29T12:00:00+01:00"
draft = true
+++

A couple of weeks ago, we found a strange case of "nil != nil" in our codebase.
We had something like this:

```go

func NewStore(b SomeInterface) SomeStruct {
  return &SomeStruct{client: b}
}

func (s *SomeStruct) Do() {
  if b != nil {
    // do stuff
  }
}

func main() {
  var b *AStruct
  // ...
  p := NewStore(b)
}



```
