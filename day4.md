# Day 3

Parse input:

```
var rawInput = `
[1518-11-01 00:00] Guard #10 begins shift
[1518-11-01 00:05] falls asleep
[1518-11-01 00:25] wakes up
[1518-11-01 00:30] falls asleep
[1518-11-01 00:55] wakes up
[1518-11-01 23:58] Guard #99 begins shift
[1518-11-02 00:40] falls asleep
[1518-11-02 00:50] wakes up
[1518-11-03 00:05] Guard #10 begins shift
[1518-11-03 00:24] falls asleep
[1518-11-03 00:29] wakes up
[1518-11-04 00:02] Guard #99 begins shift
[1518-11-04 00:36] falls asleep
[1518-11-04 00:46] wakes up
[1518-11-05 00:03] Guard #99 begins shift
[1518-11-05 00:45] falls asleep
[1518-11-05 00:55] wakes up
`;

var input = rawInput.split(/\n/).filter(line => line).map(line => {
  var reg =  /\[(\d+)-(\d+)-(\d+) (\d+):(\d+)\] (.+)/g;
  [_, year, month, date, hour, minute, event] = reg.exec(v);
  return {
    year: parseInt(year),
    month: parseInt(month),
    date: parseInt(date),
    hour: parseInt(hour),
    minute: parseInt(minute),
    event
  };
}).sort((l, r) => (l.year * 100000000 + l.month * 1000000 + l.date * 10000 + l.hour * 100 + l.minute) - (r.year * 100000000 + r.month * 1000000 + r.date * 10000 + r.hour * 100 + r.minute))

guards = {
//  [id]: [date, start, end]
}
var currentGuard = undefined;
var currentStart = undefined;
input.forEach(v => {
  if (v.event == "wakes up") {
    guards[currentGuard].push([`${v.month}-${v.date}`, currentStart, v.minute]);
  } else if (v.event == "falls asleep") {
    currentStart = v.minute;
  } else {
    var reg =  /Guard #(\d+) begins shift/g;
    [_, id] = reg.exec(v.event);
    currentGuard = id;
    if (!guards[id]) {
      guards[id] = [];
    }
  }
})
```

## Part 1

```
var maxDuration = -1;
var startTime = -1;
var guardID = undefined;
Object.keys(guards).forEach(k => {
  var guard = guards[k];
  var totalSleepTime = 0;
  guard.forEach(([date, start, end]) => {
    var sleepDuration = end - start;
    totalSleepTime +=  sleepDuration;
  })

  if (totalSleepTime > maxDuration) {
    maxDuration = totalSleepTime;
    guardID = k;
  }
})

times = [];
for (var i=0; i < 60; i++) {
  times[i] = 0;
}

guards[guardID].forEach(([date, start, end]) => {
  for (var i=start; i < end; i++) {
    times[i] += 1;
  }
})


maxTime = 0;
for (var i=0; i < 60; i++) {
  if (times[i] >= times[maxTime]) {
    maxTime = i;
  }
}

console.log(maxTime + " x " + guardID + " = " + maxTime * parseInt(guardID))
```

## Part 2

```
var currentMaxTime = -1;
var currentMaxOccuranceOfTime = -1;
var guardID = undefined;
Object.keys(guards).forEach(k => {
  var guard = guards[k];
  var times = [];
  for (var i=0; i < 60; i++) {
    times[i] = 0;
  }
  guard.forEach(([date, start, end]) => {
    for (var i=start; i < end; i++) {
      times[i] += 1;
    }
  })

  var maxTime = 0;
  for (var i=0; i < 60; i++) {
    if (times[i] >= times[maxTime]) {
      maxTime = i;
    }
  }
  maxOccuranceOfTime = times[maxTime];

  if (maxOccuranceOfTime > currentMaxOccuranceOfTime) {
    currentMaxOccuranceOfTime = maxOccuranceOfTime;
    currentMaxTime = maxTime;
    guardID = k;
  }

})

console.log(currentMaxTime + " x " + guardID + " = " + currentMaxTime * parseInt(guardID))
```

