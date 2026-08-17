Bilkul. Agar aap **C#/.NET interview** ke liye Collections aur Data Structures prepare kar rahi ho, to interviewer generally **List, Array, Generic/Non-Generic, LinkedList, Dynamic, Dictionary, Stack, Queue, IEnumerable** etc. se questions pooch sakta hai.

Main aapko **basic → intermediate → advanced → coding questions** ke order mein list de raha hoon.

## 1. Collections ke Basic Questions

1. Collection kya hoti hai?
2. C# mein Collections ki zarurat kyon hoti hai?
3. Array aur Collection mein kya difference hai?
4. `Array` kya hai?
5. Array ki limitation kya hai?
6. Collection aur Data Structure mein kya difference hai?
7. C# mein collection ke different types kaun-kaun se hain?
8. Generic Collection kya hoti hai?
9. Non-Generic Collection kya hoti hai?
10. Generic aur Non-Generic Collection mein difference?
11. Generic collections ko prefer kyon karte hain?
12. Boxing aur Unboxing kya hota hai?
13. Generic collections boxing/unboxing ko kaise avoid karti hain?

---

# 2. Generic Collections

Ye **bahut important interview topic** hai.

### Important classes:

* `List<T>`
* `Dictionary<TKey,TValue>`
* `HashSet<T>`
* `Queue<T>`
* `Stack<T>`
* `LinkedList<T>`
* `SortedList<TKey,TValue>`
* `SortedDictionary<TKey,TValue>`

### Questions

14. Generic Collection kya hai?

15. `List<T>` kya hai?

16. `List<T>` internally kaise work karti hai?

17. `List<T>` aur Array mein difference?

18. `List<T>` ki default capacity kya hoti hai?

19. List ki capacity aur Count mein difference?

20. List ki capacity full hone par kya hota hai?

21. `List<T>.Add()` internally kaise work karta hai?

22. `Add()` aur `AddRange()` mein difference?

23. `List<T>` mein element remove kaise karenge?

24. `Remove()` vs `RemoveAt()`?

25. `Remove()` vs `RemoveAll()`?

26. `Clear()` kya karta hai?

27. `List<T>` mein searching kaise karenge?

28. `Contains()` kaise work karta hai?

29. `Find()` aur `FindAll()` mein difference?

30. `List<T>` mein sorting kaise karenge?

31. `Sort()` kaise work karta hai?

---

# 3. Non-Generic Collections

Important classes:

* `ArrayList`
* `Hashtable`
* `Stack`
* `Queue`

### Interview Questions

32. Non-Generic Collection kya hoti hai?

33. `ArrayList` kya hai?

34. `ArrayList` aur `List<T>` mein difference?

35. `ArrayList` ko avoid kyon karte hain?

36. `ArrayList` mein boxing/unboxing kaise hoti hai?

37. `Hashtable` kya hai?

38. `Hashtable` aur `Dictionary<TKey,TValue>` mein difference?

39. Non-generic `Stack` kya hai?

40. Generic `Stack<T>` aur non-generic `Stack` mein difference?

41. Non-generic `Queue` kya hai?

42. Generic `Queue<T>` aur non-generic `Queue` mein difference?

---

# 4. Dynamic / Dynamic Collections

Yahan ek important distinction samajhna:

### `dynamic` ≠ Dynamic Collection

Interview mein ye question aa sakta hai:

43. `dynamic` keyword kya hai?

44. `var` aur `dynamic` mein difference?

45. `dynamic` aur `object` mein difference?

46. Kya `dynamic` collection hoti hai?

47. Dynamic size collection ka example kya hai?

Example:

```csharp
List<int> numbers = new List<int>();

numbers.Add(10);
numbers.Add(20);
numbers.Add(30);
```

Yahan List ka size runtime par grow ho sakta hai.

### Interview mein bolna:

> "C# mein List<T> dynamically grow hone wali generic collection hai, jabki dynamic keyword compile-time type checking ko runtime par defer karta hai. Dono alag concepts hain."

Ye **very important distinction** hai.

---

# 5. LinkedList

Ye bhi important hai.

48. LinkedList kya hoti hai?

49. LinkedList aur Array mein difference?

50. LinkedList aur List mein difference?

51. LinkedList internally kaise work karti hai?

52. Node kya hota hai?

53. Singly Linked List kya hai?

54. Doubly Linked List kya hai?

55. Circular Linked List kya hai?

