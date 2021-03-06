# 626. Exchange Seats

Mary is a teacher in a middle school and she has a table seat storing students' names and their corresponding seat ids.

The column id is continuous increment.
Mary wants to change seats for the adjacent students.
Can you write a SQL query to output the result for Mary?


|    id   | student |
|---------|---------|
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |

For the sample input, the output is:

|    id   | student |
|---------|---------|
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |

Note:
If the number of students is odd, there is no need to change the last one's seat.

## Explanation :

A table with 'id' and 'student' as columns are given and using the following approach we can swap the id values :

1. **Using  CASE :**  See <a href="https://github.com/saiabhishekgv/LeetCode_Database/blob/master/626-Exchange%20Seats.md#leetcode-solution-"> Leetcode solution Approach 1</a>. It can be expandable, if we have more than 2 variables to swap.

2. **Using  XOR and COALESCE() :** If we *xor* a number with the same number it will cancel out to 0. Using this concept, Get the ascii value of 'f' and 'm' and do xor with value of sex column. See <a href="https://github.com/saiabhishekgv/LeetCode_Database/blob/master/626-Exchange%20Seats.md#leetcode-solution-"> Leetcode solution Approach 2</a>.


## Leetcode solution :

*Approach I:*  **Using flow control statement CASE**

*Algorithm*:

For students with odd id, the new id is (id+1) after switch unless it is the last seat. And for students with even id, the new id is (id-1). In order to know how many seats in total, we can use a subquery:

  `
  SELECT
      COUNT(*) AS counts
  FROM
      seat
  `

Then, we can use the CASE statement and MOD() function to alter the seat id of each student.

*MySQL* :

  `SELECT
      (CASE
          WHEN MOD(id, 2) != 0 AND counts != id THEN id + 1
          WHEN MOD(id, 2) != 0 AND counts = id THEN id
          ELSE id - 1
      END) AS id,
      student
  FROM
      seat,
      (SELECT
          COUNT(*) AS counts
      FROM
          seat) AS seat_counts
  ORDER BY id ASC;
  `

*Approach II:* **Using bit manipulation and COALESCE()**

*Algorithm*:  Bit manipulation expression  `(id+1)^1-1` can calculate the new id after switch.

  `SELECT id, (id+1)^1-1, student FROM seat;`

  | id |`(id+1)^1-1`| student |
  |----|------------|---------|
  | 1  | 2          | Abbot   |
  | 2  | 1          | Doris   |
  | 3  | 4          | Emerson |
  | 4  | 3          | Green   |
  | 5  | 6          | Jeames  |

Then, we can make a temp table and join seat with this table like below.

  `SELECT
      *
  FROM
      seat s1
          LEFT JOIN
      seat s2 ON (s1.id+1)^1-1 = s2.id
  ORDER BY s1.id;`

  | id | student | id | student |
  |----|---------|----|---------|
  | 1  | Abbot   | 2  | Doris   |
  | 2  | Doris   | 1  | Abbot   |
  | 3  | Emerson | 4  | Green   |
  | 4  | Green   | 3  | Emerson |
  | 5  | Jeames  |    |         |

Note: The first two columns are from s1 and the last two are from s2.

At last, we can output s1.id and s2.student. However, the s2.student is NULL for seat id '5' but s1.student is right. Thus, we we can use function COALESCE() to generate the correct output for the last record.

*MySQL*:

`  SELECT
      s1.id, COALESCE(s2.student, s1.student) AS student
  FROM
      seat s1
          LEFT JOIN
      seat s2 ON ((s1.id + 1) ^ 1) - 1 = s2.id
  ORDER BY s1.id;
`
