# Database Improvement for Installment Payments

## Overview
To enhance the tracking of unpaid balances in an installment payment system, the following improvements are proposed:

### 1. Modify the `payments` Table
- **Add `totalAmountDue` Column**: Stores the total order amount.
- **Add `balanceRemaining` Column**: Tracks the unpaid amount.

### 2. Update the `orders` Table
- **Create `payment_status` Column**: Indicates the payment status with possible values:
  - "Paid in Full"
  - "Partially Paid"
  - "Unpaid"

### 3. Generate an Unpaid Balance Report
The following SQL query calculates the balance remaining for each order, allowing for better monitoring of unpaid balances.

```sql
SELECT o.orderNumber, o.customerNumber, 
       (SUM(od.quantityOrdered * od.priceEach) - COALESCE(SUM(p.amount), 0)) AS balanceRemaining
FROM orders o
JOIN orderdetails od ON o.orderNumber = od.orderNumber
LEFT JOIN payments p ON o.customerNumber = p.customerNumber
GROUP BY o.orderNumber, o.customerNumber
HAVING balanceRemaining > 0;
