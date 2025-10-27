# CIS 5570 Programming Assignment 2 -> Group 2 - Online#


## Authors ##
- Tejas Vaity
- Brad Byard
- Anthony Lewis


## Platform used for Assignment ##
We completed this assignment using PySpark in a distributed setup on a shared Google Colab file with teammates.

## How to run the Program ##
- Load the Programming_Assignment_2_Group2Online.ipynb file in your google collab and on top left click on Run all.
- The pyspark environment is setup in the first code blocks and will setup when you run automatically.
- The assignment is divided into four parts, each generating its own output files. Once all parts are executed, the resulting files will be saved in the Colab working directory and can be easily accessed and reviewed.
- For Part I, test files 3x3-test1.txt and 4x5-test2.txt are provided. You can modify their values for testing by running the first three code blocks also some default values are already pre-filled.
- Similarly for Part III you can edit test3.txt to try your own input values.

## Working of Code ##

- Part I
The program reads two test input files 3x3-test1.txt and 4x5-test2.txt, The code uses map() transformations to convert each line into a key-value pair like {(row,column), value}. Then reduceByKey() adds all the values for the same cell positions. The resulting RDD is sorted by row and column indices and then saved in both CSV and TXT formats.

- Part II 
Same steps as Part I by adding reduceByKey(lambda a, b: a + b). Spark adds the elements for the same (row, column) coordinates, then sorted using sortBy(lambda kv: (kv[0][0], kv[0][1])) and saved the files to csv and text files.

- Part III
Each line in the input defines a row of matrix M or column N. Spark maps and expands these rows to individual cell elements. Using RDD transformations the entries are matched and the element wise products are computed and added for each resulting cell (i, j).

- Part IV
The input file contains both matrices (M and N) with their respective indices and values. Each line is parsed into a tuple (matrix, row, col, value).Using Spark transformations: M entries are keyed by their column (b) → (b, ('M', a, val)) and N entries are keyed by their row (a) → (a, ('N', b, val)). The data is grouped by these keys to find all combinations where matrix indices align for multiplication. The program computes partial products for all matching pairs of elements and sums them for each (i, j) cell using reduceByKey(lambda x, y: x + y).The final matrix result is formatted and saved in matrixMultiplicationFinal.txt and matrixMultiplicationFinal.csv files.

- The code automatically handles any valid matrix size as long as the dimensions are compatible for matrix addition or multiplication.

## Performance ##
But Part II and Part IV its hard to tell difference but Part II code gives result fast than Part IV by 10-20 seconds. The runtime is short because matrix addition requires only a single reduceByKey pass. It can be improved by using .coalesce(1) and cahnging it to more than 1. But multiplication is computationally heavier than addition since each cell in the result requires iterating through multiple elements from both matrices. It can be improved by replacing groupByKey with reduceByKey or Spark’s built-in join() can reduce shuffle time and memory usage.