56. C# ka `LinkedList<T>` kis type ki linked list hai?

57. `LinkedList<T>` mein `LinkedListNode<T>` kya hai?

58. LinkedList mein beginning mein element kaise add karenge?

59. End mein element kaise add karenge?

60. `AddFirst()` aur `AddLast()` mein difference?

61. `AddBefore()` aur `AddAfter()` kya karte hain?

62. LinkedList mein searching kaise hoti hai?

63. LinkedList mein random access slow kyon hota hai?

64. `list[5]` LinkedList mein kyon nahi kar sakte?

65. LinkedList ka advantage kya hai?

66. LinkedList ka disadvantage kya hai?

---

# 6. List vs LinkedList ⭐

Ye direct interview question hai.

| Feature            | List<T>                     | LinkedList<T>              |
| ------------------ | --------------------------- | -------------------------- |
| Internal structure | Dynamic Array               | Nodes                      |
| Index access       | Fast                        | Slow                       |
| Random access      | Yes                         | No                         |
| Memory             | Less overhead               | More overhead              |
| Insert middle      | Costly                      | Node available ho to fast  |
| Search             | Fast compared to LinkedList | Sequential                 |
| Best use           | Frequent read/index access  | Frequent insertion/removal |

### Interview Question:

**67. List aur LinkedList mein se kab kya use karenge?**

Answer:

> Agar mujhe index-based access aur frequent reading chahiye to `List<T>` use karunga. Agar mujhe known nodes ke around frequent insertion/removal karni hai aur random access ki requirement nahi hai, to `LinkedList<T>` use karunga.

---

# 7. Dictionary

Very important for .NET interviews.

68. Dictionary kya hoti hai?

69. `Dictionary<TKey,TValue>` kya hai?

70. Dictionary internally kaise work karti hai?

71. Dictionary mein Key aur Value kya hote hain?

72. Dictionary mein duplicate key allowed hai?

73. Dictionary mein duplicate values allowed hain?

74. Dictionary aur List mein difference?

75. Dictionary aur Hashtable mein difference?

76. Dictionary mein data kaise add karenge?

```csharp
Dictionary<int, string> students = new();

students.Add(1, "Rahul");
students.Add(2, "Amit");
```

77. Dictionary se value kaise retrieve karenge?

```csharp
var name = students[1];
```

78. `ContainsKey()` kya karta hai?

79. `ContainsValue()` kya karta hai?

80. Dictionary mein duplicate key add karne par kya hoga?

81. `TryGetValue()` kya hai?

82. `TryGetValue()` ko indexer se prefer kab karenge?

---

# 8. HashSet

83. `HashSet<T>` kya hai?

84. HashSet aur List mein difference?

85. HashSet duplicate values ko kaise handle karta hai?

86. HashSet mein duplicate element add karne par kya hota hai?

87. HashSet ka use kab karenge?

Example:

```csharp
HashSet<int> numbers = new();

numbers.Add(10);
numbers.Add(20);
numbers.Add(10);
```

Result:

```text
10
20
```

---

# 9. Stack

90. Stack kya hai?

91. Stack kis principle par work karta hai?

**LIFO — Last In First Out**

92. `Push()` kya karta hai?

93. `Pop()` kya karta hai?

94. `Peek()` kya karta hai?

Example:

```csharp
Stack<int> stack = new();

stack.Push(10);
stack.Push(20);
stack.Push(30);
```

Output of `Pop()`:

```text
30
```

95. Stack ka real-life example?

96. Stack ka programming mein use?

Examples:

* Undo/Redo
* Browser history
* Recursion
* Expression evaluation

---

# 10. Queue

97. Queue kya hai?

98. Queue kis principle par work karti hai?

**FIFO — First In First Out**

99. `Enqueue()` kya hai?

100. `Dequeue()` kya hai?

101. `Peek()` kya hai?

Example:

```csharp
Queue<int> queue = new();

queue.Enqueue(10);
queue.Enqueue(20);
queue.Enqueue(30);
```

`Dequeue()`:

```text
10
```

102. Queue ka real-life example?

Examples:

* Printer queue
* Customer queue
* Job processing
* Background tasks

---

# 11. IEnumerable / ICollection / IList

.NET interviews mein **bahut important**.

103. `IEnumerable<T>` kya hai?

104. `ICollection<T>` kya hai?

105. `IList<T>` kya hai?

106. IEnumerable aur List mein difference?

