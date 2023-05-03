Download Link: https://assignmentchef.com/product/solved-cs1332-homework7-patternmatching
<br>
For this assignment you will be coding 3 different pattern matching algorithms: Knuth-Morris-Pratt (KMP), Boyer-Moore, and Rabin-Karp. For all three algorithms, you should find <strong>all </strong>occurrences of the pattern in the text, not just the first match. The occurrences are returned as a list of integers; the list should contain the indices of occurrences in ascending order. There is information about all three algorithms in the javadocs with additional implementation details below. If you implement any of the three algorithms in an unexpected manner (i.e. contrary to what the Javadocs and PDF specify), <strong>you may receive a 0</strong>.

For all of the algorithms, make sure you check the simple failure cases as soon as possible. For example, if the pattern is longer than the text, don’t do any preprocessing on the pattern/text and just return an empty list since there cannot be any occurrences of the pattern in the text.

Note that for pattern matching, we refer to the text length as <em>n </em>and the pattern length as <em>m</em>.

<h2>CharacterComparator</h2>

CharacterComparator is a comparator that takes in two characters and compares them. This allows you to see how many times you have called compare(); besides this functionality, its return values are what you’d expect a properly implemented compare() method to return. You <strong>must </strong>use this comparator as the number of times you call compare() with it will be used when testing your assignment.

If you do not use the passed in comparator, this will cause tests to fail and will significantly lower your grade on this assignment. <strong>You must implement the algorithms as they were taught in class. </strong>We are expecting <strong>exact </strong>comparison counts for this homework. If you are getting fewer comparison counts than expected, it means one of two things: either you implemented the algorithm wrong (most likely) or you are using an optimization not taught in the class (unlikely).

<h2>Knuth-Morris-Pratt</h2>

<h3>Failure Table</h3>

The Knuth-Morris-Pratt (KMP) algorithm relies on using the prefix of the pattern to determine how much to shift the pattern by. The algorithm itself uses what is known as the failure table (also called failure function). Before actually searching, the algorithm generates a failure table. This is an array of length <em>m </em>where each index will correspond to the substring in the pattern up to that index. Each index <em>i </em>of the failure table should contain the length of the longest proper prefix that matches a proper suffix of pattern[0, …, i]. A proper prefix/suffix does not equal the string itself. There are different ways of calculating the failure table, but we are expecting the specific format described below.

For any string pattern, have a pointer i starting at the first letter, a pointer j starting at the second letter, and an array called table that is the length of the pattern. First, set index 0 of table to 0. Then, while j is still a valid index within pattern:

<ul>

 <li>If the characters pointed to by i and j match, then write i + 1 to index j of the table and increment i and j.</li>

 <li>If the characters pointed to by i and j do not match:

  <ul>

   <li>If i is not at 0, then change i to table[i – 1]. Do not increment j or write any value to the table.</li>

   <li>If i is at 0, then write i to index j of the table. Increment only j.</li>

  </ul></li>

</ul>

For example, for the string abacab, the failure table will be:

<table width="137">

 <tbody>

  <tr>

   <td width="23">a</td>

   <td width="23">b</td>

   <td width="23">a</td>

   <td width="23">c</td>

   <td width="23">a</td>

   <td width="23">b</td>

  </tr>

  <tr>

   <td width="23">0</td>

   <td width="23">0</td>

   <td width="23">1</td>

   <td width="23">0</td>

   <td width="23">1</td>

   <td width="23">2</td>

  </tr>

 </tbody>

</table>

For the string ababac, the failure table will be:

<table width="137">

 <tbody>

  <tr>

   <td width="23">a</td>

   <td width="23">b</td>

   <td width="23">a</td>

   <td width="23">b</td>

   <td width="23">a</td>

   <td width="23">c</td>

  </tr>

  <tr>

   <td width="23">0</td>

   <td width="23">0</td>

   <td width="23">1</td>

   <td width="23">2</td>

   <td width="23">3</td>

   <td width="23">0</td>

  </tr>

 </tbody>

</table>

For the string abaababa, the failure table will be:

