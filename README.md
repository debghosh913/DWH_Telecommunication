# DWH_Telecommunication

Telecommunication Billing 
Management System
Project Report
Data Warehousing
SS G515
Debanjan Ghosh - 2021H1120266P 
Thota Phaneendra Babu - 2021H1120258P
Subhadeep Deb Roy – Assistant System 
Engineer-Trainee (TCS)
Under the supervision 
Dr. L. Rajya Lakshmi 
Assistant Professor
Department of Computer Science and Information Systems
Birla Institute of Technology and Science 
Pilani, Pilani Campus, Rajasthan (India)
Motivation:
Consider a telecom company which is located in different countries and has a 
considerable customer base which offers multiple service plans like data, voice, 
messages. There are other telecom companies which offer similar service to the 
users, in order to provide better customer experience, to stand out amongst its 
competitors and to gain good profits the company must need to analyze the 
behavior of the data related to the services. To make strategic decisions from the 
data a data warehouse is necessary.
Introduction
1. In this report we will do a case study on a Telecommunication 
Management System .
2. We will gather requirements for our System.
3. We will identify different business Dimensions.
4. We will spot all the Business Metrics that are relevant to the
telecommunication company.
5. After doing all these necessary steps ,We will build our star schema.
Steps for making our data warehouse:
1)Requirements Gathering:
To gather and store requirements,we will make an information package diagram.
2)Identify different business Dimensions:
We have identified six different dimensions.
Customer Dimension: All customer information such as customers name
,age,mobile number is stored in this dimension.
Payment Dimension:Customers payment related information is stored 
in that dimension. Which payment mode they use (UPI,Net Banking 
,credit card ,debit card) is stored in the table.
Time: We have included Time to capture the transactional details from a day 
granular level to year.
Promotion: On the Promotion Dimension table there will be promo code .If a 
customer is eligible he will get a certain discount.
Recharge_plan: Different data plans, plan validity, voice call charge per 
minute in India , voice call charge per minute abroad, per unit message 
charge in India, per unit message charge abroad, Message limit per day 
according to the plan, Data Limit per day is stored.
Location Dimension: Using location dimension we can keep track of area 
wise transactional details. we can roll up in our dimension from area to 
country level.
3)Identifying Different Business Metrics:
➢ Data Usage: By analyzing Data usage, company get to know which 
data plan is more popular. Accordingly the company can introduce new 
data packages to its customers.
➢ Voice_call_Duration: By analyzing durations of both international and 
national calls, company can measure the traffic generated across the
network,so that they can improve customer experience.
➢ Total_voice_call_cost: By analyzing the total voice call cost, company 
will have a rough estimate of their profit through voice call.
➢ Total_message_count: By analyzing the total_message count company
comes to know whether users are more inclined towards text message or 
calls or data.
➢ Gateway charges:By analyzing the Gateway charges ,company comes to 
know which Gateways are used frequently(payment gateways).Accordingly 
company can give certain promo codes to attract customers.
Star Schema:
Implementation of Queries:
--Using CUBE operator
SELECT r.plan_name, SUM:/b. st dard_gateway_charges : AS total_charges
FROM
bill_fact b Sci recharge_plan_dim r
on b.plan_key=r.plan_Ley 
GROUP BY CUBE:/r.plan_name: 
ORDER BY r.p1an_name
@ Results Messages
1
: !°r.r°re
. NULL
. t°t°!—h°’°e°
, 49
2
” : ” ” ” ” '
Jic•_US_175O 13.5
3 Jio_US_75O 7.5
4 Jio399 14.5
5 Jic•699 13.5
/*Total gate ay charges per plan per month*/
SELECT r. plan_name, t.mo t , SUll:/b. standard_gateway_charges: AS total_charges
FROhl bill_fact b
Sci recharge_plan_dim r on b.plan_key=r.plan_key 
jci time_dim t on b.time_key=t.time_key
GROUP BY r.plan_name,t.mo t 
ORDER BY r.plan_name, t.mo t
@ Results } |d Messages
, '°r.r°r.e . . °nth txt °'—eh ^r*e°
1 . Jio US 1750 . April 9
2 ” Ji”o US_ 1ISO November 4.5
3 Jio_US_75O April 5
4 Jio_US_7g@ Nc•vember 2.5
5 Jio399 April 9.5
6 Jio399 Janu ary 2.5
7 Jio399 March 2.5
g Jio699 April 4.5
g Jio699 Janu ary 4.5
10 Jio699 March 4.5
SELECT COALESCE(r.plan_name,'All types ) as plan_name, COALESCE(t.month,'sum'l as month, SUf(b.standard_gateway_charges) AS total_char 
FROM bill_fact b
a necha nge_p1an_d > n on b . p1an_key= n . pt an_key
ñ o i ’ tine d1m I on b , t1r»e key=t , t1me key
GROUP BY rollup(r.plan_name,t.monthl
|@ Results [ |d Messages
pIan_n ame month totaI_ch arges
1 Jio—US—.
1
....
7
. .
5
...
0
......›
: April 9
2 Jio_US_1750 November 4.5
3 Jio_US_17hO sum 13.5
Jio_US_75O April 5
Jio_US_75O November 2.5
6 Jio_US_75O sum 7.5
Jio399 April 9.5
8 Jio399 January 2.5
9 Jio399 March 2.5
10 Jio399 sum 14.5
11 Jio699 April 4.5
12 Jio699 January 4.5
13 Jio699 March 4.5
14 Jio699 sum 13.5
15 All types sum 49
/*Slicing*/
select t.mo t , SUl1:/b.data_usage: as monthly_usage 
FROPI bill_fact
Sci recharge_plan_dim r on r.plan_key=b.plan_key 
Sci time_dim t on t.time_key=b.time_key
group by r.plan_country,t.mo t having r.plan_country= 'India'
|@ Results }Z| Messages
month monthIy_usag e
1 April . 11
2 January 5.5
3 March 5.5
/*Dicing*/
select t.month, SUll.b.data_usage: as monthly_usage
FROhl bill_fact b
Sci recharge_plan_dim r r.plan_key=b.plan_key 
Sci time_dim t on t.time_key=b.time_key
where t.month i :/ 'April',' January' /:
group by r.plan_country,t.month
|@ Results @ Messages
months monthIy_usa ge 1
A...
p
....
r
..
i
.
l ..........11
2 January 5.5
r.plan_country='India'
SELECT r.plan_name, t.month, SUll:/b. standard_gateway_charges: AS total_charges
FROM bill_fact b
Sci recharge_plan_dim r on b.plan_key=r.plan_key
Sci time_dim t on b.time_key=t.time_key 
GROUP BY CUBE:/ r.plan_name,t.month :
°'°r.r°e.e. . ° nth t°t°'—°h^
r*e°
1 Jio_US_175O April 9
2 Jio_US_7§D April 5
3 Jio399 April 9.5
4 Jio699 April 4.5
5 NULL April 28
6 Jio399 January 2.5
7 Jio699 January 4.5
B NULL January 7
9 Jio399 March 2.5
10 Jio699 March 4.5
11 NULL March 7
12 Jio_US_175D Novem ber 4.5
13 Jio_US_75O Novem ber 2.5
14 NULL Novem ber 7
15 NULL NULL 49
16 Jio_US_1750 NULL 13.5
17 Jio_US_75O NULL 7.5
18 Jio399 NULL 14.5
19 Jio699 NULL 13.5
