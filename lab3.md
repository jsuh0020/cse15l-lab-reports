# Lab Report 3 - Bugs and Commands

## Part 1 - Bugs

For my bug, I chose to analyze the `averageWithoutLowest` method in the `ArrayExamples.java` file.

### Failure Inducing JUnit Test

```java
  @Test
  public void testAverageWithoutLowestFails() {
    double[] input = { 1.0, 1.0, 3.0, 5.0 };
    assertEquals(3.0, ArrayExamples.averageWithoutLowest(input), 0.00001);
  }
```
### Passing JUnit Test

```java
  @Test
  public void testAverageWithoutLowestPasses() {
    double[] input = { 1.0, 2.0, 3.0, 4.0 };
    assertEquals(3.0, ArrayExamples.averageWithoutLowest(input), 0.00001);
  }
```

### The Symptom

![Image](/lab3_images/lab3_1.png)

### The Bug

Before:

```java
 static double averageWithoutLowest(double[] arr) {
   if (arr.length < 2) {
     return 0.0;
   }
   double lowest = arr[0];
   for (double num : arr) {
     if (num < lowest) {
       lowest = num;
     }
   }
   double sum = 0;
   for (double num : arr) {
     sum += num;
   }
   return (sum - lowest) / (arr.length - 1);
 }
```

After:

```java
 static double averageWithoutLowest(double[] arr) {
   if (arr.length < 2) {
     return 0.0;
   }
   double lowest = arr[0];
   for (double num : arr) {
     if (num < lowest) {
       lowest = num;
     }
   }
   double sum = 0;
   for (double num : arr) {
     if (num != lowest) {
       sum += num;
     }
   }
   return sum / (arr.length - 1);
 }

```

## Part 2 - Researching Commands
