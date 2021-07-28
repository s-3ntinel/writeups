# Spelling Test

## Description
I made a spelling test for you, but with a twist. There are several words in words.txt that are misspelled by one letter only. Find the misspelled words, fix them, and find the letter that I changed. Put the changed letters together, and you get the flag. Make sure to insert the "{}" into the flag where it meets the format.

NOTE: the words are spelled in American English

## Files
Provided set of misspelled words.

## Code
### `words.txt`
```
molecules
guidelines
cosmetics
widescreen
manufacture
purchased
frequencies
convirgence
successful
conjunction
...
```

## Methodology
We have to obtain a wordlist containing as many english words as possible to corelate it with the list of provided misspelled words and filter out the correct words. What will remain are the misspelled words from which we will get the flag.

I used this wordlist.

https://raw.githubusercontent.com/dwyl/english-words/master/words_alpha.txt

## Code
```python
with open('words.txt') as f:
  w1 = f.readlines()

with open('words_alpha.txt') as f:
  w2 = f.readlines()

# strip new lines
sw1 = [s.strip() for s in w1]
sw2 = [s.strip() for s in w2]

for w in sw1:
  # print misspelled word
  if not w in sw2:
    print(w)

```

Pipe the output to a file. For me the file contained 70 words. I had to manually scan the file and remove the remaining correct words that weren't in the `words_alpha.txt`. Then go thought each misspelled word and mark down the wrong letter.

## Flag
**ictf{youpassedthespellingtest}**