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

### Case Insensitive Search (`grep -i`)

Source: `grep` manual

**Example 1: Searching For "abbreviations" Without Case Sensitivity**

```
$ grep -i "abbreviations" technical/biomed/rr74.txt
        Abbreviations
```

By default, the `grep` command is case sensitive, but by adding the `-i` flag onto the grep command, you're searching for the pattern without case sensitivity. That is why when I searched for "abbreviations", "Abbreviations" was given as output, although it is uppercase, which is useful since I still wanted to search for the word "abbreviations".

**Example 2: Searching For "recent" Without Case Sensitivity**

```
$ grep -i "recent" technical/biomed/rr166.txt 
        recently evolved [ 1 2 ] . Lung fibroblasts respond, 
        Recent reports suggesting decreased COX-2 expression and
```

The `-i` flag searches the file for both lowercase and uppercase strings that match the string you're searching for. This is why there were two results when I searched "recent", although one is uppercase and one is lowercase. This command could be useful when you want any part of the file containing the pattern, regardless of case sensitivity.

### Maximum Number of Matches (`grep -m`)

Source: `grep` manual

**Example 1: Searching For First Occurrence of Pattern**

```
$ grep -m 1 "the" technical/biomed/ar68.txt
        autoimmune disease of unknown etiology. Although the
```

`grep -m` takes in an argument for how many occurrences of the pattern that should be outputted. In this case, we are searching for one occurrence of "the" in `ar68.txt`, and by limiting the number of outputs to one, you can easily find the first occurrence of a pattern in a given file.

**Example 2: Searching For First 10 Occurrences of Pattern**

```
$ grep -m 10 "the" technical/biomed/ar68.txt
        autoimmune disease of unknown etiology. Although the
        disease is characterized by synovitis of the joints, tendon
        sheaths, and bursae, manifestations that do not involve the
        immunologic processes [ 2]. The hallmarks of the synovial
        infiltration involving the myeloid, macrophage, and
        controversy regarding the relative importance of the types
        of cells and their products involved in the inflammatory
        processes of rheumatoid arthritis [ 3]. Nevertheless, it
        seems likely that all of these cell types participate to
        arthritis involves the description of restricted subsets of
```

This time, `grep -m` is followed by the argument 10, meaning that we are searching for the first 10 occurrences of "the" in `ar68.txt`. This might be useful if you only need to look at a certain number of occurrences of a pattern  in a file, which can help reduce clutter in the output.

### Line Numbers With Matching (`grep -n`)

Source: `grep` manual

**Example 1: Printing Matching Strings With Line Number**

```
$ grep -n "bio" /technical/biomed/ar104.txt
11:          Recent research has identified certain biologic agents
15:          the use of biologic agents for arthritis therapy is the
161:        identified certain biologic agents that appear more able
168:        with the use of biologic agents for arthritis therapy is
331:          biotin-conjugated antimouse whole IgG (heavy and light
```

`grep -n` prints out the line in which the pattern was found along with the contents of the line. This could be useful if you have to cite a document and need a specific line where something was said, or if you have to revisit the file and need context around where the pattern was found, and you could read the file around the outputted line numbers.

**Example 2: Counting Number of Lines With Pattern**

```
$ grep -n "bio" /technical/biomed/ar104.txt | wc -l
5
```

This command is the same as the one in example 1, except we're piping the output into `wc -l`, which checks how many lines there are (in this case 5). This could be useful if you need data on how many lines contain a certain pattern.

### Match Everything Besides Specified Pattern (`grep -v`)

Source: `grep` manual

**Example 1: Finding Lines Without "a" In Them**

```
$ grep -v "a" /technical/plos/pmed.0010056.txt







        Why Open Source?
        full. Furthermore, it would use open-source licenses to keep its discoveries freely
      

        How It Works
        code.
        more powerful in the future.
        drugs.


        scientists.
        expire [12].


        West.


        Next Steps


        Conclusion
```

`grep -v` prints out all the lines in the file that DON'T have the pattern you searched for, which is why all the lines in the output don't have "a" in them. This could be useful if a file you're searching through is very large, and you only want certain portions of the file that don't have the pattern in them.

**Example 2: Finding All Files Without "chapter" in Their Name**

```
$ find technical/911report -type f | grep -v "chapter"
./preface.txt
```

This command searches through the `technical/911report` directory and outputs any files that don't have "chapter" in their name. This application of `grep -v` is useful if theres a lot of files in a directory and you are trying to filter out files with irrelevant information based on the title of the file.
