Problem: Design a HashMap without using any built-in hash table libraries.

Implement the MyHashMap class:

MyHashMap() initializes the object with an empty map. \
void put(int key, int value) inserts a (key, value) pair into the HashMap. If the key already exists in the map, update the corresponding value. \
int get(int key) returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key. \
void remove(key) removes the key and its corresponding value if the map contains the mapping for the key. \

Example 1: \

Input \
["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"] \
[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]] \
Output \
[null, null, null, 1, -1, null, 1, null, -1] \
 
Explanation \
MyHashMap myHashMap = new MyHashMap(); \
myHashMap.put(1, 1); // The map is now [[1,1]] \
myHashMap.put(2, 2); // The map is now [[1,1], [2,2]] \ 
myHashMap.get(1);    // return 1, The map is now [[1,1], [2,2]] \
myHashMap.get(3);    // return -1 (i.e., not found), The map is now [[1,1], [2,2]] \
myHashMap.put(2, 1); // The map is now [[1,1], [2,1]] (i.e., update the existing value) \
myHashMap.get(2);    // return 1, The map is now [[1,1], [2,1]] \
myHashMap.remove(2); // remove the mapping for 2, The map is now [[1,1]] \
myHashMap.get(2);    // return -1 (i.e., not found), The map is now [[1,1]] \
 

Constraints: \

0 <= key, value <= 106 \
At most 104 calls will be made to put, get, and remove.

Solution:
```
class MyHashMap {

    class Node {
        int key;
        int value;
        Node next;

        public Node(int key, int value) {
            this.key = key;
            this.value = value;
            this.next = null;
        }
    }

    private static final int SIZE = 1000;
    private Node[] nodes;
    
    public MyHashMap() {
        this.nodes = new Node[SIZE];
    }

    public void put(int key, int value) {
        int index = getIndex(key);
        if(nodes[index] == null) {
            nodes[index] = new Node(-1, -1);
        }
        Node prev = getNode(nodes[index], key);
        if(prev == null || prev.next == null) {
            prev.next = new Node(key, value);
        }
        prev.next.value = value;
    }
    
    public int get(int key) {
        int index = getIndex(key);
        if(nodes[index] == null) {
            return -1;
        }
        Node prev = getNode(nodes[index], key);
        if(prev == null || prev.next == null) {
            return -1;
        }
        return prev.next.value;
    }
    
    public void remove(int key) {
        int index = getIndex(key);
        if(nodes[index] == null) {
            return;
        }

        Node prev = getNode(nodes[index], key);
        if(prev == null || prev.next == null) {
            return;
        }
        prev.next = prev.next.next;
    }

    private int getIndex(int key) {

        return Integer.hashCode(key) % SIZE;
    }

    private Node getNode(Node head, int key) {
        Node prev = null;
        while(head != null || head.key != key) {
            prev = head;
            head = head.next;
        }
        return prev;
    }
}
```

`TimeComplexity`: O(N/B), where N is the number of all possible keys and B is the number of buckets.\
`SpaceComplexity`: O(M + N), where M is the given element size and N is the bucket size which is 10000 here.
