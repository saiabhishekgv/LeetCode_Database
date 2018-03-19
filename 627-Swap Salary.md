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

1. ** Using  CASE :**  See <a href="https://github.com/saiabhishekgv/LeetCode_Database/blob/master/627-Swap%20Salary.md#leetcode-solution-"> Leetcode solution </a>. It can be expandable, if we have more than 2 variables to swap.

2. ** Using  XOR : ** If we *xor* a number with the same number it will cancel out to 0. Using this concept, Get the ascii value of 'f' and 'm' and do xor with value of sex column. Query would be :

  `update salary set sex = CHAR(ASCII('f') ^ ASCII('m') ^ ASCII(sex));`

3. ** Using  IF :** Same as approach 1 ( Using case ) but here we have only two variables to swap. Hence, using a IF would be a better choice. `` IF ( expression, True, False )``  is the set action to be performed. Query would be :

  `UPDATE salary SET sex = IF(sex = 'm', 'f', 'm');`


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
