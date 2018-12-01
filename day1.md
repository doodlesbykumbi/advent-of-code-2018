# Day 1

Input is a list of newline separated signed integers
```
const input = `
-6
+3
+8
+5
-6
`;
```
## Part 1

Sum of parsed input entries:
```
function sum_of_input(input) {
  return input.split(/\n/)
              .filter(v => v)
              .map(v => parseInt(v))
              .reduce((acc, v) => acc + v, 0);
}
```

## Part 2

Imagine that you're calcuting a rolling sum by adding each consecutive parsed entry in the input.
When you reach the end of the input start from the beginning and carry on.
Find the first rolling sum to repeat:
```
function first_repeated_rolling_sum(input) {
  const changes = input.split(/\n/).filter(v => v).map(v => parseInt(v));

  let sums = {};
  let sum = 0;
  let i = 0;
  while (true) {
    i = (i + 1) % changes.length;
    const change = changes[i];
    sum += i;
    if (sums[sum]) return sum;
    sums[sum] = true;
  }
}
```

