# MYSQL_BANK
This project focuses on analyzing a Bank Transactions dataset using MySQL. 
It demonstrates practical SQL skills including data cleaning, aggregation, filtering, and analytical queries used in real-world banking data analysis. 
The dataset contains information about customer transactions such as deposits, withdrawals, and balances over time.

# Questions

1.Show all transactions in the table.

2.List transactions where WITHDRAWAL_AMT > 0.

3.List transactions where DEPOSIT_AMT > 0.

4.Find transactions done on a specific date (e.g., 2023-08-01).

5.Display all transactions for a given Account_No.

6.Count total number of transactions per Account_No.

7.Find transactions where BALANCE_AMT < 10000.

8.Show transactions where WITHDRAWAL_AMT > DEPOSIT_AMT.

9.List distinct TRANSACTION_DETAILS values.

10.Find the latest transaction date for each account.

11.Calculate the total deposits for each account.

12.Calculate the total withdrawals for each account.

13.Find the account with the highest deposit in a single transaction.

14.Find the account with the highest total withdrawal.

15.List accounts with more than 5 transactions.

16.Find transactions where WITHDRAWAL_AMT = 0 AND DEPOSIT_AMT = 0.

17.Compute the average withdrawal amount per account.

18.Compute the average deposit amount per account.

19.Show transactions sorted by BALANCE_AMT descending.

20.Find the first and last transaction dates per account.

21.Find accounts where total deposits exceed total withdrawals.

22.Identify accounts where BALANCE_AMT ever went below ₹5000.

23.Compute the net change in balance for each account: SUM(DEPOSIT_AMT - WITHDRAWAL_AMT).

24.Show accounts with consecutive withdrawals above 10,000.

25.Rank accounts by total balance using window functions.

26.Find the month with highest total deposits for each account.

27.Find transactions where WITHDRAWAL_AMT > 50% of BALANCE_AMT.

28.Compute average daily balance per account.

29.Show the most frequent transaction type per account.

30.Generate a report showing total deposits, total withdrawals, and ending balance per account.



# MYSQL_CODE

CREATE DATABASE bank;
use bank;


SET SQL_SAFE_UPDATES = 0;

UPDATE bank
SET BALANCE_AMT = ABS(BALANCE_AMT);

SET SQL_SAFE_UPDATES = 1; 



SELECT * FROM bank;


SELECT TRANSACTION_DETAILS , WITHDRAWAL_AMT 
FROM bank
WHERE WITHDRAWAL_AMT >0;



SELECT TRANSACTION_DETAILS , DEPOSIT_AMT 
FROM bank
WHERE DEPOSIT_AMT >0;



SELECT * FROM bank
WHERE TRANS_DATE = '2019-03-05' ;



SELECT ACCOUNT_NO ,COUNT(*) AS TOTAL
FROM bank
group by ACCOUNT_NO;



SELECT TRANSACTION_DETAILS , BALANCE_AMT
FROM bank
WHERE BALANCE_AMT >100000;



SELECT TRANSACTION_DETAILS
FROM bank
WHERE WITHDRAWAL_AMT > DEPOSIT_AMT;



SELECT DISTINCT TRANSACTION_DETAILS
FROM bank;



SELECT ACCOUNT_NO , SUM(DEPOSIT_AMT) AS DEPOSITE_AMOUNT
FROM bank
GROUP BY ACCOUNT_NO;



SELECT ACCOUNT_NO , SUM(WITHDRAWAL_AMT) AS WITHDRAW_AMOUNT
FROM bank
GROUP BY ACCOUNT_NO;



SELECT * FROM bank
ORDER BY DEPOSIT_AMT DESC
LIMIT 1;



SELECT * FROM bank
ORDER BY WITHDRAWAL_AMT DESC
LIMIT 1;


SELECT * FROM bank
WHERE WITHDRAWAL_AMT = 0 AND DEPOSIT_AMT = 0;



SELECT ACCOUNT_NO , AVG(WITHDRAWAL_AMT) AS AVG_WTHDRAW_AMT 
FROM bank
GROUP BY ACCOUNT_NO
ORDER BY ACCOUNT_NO DESC;



SELECT ACCOUNT_NO , AVG(DEPOSIT_AMT) AS AVG_DEPOSITE_AMT 
FROM bank
GROUP BY ACCOUNT_NO
ORDER BY ACCOUNT_NO DESC;



SELECT * FROM bank
ORDER BY BALANCE_AMT DESC;



SELECT ACCOUNT_NO,
       MIN(TRANS_DATE) AS FIRST_TRANSACTION,
       MAX(TRANS_DATE) AS LAST_TRANSACTION
FROM bank
GROUP BY ACCOUNT_NO;



SELECT ACCOUNT_NO,
       SUM(DEPOSIT_AMT) AS TOTAL_DEPOSIT,
       SUM(WITHDRAWAL_AMT) AS TOTAL_WITHDRAWAL
FROM bank
GROUP BY ACCOUNT_NO
HAVING SUM(DEPOSIT_AMT) > SUM(WITHDRAWAL_AMT);




SELECT DISTINCT ACCOUNT_NO
FROM bank
WHERE BALANCE_AMT < 5000;




SELECT ACCOUNT_NO,
       SUM(DEPOSIT_AMT - WITHDRAWAL_AMT) AS NET_CHANGE
FROM bank
GROUP BY ACCOUNT_NO;




SELECT ACCOUNT_NO, TRANS_DATE, WITHDRAWAL_AMT
FROM bank
WHERE WITHDRAWAL_AMT > 10000
ORDER BY ACCOUNT_NO, TRANS_DATE;




SELECT ACCOUNT_NO,
       SUM(BALANCE_AMT) AS TOTAL_BALANCE,
       RANK() OVER (ORDER BY SUM(BALANCE_AMT) DESC) AS RANKING
FROM bank
GROUP BY ACCOUNT_NO;




SELECT ACCOUNT_NO,
       MONTH(TRANS_DATE) AS MONTH_NO,
       SUM(DEPOSIT_AMT) AS TOTAL_DEPOSIT
FROM bank
GROUP BY ACCOUNT_NO, MONTH(TRANS_DATE)
ORDER BY TOTAL_DEPOSIT DESC;




SELECT *
FROM bank
WHERE WITHDRAWAL_AMT > 0.5 * BALANCE_AMT;



SELECT ACCOUNT_NO,
       AVG(BALANCE_AMT) AS AVG_DAILY_BALANCE
FROM bank
GROUP BY ACCOUNT_NO;




SELECT ACCOUNT_NO,
       CASE 
           WHEN SUM(DEPOSIT_AMT > 0) > SUM(WITHDRAWAL_AMT > 0) THEN 'Deposit'
           ELSE 'Withdrawal'
       END AS MOST_FREQUENT_TYPE
FROM bank
GROUP BY ACCOUNT_NO;



SELECT ACCOUNT_NO,
       SUM(DEPOSIT_AMT) AS TOTAL_DEPOSIT,
       SUM(WITHDRAWAL_AMT) AS TOTAL_WITHDRAWAL,
       MAX(BALANCE_AMT) AS CURRENT_BALANCE
FROM bank
GROUP BY ACCOUNT_NO;














