# Bounded Blocking Queue (Concurrency)

## Problem Description
Implement a thread-safe **Bounded Blocking Queue**. The queue has a maximum capacity.
- `enqueue(element)`: Adds an element to the queue. If the queue is full, the calling thread must block until space becomes available.
- `dequeue()`: Removes and returns an element from the queue. If the queue is empty, the calling thread must block until an element becomes available.
- `size()`: Returns the current number of elements in the queue.

## Approach
To make the queue thread-safe and handle the blocking behavior, we use **Locks** and **Condition Variables**.

1.  **Mutual Exclusion**: Use a `Lock` to ensure only one thread can modify the internal queue (e.g., a `collections.deque`) at a time.
2.  **Condition Variables**:
    - `not_full`: Used by `enqueue`. Threads wait on this condition if the queue is at capacity. They are notified by `dequeue`.
    - `not_empty`: Used by `dequeue`. Threads wait on this condition if the queue is empty. They are notified by `enqueue`.
3.  **Looping vs If**: Always use a `while` loop when checking the condition (e.g., `while len(queue) == capacity`) before waiting, to handle "spurious wakeups".

## Code Implementation (Python)

```python
import threading
from collections import deque
import time

class BoundedBlockingQueue:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.queue = deque()
        self.lock = threading.Lock()
        # Condition variables
        self.not_full = threading.Condition(self.lock)
        self.not_empty = threading.Condition(self.lock)

    def enqueue(self, element: int) -> None:
        with self.not_full:
            # Wait while the queue is full
            while len(self.queue) == self.capacity:
                self.not_full.wait()
            
            self.queue.append(element)
            # Notify any threads waiting to dequeue
            self.not_empty.notify()

    def dequeue(self) -> int:
        with self.not_empty:
            # Wait while the queue is empty
            while len(self.queue) == 0:
                self.not_empty.wait()
            
            element = self.queue.popleft()
            # Notify any threads waiting to enqueue
            self.not_full.notify()
            return element

    def size(self) -> int:
        with self.lock:
            return len(self.queue)

# --- Test Cases ---
def producer(q, items):
    for item in items:
        print(f"Producing {item}")
        q.enqueue(item)
        time.sleep(0.1) # Simulate work

def consumer(q, count):
    for _ in range(count):
        item = q.dequeue()
        print(f"Consuming {item}")
        time.sleep(0.5) # Simulate slow consumption

if __name__ == "__main__":
    q = BoundedBlockingQueue(2) # Capacity of 2

    t1 = threading.Thread(target=producer, args=(q, [1, 2, 3, 4, 5]))
    t2 = threading.Thread(target=consumer, args=(q, 5))

    t1.start()
    t2.start()

    t1.join()
    t2.join()
    print("Simulation finished.")
```

## Complexity Analysis
- **Time Complexity**: 
    - `enqueue`: $O(1)$ amortized for deque operation, but thread may block.
    - `dequeue`: $O(1)$ amortized for deque operation, but thread may block.
- **Space Complexity**: $O(C)$ where $C$ is the capacity of the queue.

