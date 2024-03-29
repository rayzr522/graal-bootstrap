# graal-bootstrap

> A simple project to get you quickly going with a non-GraalVM-based project that allows you to still use Graal to run JS code on a stock JDK.

This is very much a WIP project and currently is mainly a testing ground for me to try and find the most efficient way to set up Graal on a stock JDK.

## Tests

All tests are currently handled by running a one-liner of JavaScript code that computes the first 1000 elements of Fibonacci's sequence and places them into an array. The one liner is as such:

```javascript
new Array(1000)
  .fill(0)
  .reduce((out, _, i) => out.concat(i < 2 ? 1 : out[i - 1] + out[i - 2]), []);
```

At the moment, the results for running the program on OpenJDK 8 with an i7-7700k on Arch Linux are as such:

```
----- Context Creation -----
580.37s total, 58.04ms avg, 55.62ms low (7154), 441.82ms high (0)

----- Single Context -----
578.84s total, 57.88ms avg, 54.97ms low (7667), 85.18ms high (8898)
```

**UPDATE (08/31/19 22:19):** Using the experimental JVMCI compiler option on JDK 11 using the same hardware and system now gives the following results (run `mvn exec:exec` using JDK 11 to test it):

```
----- Context Creation -----
773.17s total, 77.32ms avg, 62.98ms low (8117), 3201.16ms high (6175)

----- Single Context -----
47.92s total, 4.79ms avg, 4.24ms low (9957), 169.21ms high (3)
```

Running the same tests in Node.js using my own custom tool called [benchmarky](https://github.com/Rayzr522/benchmarky) returns the following results:

| NAME               | REP   | TOTAL            | AVG          | LOW     | HIGH      |
| ------------------ | ----- | ---------------- | ------------ | ------- | --------- |
| fibonacci-oneliner | 10000 | 29292.00000000ms | 2.92920000ms | 2ms (5) | 5ms (193) |

_The numbers in parenthesis for low & high are the index of the repetition which resulted in that number._

There is clearly a bit of a difference between the two benchmarks. I am going to continue researching ways to improve the speed of Graal on a stock JDK.
