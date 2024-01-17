# Table 1: RepairTechnicians
This table will store information about the technicians who repair bicycles.

# Table 2: RepairJobs
This table will store information about the repair jobs performed by technicians.

## Please note that these table structures are quite basic, and you may need to adjust them based on specific requirements or additional details you want to capture in your database. Additionally, you might want to add primary and foreign key constraints or feel free to add primary and foreign key constraints .

## Task 1: Aggregate Functions and Subqueries
Task: Find the average number of years of experience for technicians who have performed repair jobs on bicycles costing more than $100. Include the technician's name and the average experience.

```
SELECT
    rt.TechnicianName,
    AVG(rt.YearsOfExperience) AS AvgExperience
FROM
    RepairTechnicians rt
    INNER JOIN RepairJobs rj ON rt.TechnicianID = rj.TechnicianID
WHERE
    rj.RepairCost > 100
GROUP BY
    rt.TechnicianID, rt.TechnicianName;

```

## Task 2: Combining Joins and Subqueries
Task: List the names of technicians who have not performed any repair jobs.

```
SELECT
    TechnicianName
FROM
    RepairTechnicians rt
LEFT JOIN
    RepairJobs rj ON rt.TechnicianID = rj.TechnicianID
WHERE
    rj.TechnicianID IS NULL;

```

## Task 3: Using EXISTS and IN
Task: Find the technicians who have worked on repairs for bicycles owned by more than one person. Include the technician's name and the count of distinct bicycle owners.

```
SELECT
    rt.TechnicianName,
    COUNT(DISTINCT rj.BicycleOwner) AS OwnerCount
FROM
    RepairTechnicians rt
    INNER JOIN RepairJobs rj ON rt.TechnicianID = rj.TechnicianID
WHERE
    EXISTS (
        SELECT 1
        FROM RepairJobs rj2
        WHERE rj2.TechnicianID = rt.TechnicianID
        GROUP BY rj2.BicycleID
        HAVING COUNT(DISTINCT rj2.BicycleOwner) > 1
    )
GROUP BY
    rt.TechnicianID, rt.TechnicianName;

```

## Task 4: Subqueries with GROUP BY and HAVING
Task: Identify technicians who have performed repairs on at least 5 bicycles and list the total number of repairs they have conducted. Include the technician's name and the total number of repairs.

```
SELECT
    rt.TechnicianName,
    COUNT(*) AS TotalRepairs
FROM
    RepairTechnicians rt
    INNER JOIN RepairJobs rj ON rt.TechnicianID = rj.TechnicianID
GROUP BY
    rt.TechnicianID, rt.TechnicianName
HAVING
    COUNT(*) >= 5;

```

## Task 5: Subqueries with AND, OR, and NOT
Task: List the technicians who have performed repairs on bicycles with a cost between $50 and $150 OR have more than 7 years of experience. Include the technician's name and the repair cost.

```
SELECT
    rt.TechnicianName,
    rj.RepairCost
FROM
    RepairTechnicians rt
    INNER JOIN RepairJobs rj ON rt.TechnicianID = rj.TechnicianID
WHERE
    (rj.RepairCost BETWEEN 50 AND 150)
    OR (rt.YearsOfExperience > 7);

```

## Task 6: Combining EXISTS and Aggregate Functions
Task: Find technicians who have not performed any repairs on bicycles with a cost exceeding $200. Include the technician's name and the total number of such repairs.

```
SELECT
    rt.TechnicianName,
    COUNT(*) AS TotalExpensiveRepairs
FROM
    RepairTechnicians rt
WHERE
    NOT EXISTS (
        SELECT 1
        FROM RepairJobs rj
        WHERE rj.TechnicianID = rt.TechnicianID AND rj.RepairCost > 200
    )
GROUP BY
    rt.TechnicianID, rt.TechnicianName;

```
