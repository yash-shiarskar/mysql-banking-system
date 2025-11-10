# mysql-banking-system
The Banking Management System is a simple SQL-based project developed to simulate core banking operations. It allows managing customers, accounts, and transactions while demonstrating practical use of relational databases, SQL queries, stored procedures, and constraints.

# Key Objectives
 ##Customer Management: Efficiently store and manage customer information, ensuring data integrity and security.

##Account Management: Enable creation, modification, and closure of bank accounts with proper validations.

##Transaction Handling: Facilitate deposits, withdrawals, and fund transfers with accurate recording and balance checks.

#Advantages

Data Management: Centralized storage of customer and account information.

Accuracy: Automated calculations reduce human error in transactions.

Efficiency: Quick processing of deposits, withdrawals, and transfers.

Transparency: Maintains detailed transaction history for auditing.

Learning Tool: Provides hands-on practice with SQL, stored procedures, and relational database design.

#Disadvantages

Limited Interface: Works mainly via MySQL CLI or Workbench; no GUI for end-users.

Scalability: Not designed for large-scale banking operations.

Security: Lacks advanced authentication and encryption features.

Manual Maintenance: Requires manual script execution for some operations.

#Working Flow

Customer Management: Add or update customer details.

Account Creation: Link new accounts to customers with initial balance.

Transactions: Perform deposits, withdrawals, and transfers.

Transaction Logging: Record each transaction in the transactions table.

Balance Check: Validate account balance before withdrawal or transfer.

Reporting: Generate statements or view transaction history.
-- Step 1: Create and use database
CREATE DATABASE bankingSystem;
USE bankingSystem;

-- Step 2: Create Account Table
CREATE TABLE account (
    account_number INT AUTO_INCREMENT PRIMARY KEY,
    customer_name VARCHAR(30) NOT NULL,
    gender ENUM('male', 'female', 'other') NOT NULL,
    account_type ENUM('saving', 'current') NOT NULL DEFAULT 'saving',
    current_balance INT NOT NULL
);

-- Step 3: Insert Sample Data (Maharashtrian Names)
INSERT INTO account (customer_name, gender, account_type, current_balance)
VALUES
('Omkar Patil', 'male', 'saving', 27000),
('Snehal Deshmukh', 'female', 'current', 48000),
('Ruturaj Shinde', 'male', 'saving', 19000),
('Pooja Jadhav', 'female', 'saving', 26000),
('Amol Pawar', 'male', 'current', 65000),
('Kavita Gaikwad', 'female', 'saving', 15000),
('Sagar More', 'male', 'current', 73000),
('Aishwarya Chavan', 'female', 'current', 12000),
('Yash Kulkarni', 'male', 'saving', 31000),
('Pranali Bhosale', 'female', 'current', 59000),
-- duplicate customer to show both account types
('Omkar Patil', 'male', 'current', 45000);

-- Step 4: Create Transaction Table
CREATE TABLE transaction (
    trans_id INT AUTO_INCREMENT PRIMARY KEY,
    account_number INT,
    trans_type ENUM('deposit','withdrawal','transfer') NOT NULL,
    amount INT NOT NULL,
    trans_date DATE NOT NULL,
    FOREIGN KEY (account_number) REFERENCES account(account_number)
);

-- Step 5: Insert Sample Transactions
INSERT INTO transaction (account_number, trans_type, amount, trans_date)
VALUES
(1, 'deposit', 12000, '2025-11-01'),
(1, 'withdrawal', 6000, '2025-11-04'),
(2, 'deposit', 8000, '2025-11-03'),
(3, 'deposit', 9000, '2025-10-15'),
(4, 'transfer', 4000, '2025-11-07'),
(5, 'deposit', 12000, '2025-11-08'),
(6, 'withdrawal', 5500, '2025-11-09'),
(7, 'deposit', 15000, '2025-11-02'),
(8, 'withdrawal', 3000, '2025-09-21'),
(9, 'deposit', 11000, '2025-11-06'),
(10, 'withdrawal', 9500, '2025-11-08'),
(11, 'deposit', 20000, '2025-11-10');

-- Step 6: Queries with Output

-- 1️⃣ Retrieve all customers with balance > 50,000
SELECT * FROM account WHERE current_balance > 50000;

-- 2️⃣ Display account number, customer name, and account type of all SAVING accounts
SELECT account_number, customer_name, account_type FROM account WHERE account_type = 'saving';

-- 3️⃣ List all transactions made in the current month
SELECT * FROM transaction WHERE MONTH(trans_date) = MONTH(CURDATE()) AND YEAR(trans_date) = YEAR(CURDATE());

-- 4️⃣ Show customers who have not made any transactions yet
SELECT a.account_number, a.customer_name, a.account_type FROM account a LEFT JOIN transaction t ON a.account_number = t.account_number WHERE t.account_number IS NULL;

-- 5️⃣ Display the top 3 customers with the highest account balance
SELECT account_number, customer_name, current_balance FROM account ORDER BY current_balance DESC LIMIT 3;

-- 6️⃣ Retrieve all transactions where the amount is greater than 10,000
SELECT * FROM transaction WHERE amount > 10000;

-- 7️⃣ Show the total balance of all accounts combined
SELECT SUM(current_balance) AS total_balance FROM account;

-- 8️⃣ List customers along with their total deposited amount
SELECT a.account_number, a.customer_name, SUM(t.amount) AS total_deposited FROM account a JOIN transaction t ON a.account_number = t.account_number WHERE t.trans_type = 'deposit' GROUP BY a.account_number, a.customer_name;

-- 9️⃣ Find customers who made a withdrawal of more than 5,000
SELECT a.account_number, a.customer_name, t.amount FROM account a JOIN transaction t ON a.account_number = t.account_number WHERE t.trans_type = 'withdrawal' AND t.amount > 5000;

-- 🔟 Display the most recent transaction date for each account
SELECT account_number, MAX(trans_date) AS most_recent_transaction FROM transaction GROUP BY account_number;

-- 1️⃣1️⃣ Retrieve the number of transactions each customer has made
SELECT a.account_number, a.customer_name, COUNT(t.trans_id) AS total_transactions FROM account a LEFT JOIN transaction t ON a.account_number = t.account_number GROUP BY a.account_number, a.customer_name;

-- 1️⃣2️⃣ List customers who have both SAVINGS and CURRENT accounts
SELECT customer_name FROM account GROUP BY customer_name HAVING COUNT(DISTINCT account_type) = 2;
mysql> --13 Display the average account balance per account type
SELECT * FROM account WHERE customer_name LIKE 'P%';
mysql> -- 14 Retrieve customers sorted by their account balance in descending order
mysql> SELECT account_number, customer_name, current_balance FROM account ORDER BY current_balance DESC;
mysql> -- 15 Display the average account balance per account type
mysql> SELECT account_type, AVG(current_balance) AS average_balance FROM account GROUP BY account_type;

#Conclusion

The Banking Management System demonstrates fundamental banking operations using MySQL, providing a practical approach to managing customers, accounts, and transactions. It emphasizes database design, relational integrity, and the use of stored procedures for automation. While it is suitable for learning and small-scale simulations, it highlights the importance of accuracy, transparency, and structured data management in real-world banking. This project serves as a solid foundation for building more advanced banking applications with enhanced security, scalability, and user interfaces.
