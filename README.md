
file:///C:/Users/venka/OneDrive/Videos/Documents/Desktop/split-the-bill/naveen.html


# Split The Bill Project 

## Project Title: Smart Bill Splitter for Friends

## 1. Introduction

When friends go to a hotel or restaurant, everyone eats food together. After eating, the waiter gives one total bill. It becomes difficult to calculate how much each person should pay.

This project is a **Bill Splitter Application** that automatically divides the bill among all members equally or with extra charges like tax and tip.

Example:
Total Bill = ₹9000
Members = 9
Each Person Pays = ₹1000

This project is useful for trips, parties, restaurants, cafes, and group dinners.

---

## 2. Objectives

* Calculate bill quickly
* Divide bill equally among friends
* Reduce confusion during payment
* Add tax / tip if needed
* Save time and avoid mistakes

---

## 3. Features

* Enter total bill amount
* Enter number of members
* Equal split calculation
* Tip percentage option
* GST / tax option
* Per person payable amount
* Easy interface

---

## 4. Example Scenario

9 friends visited a hotel.

| Item       | Amount |
| ---------- | ------ |
| Food Bill  | ₹9000  |
| Members    | 9      |
| Per Person | ₹1000  |

---

## 5. Formula Used

\text{Each Person Pay} = \frac{\text{Total Bill}}{\text{Number of Members}}

For ₹9000 and 9 members:

\frac{9000}{9}=1000

---

## 6. Working Process

```text
Friends Go Hotel
      ↓
Total Bill = ₹9000
      ↓
Members = 9
      ↓
System Calculates
      ↓
Each Person = ₹1000
```

---

## 7. Python Code

```python
bill = 9000
members = 9

each = bill / members

print("Total Bill =", bill)
print("Members =", members)
print("Each Person Pays =", each)
```

## Output

```text
Total Bill = 9000
Members = 9
Each Person Pays = 1000.0
```

---

## 8. User Input Code

```python
bill = float(input("Enter Total Bill: "))
members = int(input("Enter Number of Members: "))

each = bill / members

print("Each Person Should Pay =", each)
```

---

## 9. Tip Included Example

Bill = ₹9000
Tip = ₹900
Total = ₹9900

\frac{9900}{9}=1100

Each person pays ₹1100

---

## 10. Advanced Code with Tip

```python
bill = 9000
tip = 900
members = 9

total = bill + tip
each = total / members

print("Final Bill =", total)
print("Each Person Pays =", each)
```

---

## 11. Diagram

```text
                BILL SPLITTER

        Total Bill = ₹9000
              / / / / / / / / /
            9 Friends Share
     ₹1000 ₹1000 ₹1000 ₹1000 ₹1000
     ₹1000 ₹1000 ₹1000 ₹1000
```

---

## 12. Real Life Examples

### Example 1

Bill = ₹5000
Members = 5
Each = ₹1000

### Example 2

Bill = ₹2400
Members = 6
Each = ₹400

### Example 3

Bill = ₹12500
Members = 10
Each = ₹1250

---

## 13. Technologies Used

* Python
* Tkinter
* Flask
* HTML
* CSS

---

## 14. Future Enhancements

* Unequal split based on items eaten
* UPI payment integration
* Save bill history
* Mobile app version
* QR code payment

---

## 15. Resume Points

* Developed Smart Bill Splitter application using Python
* Automated equal bill sharing among group members
* Added tip and tax calculations
* Improved payment accuracy for group dining

---

## 16. Conclusion

Smart Bill Splitter is a simple and practical project that helps friends divide hotel or restaurant bills easily. It saves time, avoids confusion, and ensures everyone pays the correct amount.
