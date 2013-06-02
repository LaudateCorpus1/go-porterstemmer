# Go Porter Stemmer

A native Go clean room implementation of the Porter Stemming Algorithm.

This is NOT a port. This is a native Go implementation from the human-readable
description of the algorithm.

I've tried to make it (more) efficient by NOT internally using string's, but
instead internally using []rune's and using the same (array) buffer used by
the []rune slice (and sub-slices) at all steps of the algorithm.

For Porter Stemmer algorithm, see:

http://tartarus.org/martin/PorterStemmer/def.txt

http://tartarus.org/martin/PorterStemmer/

Also, since when I initially implemented it, it failed the tests at...

http://tartarus.org/martin/PorterStemmer/voc.txt

http://tartarus.org/martin/PorterStemmer/output.txt

... I then looked at the source code here...

http://tartarus.org/martin/PorterStemmer/c.txt

... and noticed that there are some items marked as "DEPARTURE",
which differ from the original algorithm.

Implemented these departures too.

## Usage

To use this Golang library, use with something like:

    package main
    
    import (
      "fmt"
      "github.com/reiver/go-porterstemmer"
    )
    
    func main() {
      
      word := "Waxes"
      
      stem := porterstemmer.StemString(word)
      
      fmt.Printf("The word [%s] has the stem [%s].", word, stem)
    }

Alternatively, if you want to be a bit more efficient, use []rune slices instead, with code like:

    package main
    
    import (
      "fmt"
      "github.com/reiver/go-porterstemmer"
    )
    
    func main() {
      
      word := []rune("Waxes")
      
      stem := porterstemmer.Stem(word)
      
      fmt.Printf("The word [%s] has the stem [%s].", string(word), string(stem))
    }

Although NOTE that the above code may modify original slice (named "word" in the example) as a side
effect, for efficiency reasons. And that the slice named "stem" in the example above may be a
sub-slice of the slice named "word".

Also alternatively, if you already know that your word is already lowercase (and you don't need
this library to lowercase your word for you) you can instead use code like:

    package main
    
    import (
      "fmt"
      "github.com/reiver/go-porterstemmer"
    )
    
    func main() {
      
      word := []rune("waxes")
      
      stem := porterstemmer.StemWithoutLowerCasing(word)
      
      fmt.Printf("The word [%s] has the stem [%s].", string(word), string(stem))
    }

Again NOTE (like with the previous example) that the above code may modify original slice (named
"word" in the example) as a side effect, for efficiency reasons. And that the slice named "stem"
in the example above may be a sub-slice of the slice named "word".
