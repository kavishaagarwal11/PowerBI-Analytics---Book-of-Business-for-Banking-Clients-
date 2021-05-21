# PowerBI Analytics -Book of Business for Banking Clients

This project includes the Power BI dashboard created by Kavisha for analysis of ‘Book of Business’ of Banking clients. The data used in this project is a sample data generated using random functions in excel to replicate the type of data that I was retrieving from database using SQL queries in my previous job.  

The data model has dimensions tables and fact tables.
Dimension Tables:
1) Customer Info – This includes all know-your-customer information about clients like address, gender, contact number, employer, income etc. 
2) Account Details – This includes information about different type of accounts available in the bank for clients. 
Fact tables:
1) Mortgage –This includes mortgage details of all clients who have a mortgage
2) Investments – This includes different type of investment accounts each client has
3) Credit Card – This includes credit card details of each client who has a credit card
4) Accounts D2D – This includes the chequing and savings account details and balances each account has per client
5) Last Contact – This table gives the details of when each client was contacted last time
There were few other calculated tables created to get the desired output for analysis.

Main Dashboard:
Opportunity Radar – Banking Clients is the main dashboard and each visual has the drill through page which gives more details for each category.

Learnings:

-Importing data from files or from online

-Using Query Editor

-Joining tables and creating tables

-Creating custom columns and measures

-Designing charts and visualizations

-Formulating via DAX logic

•	Calculate, SUM, SUMX, FILTER, DATEIFF

•	SWITCH, NATURAL INNERJOIN

DAX usage:

1)	SWITCH function

Grouped customers by amount of balance in their account –
Balance Group =
             SWITCH(
                            TRUE(),
                            'Total Balance per Client'[Total Balance]<=25000,"less than 25K",
                            'Total Balance per Client'[Total Balance]<=50000,"25K to 50K",
                             'Total Balance per Client'[Total Balance]<=100000,"50K to 100K",
                             "Over 100K"
                             )


Grouped customers by age –
Age Group =
              SWITCH(
                            TRUE(),
                            'Customer Info'[Current age]<=17,"Minor",
                             'Customer Info'[Current age]<=30,"18-30",
                             'Customer Info'[Current age]<=40,"31-40",
                             'Customer Info'[Current age]<=50,"41-50",
                             'Customer Info'[Current age]<=60,"51-60",
                             "Over 60"
                              )

2)	DATEDIFF function


Calculated current age of customers – 
Current age =
              ROUNDDOWN(
                                        DATEDIFF(
                                              'Customer Info'[DOB],
                                               TODAY(),
                                               DAY
                                                          )
                                            /365.25,
                                        0)


Calculated number of days to last contact made –

No. of days to last contact =
                                        DATEDIFF
                                                     ('Last Contact'[Last contact date],
                                                          TODAY(),
                                                          DAY
                                                      )

3)	Percentage calculation

Calculated percentage of monthly payments on credit card by customers –

Monthly Payment % =
                             DIVIDE(
                                          'Credit Card'[Last Payment Amt],'Credit Card'[Balance Outstanding]
                                         )

4)	LOOKUPVALUE function

Used Lookupvalue function to retrieve the name of account from ‘Account details’ lookup table –

Account Type =
                    LOOKUPVALUE('Account details'[Account Type],
                    'Account details'[Account Code],
                    'Credit Card'[CC ID]
                                              )

5)	CALCULATE and FILTER function

Calculated number of clients for account type as RRSP and investment balance less than $25000 –

RRSP clients as per Bal & Income =
                                                 CALCULATE(
                                                                 COUNT(Investments[Client ID]),
                                                                                FILTER('Customer Info',
                                                                                'Customer Info'[Income]>100000),
                                                                                       FILTER(Investments,
                                                                                                     Investments[Account Type]="RRSP"
                                                                                                     && Investments[Balance]<25000
                                                                                                     )
                                                                            )





