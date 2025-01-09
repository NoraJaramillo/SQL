# **FORTUNE 500 ANALYSIS** 
_This project utilizes the following Fortune 500 data:_

```
   CREATE TABLE fortune_companies (
    company_id INTEGER PRIMARY KEY,
    company_name TEXT,
    industry TEXT,
    revenue REAL,
    employees INTEGER,
    healthcare_benefits BIT,
    paid_time_off_days INTEGER,
    maternity_leave_weeks INTEGER,
    avg_employee_tenure REAL
);

INSERT INTO fortune_companies (company_name, industry, revenue, employees, healthcare_benefits, paid_time_off_days, maternity_leave_weeks, avg_employee_tenure)
VALUES
    ('Apple Inc.', 'Technology', 365.7, 147000, 1, 20, 12, 4.5),
    ('Walmart Inc.', 'Retail', 523.96, 2200000, 1, 15, 8, 6.2),
    ('Exxon Mobil Corporation', 'Energy', 265.01, 72000, 0, 18, 6, 7.8),
    ('Amazon.com Inc.', 'Technology', 386.06, 1370000, 1, 22, 14, 5.1),
    ('JPMorgan Chase & Co.', 'Financials', 160.1, 255998, 1, 21, 12, 6.9),
    ('Verizon Communications Inc.', 'Telecommunications', 131.88, 132600, 0, 15, 6, 5.5),
    ('Company A', 'Retail', 235.4, 2000, 1, 18, 10, 5.8),
    ('Company B', 'Healthcare', 400.7, 2300, 1, 22, 13, 5.7),
    ('Company C', 'Manufacturing', 300.2, 2000, 1, 18, 10, 5.8),
    ('Company D', 'Healthcare', 150.5, 3500, 1, 20, 12, 6.5),
    ('Company E', 'Finance', 280.7, 1800, 0, 14, 8, 4.2),
    ('Company F', 'Technology', 420.1, 2500, 1, 22, 14, 7.1),
    ('Company G', 'Retail', 190.8, 1500, 1, 16, 9, 5.3),
    ('Company H', 'Energy', 280.5, 2200, 0, 15, 8, 6.8),
    ('Company I', 'Telecommunications', 110.3, 1800, 1, 19, 11, 4.9),
    ('Company J', 'Manufacturing', 390.6, 2700, 1, 21, 13, 6.2),
    ('Company K', 'Healthcare', 180.2, 3200, 1, 17, 9, 7.4),
    ('Company L', 'Finance', 230.4, 1900, 0, 13, 7, 5.6),
    ('Company M', 'Technology', 340.9, 2800, 1, 23, 15, 6.9),
    ('Company N', 'Retail', 200.6, 1600, 1, 15, 8, 4.7),
    ('Company O', 'Energy', 260.2, 2400, 0, 14, 7, 6.1),
    ('Company P', 'Telecommunications', 130.5, 2100, 1, 20, 12, 5.3),
    ('Company Q', 'Manufacturing', 360.0, 2900, 1, 22, 14, 7.8),
    ('Company R', 'Technology', 400.7, 2300, 1, 22, 13, 5.7),
    ('Company S', 'Retail', 210.8, 1600, 0, 16, 9, 4.9),
    ('Company T', 'Energy', 290.5, 2200, 1, 15, 8, 7.2),
    ('Company U', 'Telecommunications', 140.3, 1900, 1, 20, 12, 6.1),
    ('Company V', 'Manufacturing', 350.6, 2800, 1, 22, 14, 5.4),
    ('Company W', 'Healthcare', 160.2, 3300, 0, 18, 10, 4.8),
    ('Company X', 'Finance', 240.4, 2000, 1, 13, 7, 7.1),
    ('Company Y', 'Technology', 320.9, 2700, 1, 23, 15, 5.6),
    ('Company Z', 'Retail', 180.6, 1400, 0, 14, 8, 6.3),
    ('Company AA', 'Energy', 240.2, 2600, 1, 17, 9, 6.5),
    ('Company BB', 'Telecommunications', 120.5, 2100, 0, 19, 11, 4.5),
    ('Company CC', 'Manufacturing', 380.0, 3000, 1, 21, 13, 7.3),
    ('Company DD', 'Healthcare', 170.2, 3200, 1, 17, 9, 5.8),
    ('Company EE', 'Finance', 250.4, 1900, 0, 12, 6, 6.4),
    ('Company FF', 'Technology', 300.9, 2500, 1, 24, 16, 6.9),
    ('Company GG', 'Retail', 190.6, 1700, 0, 13, 7, 5.2),
    ('Company HH', 'Energy', 280.2, 2300, 1, 16, 9, 6.8),
    ('Company II', 'Telecommunications', 110.5, 2000, 1, 21, 12, 4.9),
    ('Company JJ', 'Manufacturing', 370.0, 3100, 1, 20, 12, 7.6),
    ('Company KK', 'Healthcare', 150.2, 3400, 0, 16, 8, 5.3);
```
---
### 1.Identifying Industry Leaders and sector comparison:
_This query analyzes the top-revenue companies within each industry, highlighting the leaders and identifying the overall highest-revenue company. It also compares key sectors like retail and technology to uncover trends and differences._
```
SELECT company_name, industry, revenue, employees
FROM(SELECT company_name, industry, revenue, employees, rank () over(partition by industry order by revenue DESC)as rank
	FROM fortune_companies)a
WHERE rank = 1
order by revenue desc;
```
_The results show that Walmart leads both the retail sector and all industries overall in revenue. This underscores retail’s dominance in revenue generation through mass-market operations, while the technology sector stands out for its efficiency and innovation, driving profitability with smaller workforces._

