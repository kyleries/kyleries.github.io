<img alt="Kyle Ries, software and data engineer" src="https://user-images.githubusercontent.com/1095673/206064782-d8f875b2-eafe-45c3-9f20-974fcdc467d6.png">

***

## Google BigQuery, a Detailed Look at APPROX_COUNT_DISTINCT
02/09/2023

# Context

DAUs, MAUs, and WAUs - oh my! Active User counts are likely nothing new for most folks in the Analytics/Data engineering community, particularly for applications and businesses that benefit from repeat engagement. Daily, Weekly, and Monthly active user counts are very common metrics in those domains. However, the way that data is capture and stored can vary. Perhaps you are able to use a 3rd party tool or service (e.g. Google Analytics). Or, perhaps your context is such that you have a detailed log of all active users interacting with your platform. For this post, we will explore the latter. 

Imagine you are tasked with building a CEO level dashboard. One of the requirements is to display D/W/MAUs, along with a reasonable comparative period (e.g. compared to last year). The data is available to you in the following format:

| date        | user_id     | platform
| ----------- | ----------- | ----------
| 01-01-2021  | 123         | ios
| 01-02-2021  | 123         | web
| 01-01-2021  | 456         | android
| 01-04-2021  | 456         | web
| 01-04-2021  | 456         | android

You understand that a given date/User ID/platform combination is unique. So, a user may be logged twice if they are active on multiple platforms on a given day.

# Situation

You are asked to compute the number of active users for a given period for purposes of creating the dashboard. This is simple enough for a relatively small set of data. Perhaps you elect the following:

<insert dag here>

<link to dbt project here>

# Problem

But, what if the volume of data you are working with is dramatically larger? What if your platform has millions of active users per day? How can you compute an accurate aggregate statistic (e.g. across a week or month). Can you use the same code? It seems the answer is no:

<insert image of Bigquery error message>

In these cases of counting distinct IDs, the challenge of doing so over a prolonged period of time is that the query engine has to look across the entire period in order to keep track of the unique IDs logged during that period of time. This poses a massive problem. 

# Solution

Enter a solution based on statistics - implemented in BigQuery as (APPROX_COUNT_DISTINCT)[https://cloud.google.com/bigquery/docs/reference/standard-sql/approximate_aggregate_functions#approx_count_distinct], and more generally belonging to a class of functions known as [HyperLogLog++](https://cloud.google.com/bigquery/docs/reference/standard-sql/hll_functions#hyperloglog_functions). 

The usage of this function is relatively straightforward enough. One can simply replace the original `COUNT DISTINCT` with the `APPROX_COUNT_DISTINCT` call. However, as any seasoned data engineer knows, such magic always come with a tradeoff. In this case, we are electing to substitute a highly precised count of IDs with a statistical approximation. This should cause you to ask any number of questions about that tradeoff, such as:

 - [How close is the approximation to the actual](https://stackoverflow.com/questions/71839073/precision-of-approx-count-distinct-in-big-query)?
 - Is the approximation deterministic?
 - How expensive is the use of this function?
 - Are there potentially other solutions that could be employed (partitioning)? 

So, let's explore each of these questions in detail so that we may confidently implement this approach and mitigate the risk that we'll get that dreaded call from the CEO, inquiring as to "Why the numbers have changed". 

# Preparation

In order to simulate and explore these questions, we're going to need to build a lot of data. I mean, a lot. After all, the intent here is to cause BigQuery to timeout - not always an easy feat! And, fortunately, we needn't make the generation of our test data overly complex. Certainly, there are patterns to real world user behavior that we could emulate, but I will assume for the sake of this exercise that the user patterns are largely random. By definition, we'll want to load these files directly into BigQuery as native tables, so we'll prepare our script locally to work out any bugs, then we will run it directly on a compute instance so we can directly ship the files to GCS.

The volume of data is TBD. We'll need to prove BigQuery's breaking point with the following test query:

```sql

select

```

Once we reach a limit that this query fails to execute, we'll have proven our breaking point. This will also give us the advantage of understanding where that threshold is. This is important, because we will be able to craft a dataset where we can use `COUNT DISTINCT` directly and compare it against `APPROX COUNT DISTINCT`. 


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
