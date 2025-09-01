# Adapter Design Pattern

## Overview
The **Adapter Pattern** is a **structural design pattern** that allows two incompatible interfaces or systems to work together.  
It acts as a **bridge** between your system and a third-party or legacy system.

---

## Why Use Adapter Pattern?

- To **connect incompatible systems** without modifying their source code.  
- To **adapt data formats** between systems (e.g., JSON → XML).  
- To **reuse existing classes** in a different interface.

**Example Scenario:**  
- Your system expects JSON input, but a third-party service provides XML.  
- An **adapter** converts XML to JSON so your system can process it without changes.

---

## Types of Adapter Pattern

### Class-Level Adapter
- Uses **inheritance** to adapt one interface to another.  
- Creates **tight coupling** between adapter and adaptee.

### Object-Level Adapter
- Uses **composition (has-a relationship)** to adapt an object.  
- Provides **flexibility** and looser coupling.

---

## Real-World Examples

- **Java API**
  - `InputStreamReader` converts a byte stream to a character stream.  
  - `OutputStreamWriter` converts a character stream to a byte stream.

- **Collections Example**
  - `Arrays.asList()` converts an array into a list, acting as an adapter.

- **Business Scenario**
  - A food delivery platform wants to also sell groceries temporarily.  
  - The grocery system has a different interface.  
  - An **adapter** converts grocery items to the food item interface, allowing the platform to add groceries seamlessly.

---

## How It Works (Step by Step)

1. **Existing System** – Already implemented (e.g., `FoodItem`).  
2. **Incompatible System** – Needs to be adapted (e.g., `GroceryItem`).  
3. **Adapter Class** – Implements the target interface (`Item`) and wraps the incompatible object (`GroceryItem`).  
4. **Client** – Uses the adapter as if it were the target interface.

---

## Benefits

- Promotes **reusability** of existing classes.  
- **Decouples client code** from the incompatible system.  
- Simplifies **integration** with third-party or legacy systems.  
- Can be applied **without modifying existing code**.

---

## Key Points for Freshers

- Adapter pattern is about **interface compatibility**, not object creation.  
- Prefer **object-level adapters** for flexibility.  
- Useful in **legacy system integration** or **third-party API integration**.  
- Helps in **extending functionality** without changing existing code.

---

## References

- [JavaDevJournal: Adapter Design Pattern](https://www.javadevjournal.com/java-design-patterns/adapter-design-pattern/)  
- [ProgrammerGirl: Java Adapter Pattern](https://www.programmergirl.com/java-adapter-pattern/)  
- [CECS Wright: Adapter Pattern Explanation](https://cecs.wright.edu/~tkprasad/courses/ceg860/paper/node26.html)
