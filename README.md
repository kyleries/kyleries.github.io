<img alt="Kyle Ries, software and data engineer" src="https://user-images.githubusercontent.com/1095673/206064782-d8f875b2-eafe-45c3-9f20-974fcdc467d6.png">

***

## Advent of Code, 2022
12/6/2022

I am participating in the [Advent of Code](https://adventofcode.com/) this year, and I encourage you to do the same! I've chosen the Python scripting language this year. In the past two years, I've attempted the challenges using only SQL which made it....interesting. :smile: The challenges are suited more for a traditional programming language, but attempting to use SQL (via BigQuery) was a helpful mental exercise. It encourages thinking about the problem through a lens of the data, rather than the traditional instinct of developers who may be more inclined to think about "iterators" and "conditionals".

As an example, during the 2021 Advent of Code, one particular challenge resulted in a need to multiply down the rows. Historically, multiplying down several thousand rows has not been a particular strong suit of SQL. Certainly, there are ways to manipulate the data in such a way that multiplying the values is possible. However, in attempting this challenge with this tool, I discovered a way to solve it elegantly with a mathematical solution rather than relying on brute force and the CPU.

Inspired by the discussion of Logs and natural logs from the fantastic book [The Joy of *X*](https://www.overdrive.com/media/4872928/the-joy-of-x), it dawned on me that rather than try to force SQL to solve this multiplication problem, I ought to leverage the power of logs (see what I did there) and turn this into a simple addition problem. In doing so, SQL worked marvelously well:

```sql
select
  round(
    exp(
      sum(
        ln(total_traveled)
      )
    )
  ) as solve
from 
  aggregated
```

I think the main takeaway for me is that, at the end of the day, most software engineering challenges are data transformation challenges - how do we move and shape data from where it rests to where it adds value. Having an understanding of the nature of the data, and the ability of pure mathematics to solve problems can be a powerful addition to a toolbox where - perhaps too often - tools such as iteration are often used at the expense of performance and cost.
