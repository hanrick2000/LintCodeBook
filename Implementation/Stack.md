###Stack
```java
import java.util.Iterator;

public class MyDeque<Item> implements Iterable<Item>{
	private class Node {
		Node next;
		Node pre;
		Item item;
		Node(Item item) {
			this.item = item;
		}
	}

	private int size = 0;
	private Node head;
	private Node tail;

	public void offerFirst(Item item) {
		Node newNode = new Node(item);
		if (head == null) {
			head = newNode;
			tail = newNode;
		} else {
			newNode.next = head;
			head.pre = newNode;
			head = newNode;
		}
		size++;
	}

	public void offerLast(Item item) {
		Node newNode = new Node(item);
		if (tail == null) {
			head = newNode;
			tail = newNode;
		} else {
			tail.next = newNode;
			newNode.pre = tail;
			tail = newNode;
		}
		size++;

	}

	public Item pollFirst() {
		Node temp = head;
		if (head == null) {
			throw new NullPointerException();
		} else {
			Node temp2 = head;
			head = head.next;
			temp2.next = null;
			head.pre = null;
		}
		size--;
		return temp.item;
	}

	public Item pollLast() {
		Node temp = head;
		if (tail == null) {
			throw new NullPointerException();
		} else {
			Node last = tail.pre;
			tail.pre = null;
			last.next = null;
			tail = last;
		}
		size--;
		return temp.item;
	}

	public boolean isEmpty() {
		return size == 0;
	}

	public int size() {
		return size;
	}

	public Iterator<Item> iterator() {
		return new MyDequeIterator();
	}

	private class MyDequeIterator implements Iterator<Item> {

		Node cur = head;
		int i = size;
		public boolean hasNext() {
			return i != 0;
		}

		public Item next() {
			Node temp = cur;
			cur = cur.next;
			i--;
			return temp.item;
		}
	}

	public static void main(String[] args) {
		MyDeque<String> q = new MyDeque<String>();

		System.out.println("Mystack test");
		q.offerFirst("first");
		q.offerFirst("second");
		q.offerFirst("third");
		for (String s : q) {
			System.out.println(s);
		}
		System.out.println("First test");

		q.pollLast();
		for (String s : q) {
			System.out.println(s);
		}
		System.out.println("Second test");

		q.offerFirst("fourth");
		q.offerFirst("fifth");
		q.pollLast();
		for (String s : q) {
			System.out.println(s);
		}
		System.out.println("Third test");

		q.offerLast("sisth");
		q.offerFirst("seventh");
		q.pollFirst();
		for (String s : q) {
			System.out.println(s);
		}
		System.out.println("Fourth test");
	}

}

```