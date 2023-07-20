# AutomaticGeneration
---

### Author: Valerii Ovchinnikov 
[(CV)](https://drive.google.com/file/d/1CkrxipsEgZlBD2OGKCbcfrq-AVDAAITN/view?usp=sharing)
---

### TASK 1

Indeed, we can utilize the equals method to compare two arbitrary objects 
since it is a **fundamental** method within the Object class, inherited by 
all other classes. 

However, the challenge arises in that this approach often yields results 
contrary to our expectations. The base class implements this method as 
follows: 
```java
public boolean equals(Object obj) { 
      return this == obj; 
}
```
which checks for reference equality rather than object state equality. 
Hence, it proves unsuitable for most situations. To illustrate, consider a 
scenario where we create two distinct objects with identical states. 

Here, the equals method would return false, even though intuitively, one 
might expect them to be equal due to their identical states.

```java
public class MyObject {
    private int value;
    
    public MyObject(int value) {
        this.value = value;
    }
}

MyObject obj1 = new MyObject(1);
MyObject obj2 = new MyObject(1);

System.out.println(obj1.equals(obj2));  // false
```
---

### TASK 2
Now, I will provide some strategies to compare two arbitrary objects:

1) **Override equals() and hashcode() methods.** 
It is not sufficient to compare arbitrary objects using only the equals 
method. Thus, the golden rule is: if you override equals, you should also 
override hashcode. After comparison, we typically override equals to first 
check if the objects are the same reference (using the basic == operator), 
then confirm that it is an instance of our class, and finally compare the 
states of the objects.

 - Pros: Provides the means to define what object equality entails.
 - Cons: Requires manual implementation for each class.

2) **Reflection** 
The primary approach to comparing objects is using the Reflection API 
(especially since the objects are arbitrary). Reflection provides 
information about the fields, methods, constructors, and modifiers, and it 
usually works in tandem with java.lang.Class. 

	Here, we can retrieve any methods and fields, along with their 
names and return values. This information allows us to determine whether 
the classes are equal with ease.

- Pros: compare any objects without writing class-specific code (the main 
task) 
- Cons: Performance overhead: reflection can add significant overhead to 
your program, requiring extra processing to inspect and manipulate the 
code structure at runtime. This can lead to slower performance, especially 
in large and complex applications.

---

### TASK 3
Even though we've discussed how to compare arbitrary objects, in this 
case, we know the specific class we aim to compare. Therefore, I won't use 
reflection or any other method to determine equality. Instead, I'll first 
check if the size() of the binary trees is equal, and if so, I'll ensure 
that every node in the same position has identical values. 

(Usually, the left node is always smaller than the parent, and the right 
node is always larger. If we need to compare a BinaryTree and a 
BinaryTreeInv (where the right leaf is smaller), we could call a 
normalise() method that changes all nodes in the tree in O(n) time 
complexity.) 

*Remark: I did not call the size function because we should make it static 
if we want to call it from static method, and the assyptotic is still 
O(n)*

```java
class BinaryTree { 
	int value;
	BinaryTree left; 
	BinaryTree right;
	
	static boolean contentsSimilar(BinaryTree lhv, BinaryTree rhv) { 
		if (lhv == null && rhv == null) {
			return true;
		}
		
		if (lhv != null && rhv != null) {
			return contentsSimilar(lhv.left, rhv.left) &&
				contentsSimilar(lhv.right, rhv.right);
		}
		
		return false;
	}
// You can consider that these methods are implemented 
// and you can use them if needed

boolean contains(int value);
boolean add(int value);
boolean remove(int value);
int size();

}
```

