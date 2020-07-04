-- 1. Data quality check
SELECT 
  * 
FROM 
  dsv1069.final_assignments_qa
  
-- 2. Reformat the data
-- CREATE TABLE IF NOT EXISTS `my_final_assignments` (
--   item_id           INT(4) NOT NULL,
--   test_assignment   INT(4),
--   test_number       VARCHAR(16),
--   test_start_date   DATETIME
-- );

-- INSERT INTO `my_final_assignments`

(
  SELECT 
    item_id, 
    test_a AS test_assignment,
    (CASE WHEN test_a IS NOT NULL THEN 'item_test_1' ELSE NULL END) AS test_number,
    (CASE WHEN test_a IS NOT NULL THEN '2013-01-05 00:00:00' ELSE NULL END) AS test_start_date
  FROM 
    dsv1069.final_assignments_qa
)
UNION
(
  SELECT 
    item_id, 
    test_b AS test_assignment,
    (CASE WHEN test_b IS NOT NULL THEN 'item_test_2' ELSE NULL END) AS test_number,
    (CASE WHEN test_b IS NOT NULL THEN '2015-03-14 00:00:00' ELSE NULL END) AS test_start_date
  FROM 
    dsv1069.final_assignments_qa
)
UNION
(
  SELECT 
    item_id, 
    test_c AS test_assignment,
    (CASE WHEN test_c IS NOT NULL THEN 'item_test_3' ELSE NULL END) AS test_number,
    (CASE WHEN test_c IS NOT NULL THEN '2016-01-07 00:00:00' ELSE NULL END) AS test_start_date
  FROM 
    dsv1069.final_assignments_qa
)
UNION
(
  SELECT 
    item_id, 
    test_d AS test_assignment,
    (CASE WHEN test_d IS NOT NULL THEN 'item_test_4' ELSE NULL END) AS test_number,
    (CASE WHEN test_d IS NOT NULL THEN '2017-01-07 00:00:00' ELSE NULL END) AS test_start_date
  FROM 
    dsv1069.final_assignments_qa
)
UNION
(
  SELECT 
    item_id, 
    test_e AS test_assignment,
    (CASE WHEN test_e IS NOT NULL THEN 'item_test_5' ELSE NULL END) AS test_number,
    (CASE WHEN test_e IS NOT NULL THEN '2018-01-07 00:00:00' ELSE NULL END) AS test_start_date
  FROM 
    dsv1069.final_assignments_qa
)
UNION
(
  SELECT 
    item_id, 
    test_f AS test_assignment,
    (CASE WHEN test_f IS NOT NULL THEN 'item_test_6' ELSE NULL END) AS test_number,
    (CASE WHEN test_f IS NOT NULL THEN '2018-01-07 00:00:00' ELSE NULL END) AS test_start_date
  FROM 
    dsv1069.final_assignments_qa
)
LIMIT 100

-- 3. Compute order binary
SELECT
  test_assignment,
  COUNT(item_id)      AS items,
  SUM(orders_binary)  AS items_ordered_30d
FROM
  (
    SELECT
      test_assignment,
      final_assignments.item_id,
      MAX(CASE WHEN created_at > test_start_date AND
                    DATE_PART('day', created_at - test_start_date) <= 30
                    THEN 1 ELSE 0 END) AS orders_binary
    FROM
      dsv1069.final_assignments
    LEFT JOIN 
      dsv1069.orders
    ON 
      final_assignments.item_id = orders.item_id
    WHERE 
      test_number = 'item_test_2'
    GROUP BY
      test_assignment,
      final_assignments.item_id
  ) items_ordered
GROUP BY
  test_assignment
LIMIT 100

-- 4. Compute view item metrics
SELECT
  test_assignment,
  COUNT(item_id)                            AS items,
  SUM(views_binary)                         AS items_viewed_30d,
  100 * SUM(views_binary) / COUNT(item_id)  AS viewed_percent,
  SUM(views)                                AS total_views,
  SUM(views) / COUNT(item_id)               AS average_views_per_item
FROM
  (
    SELECT
      test_assignment,
      final_assignments.item_id,
      MAX(CASE WHEN event_time > test_start_date THEN 1 ELSE 0 END) AS views_binary,
      COUNT(event_id) AS views
    FROM
      dsv1069.final_assignments
    LEFT JOIN 
      (
        SELECT 
          event_time,
          event_id,
          CAST(parameter_value AS INT) AS item_id
        FROM 
          dsv1069.events 
        WHERE 
          event_name = 'view_item'
        AND 
          parameter_name = 'item_id'
      ) views
    ON
      final_assignments.item_id = views.item_id
    AND
      event_time > test_start_date
    AND 
      DATE_PART('day', event_time - test_start_date) <= 30
    WHERE 
      test_number = 'item_test_2'
    GROUP BY
      test_assignment,
      final_assignments.item_id
  ) items_viewed
GROUP BY
  test_assignment
LIMIT 100
