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

**Before:**

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

**After:**

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

The issue with the original method was that it wasn't accounting for a scenario where duplicates of the lowest number were present. In the for loop of the original method, we check that `num != lowest`, such that all duplicates of the lowest number are also not accounted for. To fix this, I changed the for loop to have the sum add up ALL numbers in the array, and accounted for removing only ONE copy of the lowest number in my return statement with `(sum - lowest)`.

## Part 2 - Researching Commands
