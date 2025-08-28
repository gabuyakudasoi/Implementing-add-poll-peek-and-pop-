# Implementing-add-poll-peek-and-pop-
Implementing add(), poll(), peek(), and pop()
public class ArrayLinkedList {
    private static final int INITIAL_CAPACITY = 10;
    private int[] data;  // Array to store node values (values of list)
    private int[] next;  // Array to store the next element's index (simulating the link)
    private int size;
    private int head;  // Index of the first element
    private int freeIndex;  // Next available index in the arrays
    
    // Constructor
    public ArrayLinkedList() {
        data = new int[INITIAL_CAPACITY];
        next = new int[INITIAL_CAPACITY];
        size = 0;
        head = -1;
        freeIndex = 0;
    }

    // Add a new element to the end of the list
    public void add(int value) {
        if (freeIndex >= data.length) {
            expandArrays();  // Expand the arrays if space is full
        }

        data[freeIndex] = value;  // Store value at freeIndex
        next[freeIndex] = -1;  // Indicate this is the last element in the list
        
        if (size == 0) {
            head = freeIndex;  // The first element becomes the head
        } else {
            int current = head;
            while (next[current] != -1) {
                current = next[current];  // Traverse to the last element
            }
            next[current] = freeIndex;  // Link last element to the new one
        }

        size++;
        freeIndex++;
    }

    // Removes the first element (head) and returns it
    public int poll() {
        if (size == 0) {
            throw new IllegalStateException("List is empty.");
        }

        int value = data[head];  // Store the value at head
        head = next[head];  // Move head to the next element in the list

        size--;  // Decrease the size
        return value;  // Return the removed element's value
    }

    // Returns the first element without removing it
    public int peek() {
        if (size == 0) {
            throw new IllegalStateException("List is empty.");
        }
        return data[head];  // Return the value at the head
    }

    // Removes the first element and returns it (same as poll)
    public int pop() {
        return poll();  // pop is the same as poll, so it simply calls poll()
    }

    // Expands arrays when we run out of space
    private void expandArrays() {
        int newCapacity = data.length * 2;
        int[] newData = new int[newCapacity];
        int[] newNext = new int[newCapacity];

        // Copy the old arrays to the new ones
        System.arraycopy(data, 0, newData, 0, data.length);
        System.arraycopy(next, 0, newNext, 0, next.length);

        // Update the references to point to the new arrays
        data = newData;
        next = newNext;
    }

    // Helper method to check size
    public int size() {
        return size;
    }

    // Check if list is empty
    public boolean isEmpty() {
        return size == 0;
    }

    // Display the list
    public void display() {
        if (isEmpty()) {
            System.out.println("List is empty.");
            return;
        }

        int current = head;
        while (current != -1) {
            System.out.print(data[current] + " -> ");
            current = next[current];  // Move to the next element
        }
        System.out.println("null");
    }

    // Main method to test the linked list
    public static void main(String[] args) {
        ArrayLinkedList list = new ArrayLinkedList();  // Create a new linked list

        list.add(10);  // Add elements to the list
        list.add(20);
        list.add(30);
        list.add(40);

        list.display();  // Output: 10 -> 20 -> 30 -> 40 -> null

        System.out.println("Peek: " + list.peek());  // Output: 10
        System.out.println("Poll: " + list.poll());  // Output: 10

        list.display();  // Output: 20 -> 30 -> 40 -> null

        System.out.println("Pop: " + list.pop());  // Output: 20

        list.display();  // Output: 30 -> 40 -> null
    }
}
