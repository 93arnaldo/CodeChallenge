This query is not correct. 
Because I need to **only get the disbursement transactions** and I tried to do it using the distinct clause to the user ID, and then an ORDER BY to the created column to filter the oldest transaction which will return only the disbursement transactions. However, this approach is not working. 

Other solution that I have tried is the following:

-- Joining Tables
select
    u.id as userid,
	t.loan_id as loanid,
	l.amount as loanamount,
	-- using distict user in order to show only the first disbursement transaction by user
	t.fee as transaction_fee,
    min(t.created) as disbursement
FROM
	transactions t
INNER JOIN loans l ON
	t.loan_id = l.id
INNER JOIN users u ON
	t.user_id = u.id
WHERE
	l.loan_status = 'active'
	OR l.loan_status = 'repaid'
	-- ordering by earliest date to show only disbursement type of transaction
	group by disbursement
  
  But it also doesn't work. Another approach that I would follow next is to **use a subquery to get the oldest transaction** first, and then join it with the rest of the tables. 
  
However, I was only able to test these queries for a very short period of time because during the whole weekend, I would get the message of: **SQL Error [53300]: FATAL: too many connections for role "mqhyrlua"**, which wouldn't allow me to test and correct my queries. Check screenshot below:

![](images/failure.png)
