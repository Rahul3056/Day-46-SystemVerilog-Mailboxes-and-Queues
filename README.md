### **Day 46: SystemVerilog Mailboxes and Queues**

---

### **Objective:**

Understand how **mailboxes** and **queues** are used for **inter-process communication** and **dynamic data storage** in SystemVerilog, especially in testbenches.

---

## ✅ Part 1: **Mailboxes**

### **1. What is a Mailbox?**

A **mailbox** is a built-in SystemVerilog class used for **synchronization** and **message passing** between concurrent processes (e.g., threads or modules). Think of it as a queue that processes can use to **send** and **receive data safely**.

---

### **2. Syntax**

```systemverilog
mailbox mb;             // Unbounded mailbox
mailbox #(type) mb;     // Parameterized (optional in usage)
```

### **3. Common Methods**

| Method          | Description                             |
| --------------- | --------------------------------------- |
| `put(item)`     | Sends (inserts) item into mailbox       |
| `get(item)`     | Blocks until an item is available       |
| `try_get(item)` | Non-blocking get                        |
| `peek(item)`    | Read without removing (blocks if empty) |
| `num()`         | Returns number of items in mailbox      |

---

### **4. Example: Using a Mailbox**

```systemverilog
module tb_mailbox;

    mailbox mb = new();

    initial begin
        fork
            begin
                int data;
                #10;
                mb.put(42);
                $display("Producer sent: 42");
            end

            begin
                int data;
                mb.get(data); // Waits for data
                $display("Consumer received: %0d", data);
            end
        join
    end

endmodule
```

**Output:**

```
Producer sent: 42
Consumer received: 42
```

---

## ✅ Part 2: **Queues**

### **1. What is a Queue?**

A **queue** is a variable-size, ordered collection of elements. Unlike arrays, queues can **grow** and **shrink dynamically**. Useful in testbenches to store a list of transactions, stimuli, etc.

---

### **2. Syntax**

```systemverilog
data_type queue_name[$];       // Unbounded queue
data_type queue_name[initial]; // Fixed initial size
```

---

### **3. Common Operations**

| Operation         | Description                              |
| ----------------- | ---------------------------------------- |
| `push_front(val)` | Add element at the front                 |
| `push_back(val)`  | Add element at the back (most common)    |
| `pop_front()`     | Remove and return element from the front |
| `pop_back()`      | Remove and return element from the back  |
| `delete(index)`   | Delete specific element                  |
| `queue.size()`    | Get number of elements                   |

---

### **4. Example: Using a Queue**

```systemverilog
module tb_queue;

    int q[$]; // Declare queue of integers

    initial begin
        q.push_back(5);
        q.push_back(10);
        q.push_back(15);

        $display("Queue contents: %p", q);
        int val = q.pop_front();
        $display("Popped front: %0d", val);
        $display("Queue now: %p", q);
    end

endmodule
```

**Output:**

```
Queue contents: '{5, 10, 15}
Popped front: 5
Queue now: '{10, 15}
```

---

## 🧠 Summary

| Concept     | Use Case                                |
| ----------- | --------------------------------------- |
| **Mailbox** | Synchronizing data between threads      |
| **Queue**   | Temporarily storing dynamic data        |
| Blocking?   | Mailbox: yes (e.g., `get()`), Queue: no |

---

