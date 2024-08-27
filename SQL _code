--QUIZ FUNNEL
SELECT *
FROM survey
LIMIT 5;

--Analyze how many users answered each question
SELECT question, COUNT(DISTINCT user_id) AS 'Num responses'
FROM survey
GROUP BY 1
LIMIT 10;

-- HOME TRY ON FUNNEL
SELECT *
FROM quiz
LIMIT 5;

SELECT *
FROM home_try_on
LIMIT 5;

SELECT *
FROM purchase
LIMIT 5;

--See rate of quizzed vs tried on and purchased and percentages
WITH totals_table AS (
  SELECT quiz.user_id,
  home_try_on.user_id IS NOT NULL AS 'is_home_try_on',
  home_try_on.number_of_pairs AS 'num_pairs',
  purchase.user_id IS NOT NULL AS 'is_purchase'
FROM quiz
LEFT JOIN home_try_on
  ON home_try_on.user_id = quiz.user_id
LEFT JOIN purchase
  ON purchase.user_id = quiz.user_id)
SELECT COUNT(user_id) AS "Total Quizzed",
  SUM(is_home_try_on) AS 'Total Tried at Home',
  100 * SUM(is_home_try_on) / COUNT(user_id) AS 'Percentage Tried',
  SUM(is_purchase) AS 'Total Purchased',
  100 * SUM(is_purchase) / COUNT(is_home_try_on) AS 'Percentage Purchased'
FROM totals_table;

--Look at difference of tried on and purchased between 3 and 5 pair home_try_on quiz
WITH totals_table AS (
  SELECT quiz.user_id,
    home_try_on.user_id IS NOT NULL AS 'is_home_try_on',
    home_try_on.number_of_pairs AS 'num_pairs',
    purchase.user_id IS NOT NULL AS 'is_purchase'
FROM quiz
LEFT JOIN home_try_on
  ON home_try_on.user_id = quiz.user_id
LEFT JOIN purchase
  ON purchase.user_id = quiz.user_id)
SELECT num_pairs AS "Pairs recd",
  SUM(is_home_try_on) AS 'Total Tried at Home',
  SUM(is_purchase) AS 'Total Purchased',
  100 * SUM(is_purchase) / COUNT(is_home_try_on) AS 'Percentage Purchased'
FROM totals_table
  WHERE num_pairs IS NOT NULL
GROUP BY 1;

-- Choices selected for each question in the survey
SELECT 
  question, 
  response,   
  COUNT(*) AS 'Total',
  100.0 * COUNT(*) / (
    SELECT COUNT(*)
    FROM survey
    WHERE question = survey.question
  ) AS 'Percentage'
FROM survey
GROUP BY question, response
ORDER BY question, response;

-- Total of answers selected for each question in the survey
--Count and Percentage of Style chosen
WITH style_table AS (
  SELECT COUNT(style) AS 'total_selected_style', 
    style
FROM quiz
GROUP BY 2
ORDER BY 1 DESC)
SELECT style AS 'Style Selected',
  total_selected_style AS 'Totals',
  100 * total_selected_style / (SELECT SUM(total_selected_style) FROM style_table) AS "Percentage"
FROM style_table;

--Count and Percentage of Fit chosen
WITH fit_table AS (
  SELECT COUNT(fit) AS 'total_selected_fit', 
    fit
FROM quiz
GROUP BY 2
ORDER BY 1 DESC)
SELECT fit AS 'Fit Selected',
  total_selected_fit AS 'Totals',
  100 * total_selected_fit / (SELECT SUM(total_selected_fit) FROM fit_table) AS "Percentage"
FROM fit_table;

--Count and Percentage of Shape chosen
WITH shape_table AS (
  SELECT COUNT(shape) AS 'total_selected_shape', 
    shape
FROM quiz
GROUP BY 2
ORDER BY 1 DESC)
SELECT shape AS 'Shape Selected',
  total_selected_shape AS 'Totals',
  100 * total_selected_shape / (SELECT SUM(total_selected_shape) FROM shape_table) AS "Percentage"
FROM shape_table;

--Count and Percentage of Color chosen
WITH color_table AS (
  SELECT COUNT(color) AS 'total_selected_color', 
    color
FROM quiz
GROUP BY 2
ORDER BY 1 DESC)
SELECT color AS 'color Selected',
  total_selected_color AS 'Totals',
  100 * total_selected_color / (SELECT SUM(total_selected_color) FROM color_table) AS "Percentage"
FROM color_table;

--Which product for men and womens style is most popular and which color

SELECT model_name, color, price, COUNT(*) AS 'Total Purchased', style
FROM purchase
WHERE style = "Men's Styles"
GROUP BY 1, 2
ORDER BY 4 DESC;

SELECT model_name, color, price, COUNT(*) AS 'Total Purchased', style
FROM purchase
WHERE style = "Women's Styles"
GROUP BY 1, 2
ORDER BY 4 DESC;