### 2. Benefits and Company Size Relationship
_This query explores how companies offering healthcare benefits vary by size, grouping organizations into four categories based on their number of employees. It analyzes patterns in the average number of paid time off (PTO) days and maternity leave weeks, helping to identify trends and disparities across company sizes.
```
SELECT Qty_of_employees, round(AVG(paid_time_off_days),2) Avg_PTO_days, round(AVG(maternity_leave_weeks),2) as Avg_Maternity_weeks
FROM (SELECT company_id, healthcare_benefits, paid_time_off_days, maternity_leave_weeks,
	CASE WHEN employees > 500000 THEN '>500K'
	WHEN employees BETWEEN 100000 AND 500000 THEN '100K-500K'
	WHEN employees BETWEEN 10000 AND 100000 THEN '10000-100000'
	ELSE '0-10000' END AS Qty_of_employees
	from fortune_companies)A
GROUP BY Qty_of_employees
```
_The analysis revealed that the average number of PTO days is relatively consistent across all company sizes, ranging between 18 and 18.67 days. However, significant differences were observed in maternity leave policies:
Larger companies (>500,000 employees) provide the best conditions, offering an average of 11 weeks of maternity leave.
Interestingly, smaller companies (0-10,000 employees) also offer competitive conditions, with an average of 10.5 weeks of maternity leave.
Mid-sized companies (10,000-100,000 employees) perform the worst in both metrics, offering the lowest averages for PTO days and maternity leave weeks._

### 3. Healthcare Benefits by Industry
_This query analyzed the percentage of companies offering healthcare benefits across various industries to identify trends and determine which sectors prioritize employee well-being through health-related benefits._
```
SELECT DISTINCT industry, round((SUM(healthcare_benefits) OVER(PARTITION BY industry))*1.0 /(COUNT(healthcare_benefits) OVER(PARTITION BY industry))*1.0*100,2) as Healthcare_benefits_perc
FROM fortune_companies
ORDER BY 2 DESC
```
_The analysis revealed notable disparities in the availability of healthcare benefits across industries. The finance sector offers healthcare benefits to only 25% of companies, the lowest among all sectors, indicating a potential area for improvement in employee welfare._
_This finding suggests that while some industries prioritize healthcare benefits to enhance employee satisfaction and retention, sectors like finance may lag despite being a high-revenue industry._

### 4. Above Average Paid Time Off Policies by Industry
_This query identifies companies that offer more paid time off (PTO) days than the average for their industry, focusing on organizations that prioritize employee well-being and competitive benefits._
```
SELECT fc.company_name, fc.industry, fc.paid_time_off_days
FROM fortune_companies fc
JOIN (SELECT DISTINCT industry, AVG(paid_time_off_days) OVER(PARTITION BY industry) AS avg_PTO
FROM fortune_companies) avg
ON fc.industry = avg.industry
WHERE fc.paid_time_off_days >= avg.avg_PTO
order by 2
```
_The analysis revealed that the manufacturing and technology sectors stand out, offering significantly higher average PTO days compared to other industries. Such policies can boost job satisfaction, improve work-life balance, and position these sectors as leaders in talent attraction and retention._