107. ICollection aur IEnumerable mein difference?

108. IList aur ICollection mein difference?

109. `IEnumerable<T>` mein Add method kyon nahi hota?

110. `IEnumerable<T>` ka use kab karenge?

111. Deferred Execution kya hai?

112. Immediate Execution kya hai?

113. `ToList()` kya karta hai?

114. `IEnumerable` aur `IQueryable` mein difference?

---

# 12. Very Important Comparison Questions ⭐⭐⭐

Interview mein ye comparisons prepare karo:

115. Array vs List

116. List vs LinkedList

117. List vs ArrayList

118. Generic vs Non-Generic

119. List vs HashSet

120. List vs Dictionary

121. Dictionary vs Hashtable

122. Stack vs Queue

123. IEnumerable vs ICollection

124. ICollection vs IList

125. IEnumerable vs IQueryable

126. `var` vs `dynamic`

127. `object` vs `dynamic`

128. Array vs ArrayList

---

# 13. Time Complexity Questions

Thoda advanced interview mein poocha ja sakta hai.

129. Array mein searching ki complexity?

130. List mein searching ki complexity?

131. Dictionary lookup ki average complexity?

132. HashSet lookup ki average complexity?

133. Stack Push/Pop ki complexity?

134. Queue Enqueue/Dequeue ki complexity?

135. LinkedList insertion ki complexity?

136. LinkedList search ki complexity?

Basic idea:

| Operation          | Typical Complexity |
| ------------------ | -----------------: |
| Array index access |               O(1) |
| List index access  |               O(1) |
| Dictionary lookup  |       O(1) average |
| HashSet lookup     |       O(1) average |
| Stack Push         |               O(1) |
| Stack Pop          |               O(1) |
| Queue Enqueue      |               O(1) |
| Queue Dequeue      |               O(1) |
| LinkedList search  |               O(n) |

---

# 14. Coding Questions ⭐⭐⭐

Interviewer practical coding bhi pooch sakta hai:

137. List mein duplicate elements find karo.

138. List se duplicate elements remove karo.

139. List ko reverse karo.

140. List mein maximum number find karo.

141. List mein minimum number find karo.

142. Second highest number find karo.

143. Dictionary mein word frequency count karo.

144. String mein character frequency count karo.

145. Stack use karke string reverse karo.

146. Stack use karke parentheses validate karo.

147. Queue implement karo.

148. Stack implement karo.

149. Singly LinkedList implement karo.

150. LinkedList ko reverse karo.

151. LinkedList mein middle element find karo.

152. LinkedList mein cycle detect karo.

153. Two Lists ko merge karo.

154. List ko sort without built-in `Sort()`.

155. Array se duplicate values remove karo.

---

# 15. Scenario-Based Questions

Ye experienced .NET developer ke interview mein kaafi useful hain:

156. Agar **1 lakh records** store karne hain to List ya Dictionary kya choose karoge?

157. Agar duplicate data nahi chahiye to kaunsi collection use karoge?

158. Agar Key ke through fast search karna hai to kya use karoge?

159. Agar data FIFO order mein process karna hai to kya use karoge?

160. Agar data LIFO order mein process karna hai to kya use karoge?

161. Agar index-based access chahiye to kya use karoge?

162. Agar frequently middle mein insert/delete karna hai to kya use karoge?

163. Agar read operations bahut zyada hain to kaunsi collection suitable hogi?

164. Agar unique IDs ke saath objects store karne hain to kya use karoge?

165. Database se records return karne ke liye `List<T>` vs `IEnumerable<T>` kab use karoge?

---

## 🎯 Aapke .NET Interview ke liye Priority

Aapko sabse pehle ye **15 topics strongly prepare** karne chahiye:

1. ⭐ Array
2. ⭐ List<T>
3. ⭐ Generic vs Non-Generic
4. ⭐ ArrayList
5. ⭐ LinkedList
6. ⭐ Dictionary
7. ⭐ HashSet
8. ⭐ Stack
9. ⭐ Queue
10. ⭐ IEnumerable
11. ⭐ ICollection
12. ⭐ IList
13. ⭐ IQueryable
14. ⭐ `var` vs `dynamic`
15. ⭐ Collection time complexity

**Next level:** In sabko sirf definitions se nahi, balki **"internally kaise work karta hai + kab use karenge + real project example + coding question"** ke saath prepare karo. Ye approach .NET backend interview ke liye zyada useful rahegi.
