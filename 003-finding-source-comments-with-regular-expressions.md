# Finding Source Comments With Regular Expressions

Today a [colleague ](https://twitter.com/schooneman)and I ran into the problem of finding commented out sections in xhtml files. Particularly, we were looking for commented out sections that contained the keyword ‘.xhtml’. In this post I present you how to do this.

![image](https://31.media.tumblr.com/88fb78a0723a27dc36f20b45ee8033eb/tumblr_inline_njlxx3FPq01t9ks7b.jpg)[[MORE]]

## Regular expression

Because we use Eclipse to perform the search, the presented regular expression is in the Java flavour. See the end of the post for a Perl-style regex.

Listing of the complete regular expression to find xml comments containing the keyword ‘.xhtml’:

`(?s)<!--((?!-->).)*\.xhtml((?!-->).)*-->`

## Breakdown of the regular expression

(?s) is the Java-flavour modifier to tell the regex runner that a dot should also match new lines. This way we discover comments spanning more than a single line.

&lt;!- - is our start symbol.

(?!- -&gt;) is a negative lookahead. This matches any sequence, except - -&gt;. We need this because we want to restrict the regular expression to finding single comments.
Consequently ((?!- -&gt;).)* matches any character except the sequence after the negative lookahead.

So until now, we match text that starts with &lt;- -, doesn’t contain - -&gt; but may contain every other character sequence.

Burried inside the comment we expect to find our keyword. Therefore we match on \.xhtml.

Any sequence of characters may come after that, but we should stop as soon as we find the closing tag. We repeat the same negative lookahead match as before to achieve that.

At last, we expect to find the closing sequence of the comment (- -&gt;).

## Comments in other languages

It should be fairly trivial to adapt the regular expression to find comments in other languages.

The general pattern is as follows (keyword is optional):

`(?s)Start-Sequence((?!End-Sequence).)*KeyWord((?!End-Sequence).)*End-Sequence`

The following regex finds all multiblock comments in Java files:

`(?s)/\*((?!\*/).)*((?!\*/).)*\*/`

**Perl style**

The following listing is the same regular expression, but in the Perl style. Notice the modifier.

`/<!--((?!-->).)*\.xhtml((?!-->).)*-->/s`