<table width="183">

 <tbody>

  <tr>

   <td width="23">a</td>

   <td width="23">b</td>

   <td width="23">a</td>

   <td width="23">a</td>

   <td width="23">b</td>

   <td width="23">a</td>

   <td width="23">b</td>

   <td width="23">a</td>

  </tr>

  <tr>

   <td width="23">0</td>

   <td width="23">0</td>

   <td width="23">1</td>

   <td width="23">1</td>

   <td width="23">2</td>

   <td width="23">3</td>

   <td width="23">2</td>

   <td width="23">3</td>

  </tr>

 </tbody>

</table>

For the string aaaaaa, the failure table will be:

<table width="135">

 <tbody>

  <tr>

   <td width="23">a</td>

   <td width="23">a</td>

   <td width="23">a</td>

   <td width="23">a</td>

   <td width="23">a</td>

   <td width="23">a</td>

  </tr>

  <tr>

   <td width="23">0</td>

   <td width="23">1</td>

   <td width="23">2</td>

   <td width="23">3</td>

   <td width="23">4</td>

   <td width="23">5</td>

  </tr>

 </tbody>

</table>

<h3>Searching Algorithm</h3>

For the main searching algorithm, the search acts like a standard brute-force search for the most part, but in the case of a mismatch:

<ul>

 <li>If the mismatch occurs at index 0 of the pattern, then shift the pattern by 1.</li>

 <li>If the mismatch occurs at index j of the pattern and index i of the text, then shift the pattern such that index failure[j-1] of the pattern lines up with index i of the text, where failure is the failure table. Then, continue the comparisons at index i of the text (or index failure[j-1] of the pattern). Do <strong>not </strong>restart at index 0 of the pattern.</li>

</ul>

In addition, if the whole pattern is ever matched, instead of shifting the pattern over by 1 to continue searching for more matches, the pattern should be shifted so that the pattern at index failure[j-1], where j is at pattern.length, aligns with the index after the match in the text. KMP treats a match as a “mismatch” on the character immediately following the match.

<h2>Boyer-Moore</h2>

<h3>Last Occurrence Table</h3>

The Boyer-Moore algorithm, similar to KMP, relies on preprocessing the pattern. Before actually searching, the algorithm generates a last occurrence table. The table allows the algorithm to skip sections of the text, resulting in more efficient string searching. The last occurrence table should be a mapping from each character in the alphabet (the set of all characters that may be in the pattern or the text) to the last index the character appears in the pattern. If the character is not in the pattern, then -1 is used as the value, though you should not explicitly add all characters that are not in the pattern into the table. The getOrDefault() method from Java’s Map will be useful for this.

<h3>Searching Algorithm</h3>

Key properties of Boyer-Moore include matching characters starting at the end of the pattern, rather than the beginning and skipping along the text in jumps of multiple characters rather than searching every single character in the text.

The shifting rule considers the character in the text at which the comparison process failed (assuming that a failure occurred). If the last occurrence of that character is to the left in the pattern, shift so that the pattern occurrence aligns with the mismatched text occurrence. If the last occurrence of the mismatched character does not occur to the left in the pattern, shift the pattern over by one (to prevent the pattern from moving backwards). In addition, if the mismatched character does not exist in the pattern at all (no value in last table) then pattern shifts completely past this point in the text.

For finding multiple occurrences, if you find a match, shift the pattern over by one and continue searching.

<h2>Rabin-Karp</h2>

The Rabin-Karp algorithm relies on hashing to perform pattern matching. This algorithm, instead of using a sophisticated shift / skip through the text, uses a hash function to compare the given pattern with substrings of the text. This algorithm exploits the fact that if two strings are equal, their hash values must also be equal. The algorithm essentially reduces down to computing the hash value of the pattern and then looking for substrings of the text with that hash value. Once a substring of the text with that hash value is found, character by character comparisons are required to ensure equality (as two strings with the same hash may not actually be equal).

<strong>Note</strong>: You must use the exact rolling hash function specified in the javadocs. You are not allowed to use Math.pow() for the intial hash calculation, nor are you allowed to use it for updating the text hash. <strong>This is because exponentiating a number is not an </strong><em>O</em>(1) <strong>operation, so creating your own custom power method is also inefficient.</strong>