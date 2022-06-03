Types of SQLI

![[Pasted image 20220527101328.png]]

**Union Attack**
For a `UNION` query to work, two key requirements must be met:

-   The individual queries must return the same number of columns.
-   The data types in each column must be compatible between the individual queries.

Enumeration-

- The first method involves injecting a series of `ORDER BY` clauses
`' ORDER BY 1-- ' ORDER BY 2-- ' ORDER BY 3-- etc.`
- The second method involves submitting a series of `UNION SELECT` payloads specifying a different number of null values:

`' UNION SELECT NULL-- ' UNION SELECT NULL,NULL-- ' UNION SELECT NULL,NULL,NULL-- etc.`

**Remediations -** 
- Prepared Statements (With Parameterized Queries)
- Input Validation
- Escaping User Input


