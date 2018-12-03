# Day 3

The problem is that many of the claims overlap, causing two or more claims to cover part of the same areas. For example, consider the following claims:
```
#1 @ 1,3: 4x4
#2 @ 3,1: 4x4
#3 @ 5,5: 2x2
```
Visually, these claim the following areas:
```
........
...2222.
...2222.
.11XX22.
.11XX22.
.111133.
.111133.
........
```

Parse the raw input:

```
var rawInput = `
#1 @ 1,3: 4x4
#2 @ 3,1: 4x4
#3 @ 5,5: 2x2
`;

function parseInput(rawInput) {
  var input = rawInput.split(/\n/)
    .filter(v => v)
    .map(v => {
      var myRegexp = /#(\d+) @ (\d+),(\d+): (\d+)x(\d+)/g;
      [_, _id, _left, _top, _width, _height] = myRegexp.exec(v);

      return {
        _id,
        _left: parseInt(_left),
        _top: parseInt(_top),
        _width: parseInt(_width),
        _height: parseInt(_height),
      };
  });

  // add right and bottom
  input.forEach(v => {
    v._right = v._left + v._width;
    v._bottom = v._top + v._height;
  })

  return input;
}

var input = parseInput(rawInput);
```

## Part 1

How many square inches of fabric are within two or more claims?

```
function annotatedFabric(input, _maxX, _maxY) {
  var fabric = [];
  // init fabric
  for (var i=0; i < _maxX; i++) {
    for (var j=0; j < _maxY; j++) {
      if (!fabric[i]) {
        fabric[i] = [];
      }
      fabric[i].push(0);
    }
  };

  // represent each claim's square each by a count
  input.forEach(v => {
    for (var i=v._left; i < v._right; i++) {
      for (var j=v._top; j < v._bottom; j++) {
        fabric[i][j]++;
      }
    }
  })

  return {
    _maxX,
    _maxY,
    fabric
  };
}

var fabric = annotatedFabric(input, 1000, 1000);

function overlappedInches({_maxX, _maxY, fabric}) {
  var overlappingCount = 0;

  for (var i=0; i < _maxX; i++) {
    for (var j=0; j < _maxY; j++) {
      if (fabric[i][j] > 1) {
        overlappingCount++;
      };
    }
  }

  return overlappingCount;
}

var result1 = overlappedInches(fabric);
```

## Part 2

What is the ID of the only claim that doesn't overlap?

```
function uniqueClaimID(input, {fabric}) {
  var uniqueClaim = undefined;

  input.forEach(v => {
    for (var i=v._left; i < v._right; i++) {
      for (var j=v._top; j < v._bottom; j++) {
        if (fabric[i][j] != 1) return;
      }
    }

    uniqueClaim = v._id;
  })

  return uniqueClaim;
}

var result2 = uniqueClaimID(input, fabric);
```

