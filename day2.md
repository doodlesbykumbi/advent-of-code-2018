# Day 2

Parse raw input:
```
var rawInput = `
abcdef
bababc
abbcde
abcccd
aabcdd
abcdee
ababab
`;

function parseRawInput(input) {
  return input.split(/\n/).filter(v => v);
}

var input = parseRawInput(rawInput);
```

## Part 1

Of these box IDs, four of them contain a letter which appears exactly twice, and three of them contain a letter which appears exactly three times. Multiplying these together produces a checksum of 4 * 3 = 12.

What is the checksum for your list of box IDs?

```
function naiveChecksum(input) {
  var threes = 0;
  var twos = 0;

  input.forEach(id => {
    var idCount = {};
    id.split('').forEach(c => {
      if (!idCount[c]) {
        idCount[c] = 1;
      } else {
        idCount[c] += 1;
      }
    });

    var hasThree = Object.values(idCount).find(v => v == 3);
    var hasTwo = Object.values(idCount).find(v => v == 2);

    if (hasThree) {
      threes++;
    }
    if (hasTwo) {
      twos++;
    }
  });

  return twos * threes;
}

var result1 = naiveChecksum(input);
```

## Part 2:

The boxes will have IDs which differ by exactly one character at the same position in both strings.
The IDs `abcde` and `axcye` are close, but they differ by two characters (the second and fourth). However, the IDs `fghij` and `fguij` differ by exactly one character, the third (h and u). Those must be the correct boxes.

What letters are common between the two correct box IDs? (In the example above, this is found by removing the differing character from either ID, producing `fgij`.)

```
function commonLettersIfCorrect(left, right) {
  var numDiffs = 0;
  var diffIndex = -1;
  left.split('').forEach((leftChar, i) => {
    rightChar = right[i];
    if (rightChar != leftChar) {
      numDiffs ++;
      diffIndex = i;
    }
  })

  var commonLetters = "";
  if (numDiffs == 1) {
    commonLetters = left.split('').filter((v, i) => i != diffIndex).join('');
  }
  return commonLetters;
}

function solution(input) {
  var commonLetters;

  for (var i=0; i < input.length; i++) {
    var left = input[i];
    for (var j=0; j < input.length; j++) {
      if (i == j) {
        break;
      }
      var right = input[j];
      commonLetters = commonLettersIfCorrect(left, right);
      if (commonLetters) {
        return commonLetters;
      }
    }
  }
  return commonLetters;
}

var result2 = solution(input);
```
