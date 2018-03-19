# 627 Swap Salary

Given a table salary, such as the one below, that has m=male and f=female values. Swap all f and m values (i.e., change all f values to m and vice versa) with a single update query and no intermediate temp table.
For example:

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |

After running your query, the above salary table should have the following rows:

| id | name | sex | salary |
|----|------|-----|--------|
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |

## Explanation :

A table with 'sex' as one of its column is given and using the following approach we can swap this values :

1. See Leetcode solution down below. It can be expandable, if we have more than 2 variables to swap.
2. 


## Leetcode solution :

*Approach*: **Using UPDATE and CASE...WHEN [Accepted]**

*Algorithm*:

To dynamically set a value to a column, we can use UPDATE statement together when CASE...WHEN... flow control statement.

*MySQL* :

  ` UPDATE salary
    SET
      sex = CASE sex
          WHEN 'm' THEN 'f'
          ELSE 'm'
    END;
  `