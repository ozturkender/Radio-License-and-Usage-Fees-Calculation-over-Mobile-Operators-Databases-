# Radio-License-and-Usage-Fees-Calculation-over-Mobile-Operators-Databases-
Data Analysis using SQL


## Objective:
- The aim of this project is accurately calculate the amount of radio fees that all mobile operators should pay to government. 
	
- This fee is calculated based on the initiation and termination of subscription of subscribers. There are two types of radio fees, one-off license fee and monthly usage fee. The relevant law act is given below

_ECL - 5809.45-5: Radio license and usage fees collected from the postpaid subscribers (on the contrary of prepaid subscribers) of operators providing mobile electronic communications services shall be received on the basis of dividing fee amount into equivalent installments as of the month when subscription took place until the end of year._

All rules of calculation are to be derived from this one sentence, which are:
- Postpaid subscribers one-off license fees will be paid dividing fee amount into equivalent installments as of the month when subscription took place until the end of the year (i.e. Let fee be $12, if subscription initialized at 5th month, payments will be from 5th to 12th months in 8 installments of $12⁄8=$1.5)
	
- Postpaid subscribers monthly usage fees will be paid monthly (i.e. $12⁄12=$1 for each month) and will continue so until the termination of subscription.
- Prepaid subscribers one-off license fees will be paid as a whole at the end of subscription month (i.e. $12 at the end of 5th month.)
- Prepaid subscribers monthly usage fees will be paid on pro-rata basis depending on the month of subscription (i.e. for a prepaid subscriber entered the system at 5th month 5⁄12×$12=$5 will be paid) for that year.
- At the beginning of the years, monthly usage fees of postpaid subscribers will be paid on monthly basis as usual, however because of the “on the contrary” expression, monthly usage fees for prepaid subscribers turn into annual fees and paid at the end of the 1st month in advance.

If a subscriber changes its payment type from post to pre:

If this change occurs at the first year of subscription
- The rest of the one-off fee installments are charged as a whole at that month.
- The rest of monthly usage fee payments for that year turns into annual fee and charged at the end of that month.

If this change occurs later than the first year
- Since one-off fee installments have been completed before, only monthly usage fee turns into annual fee and charged at the end of that month.

If a subscriber changes its payment type from pre to post:
- Regardless of the year of subscription, since both one-off license fee and monthly usage fee have already been charged, these subscribers are not charged to the end of the year.

If a subscriber moves to a different operator from pre to post and pre to pre
- The new operator should identify that this subscriber was prepaid before and do not charge for the rest of the year. 

From post to pre
- Donor operator charges the whole remaining fees as of the month of the move

From post to post
- Donor operator charges for the remaining installments of the one-off license fee (if the subscription is in its first year), and only one month’s monthly fee, the month of the move. Receiver operator knows this and begins charging monthly fees at the following month.

If a subscriber changes its payment type from prepaid to postpaid and then moves to a different operator 

- Since the subscriber was prepaid at somewhere in that year, all fees were charged already. Therefore, receiver operator should know that even if the subscriber’s last payment type was postpaid, it should not be charged for the rest of the year. 

## Dataset:
Databases contain over __80 million subscribers'__ subscription and movement data spread over tens of different tables for each of three mobile operators involving last 10 years, adding up to around __1.5 billion rows of information__.

## Analyses
Each of three mobile operators has their own database structure, table relations in terms of primary and foreign keys. Each structure has been comprehended and interpreted to
- identify which type of entry corresponds to a new subscription and change of provider 
- define with which key should a subscriber be followed (i.e. mobile number cannot be used for this purpose b/c tens of thousands of subscribers change their number without terminating their contract)

Throughout this project a unique data format has been engineered in order to map fees to highly complex subscriber moves. 

_Sample Scenario 1: A subscriber (let be the one-off license fee and monthly fee x 12 equal to $12);_
- Entered the system last year as postpaid
- Terminated its contract at this year’s 8th month

will pay 

- One-off fee: None, b/c one-off fee debt is closed at the end of last year
- Monthly fee: $12/12 = $1 monthly payments each month for 8 months.

_Sample Scenario 2: A subscriber (let be the one-off license fee and monthly fee x 12 equal to $12)_
- Entered system at 7th month as postpaid
- Changed type to prepaid at 9th month,
- Moved to another provider at 11th month as postpaid

will pay

- One-off fee: $12/6 monthly installments up to 9th month for 3 months, and the remaining 3 installments (3 x  $12/6) at the 9th month, will not be charged by receiver operator after 11th month
- Monthly fee: $12/12 monthly installments up to 9th month for 3 months, and remaining 3 installments (3 x $12/12) at the 9th month

- After comprehensive and extremely detailed work, 3,280 different scenarios have been identified, 1,249 of these scenarios necessitate payments, and the rest are to ensure the integrity of the model.

- Over 25 thousand lines of SQL code had been written and inspected. 

## Conclusion

- At the end of this 3-years-long massive project, it is revealed that more than $50M of taxpayers money had been unpaid by mobile operators.

- Engineered data format became the main structure that prevented future discrepancy between government and mobile operators. 

