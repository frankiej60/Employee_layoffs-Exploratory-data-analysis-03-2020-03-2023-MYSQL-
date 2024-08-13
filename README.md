# Employee_layoffs-Exploratory-data-analysis-MYSQL-

### Project brief

---

The data analysis project aims to take the raw data a step further to identify facts from the figures. 


### Data source 
---
Employee layoffs: The primary data used for this analysis is the layoffs.data_csv file, containing detailed information about layoffs made by companies. 

### Tools
---
- MYSQL data analysis software
    - [Download here](https://MYSQL.com)

### Queries worked with
  ---
    SQL
  

select * 
from layoffs_stages2;

select max(total_laid_off), max(percentage_laid_off)
from layoffs_stages2;

select * 
from layoffs_stages2
where percentage_laid_off = 1
order by total_laid_off desc;

select * 
from layoffs_stages2
where percentage_laid_off = 1
order by funds_raised_millions desc;

select company, sum(total_laid_off)
from layoffs_stages2
group by company
order by 2 desc;

select min('date'), max('date')
from layoffs_stages;

select industry, sum(total_laid_off)
from layoffs_stages2
group by industry
order by 2 desc;

select country, sum(total_laid_off)
from layoffs_stages2
group by country
order by 2 desc;

select 'date',sum(total_laid_off)
from layoffs_stages2
group by date
order by 1 desc;

select year('date'), sum(total_laid_off)
from layoffs_stages2
group by year('date')
order by 1 desc;

select stage,sum(total_laid_off)
from layoffs_stages2
group by stage
order by 2 desc;

select substring('date', 1,7) as 'Month', sum(total_laid_off)
from layoffs_stages2
where substring('date', 1,7) is not null
group by 'Month'
order by 1 asc;

with rolling_total as
(
select substring('date', 1,7) as 'Month', (total_laid_off) as total_off
from layoffs_stages2
where substring('date', 1,7) is not null
group by 'Month'
order by 1 asc
)
select 'Month', total_laid_off,
sum(total_off) over(order by 'Month') as rolling_total
from rolling_total;

select company, year('date'), sum(total_laid_off)
from layoffs_stages2
group by company, year('date')
order by 3 desc;

WITH company_year (company, years, total_laid_off) as 
(
  select company, year('date'), sum(total_laid_off)
  from layoffs_stages2
  group by company, year('date')
), 
company_year_rank as 
(
  select *, dense_rank() over(partition by years order by total_laid_off desc) as ranking
  from company_year
  where years is not null
)
SELECT *
from company_year_rank
where ranking <= 5;
