# 1112-Highest-Grade-For-Each-Student
1112. 每位学生的最高成绩  
SELECT student_id, course_id, grade  
FROM (  
  SELECT student_id, course_id, grade,  
    ROW_NUMBER() OVER (PARTITION BY student_id ORDER BY grade DESC, course_id) AS row_num  
  FROM Enrollments  
) AS subquery  
WHERE row_num = 1  
ORDER BY student_id;  


当我们解释上述的SQL查询时，我们可以将其分为以下几个部分：

    子查询（Subquery）：

sql

SELECT student_id, course_id, grade,  
  ROW_NUMBER() OVER (PARTITION BY student_id ORDER BY grade DESC, course_id) AS row_num  
FROM Enrollments  

在这个子查询中，我们从 Enrollments 表中选择 student_id、course_id 和 grade 列，并使用窗口函数 ROW_NUMBER() 为每个学生的成绩行分配一个行号。PARTITION BY student_id 指定按学生ID分组，ORDER BY grade DESC, course_id 指定按成绩降序和 course_id 升序排序。

    外部查询（Outer Query）：

sql

SELECT student_id, course_id, grade  
FROM subquery  
WHERE row_num = 1  
ORDER BY student_id;  

在外部查询中，我们从子查询中选择行号为1的记录，这些记录代表每位学生获得的最高成绩。然后按学生ID进行排序，以满足要求的结果排序。

综合起来，这个SQL查询通过子查询计算每位学生的每门课程的成绩，并按照要求的条件排序。然后，在外部查询中选择每位学生的最高成绩记录，并按学生ID排序，最后返回结果。

