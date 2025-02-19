# PizzaSales

SELECT
    s.transaction_id,
    s.date,
    s.state,
    s.order_channel,
    s.units_sold,
    s.price_per_unit,
    s.discount,
    p.pizza_name,
    p.category,
    p.size,
    p.cost_to_make
FROM dbo.Sales AS s
JOIN dbo.Pizza AS p
    ON s.pizza_id = p.pizza_id;


--Total Units Sold

SELECT
    SUM(s.units_sold) AS total_units_sold
FROM dbo.Sales AS s;

--Total Revenue
SELECT
    SUM( (s.price_per_unit - s.discount) * s.units_sold ) AS total_revenue
FROM dbo.Sales AS s;

--Gross Profit
SELECT
    SUM( (s.price_per_unit - s.discount - p.cost_to_make) * s.units_sold ) AS total_gross_profit
FROM dbo.Sales AS s
JOIN dbo.Pizza AS p
    ON s.pizza_id = p.pizza_id;


--Average Discount Amount
SELECT
    AVG(s.discount) AS avg_discount
FROM dbo.Sales AS s;

--Total Units Sold by Category
SELECT
    p.category,
    SUM(s.units_sold) AS total_units_sold
FROM dbo.Sales AS s
JOIN dbo.Pizza AS p
    ON s.pizza_id = p.pizza_id
GROUP BY p.category
ORDER BY total_units_sold DESC;

--Units Sold Monthly Trend
SELECT
    YEAR(s.date) AS year_num,
    MONTH(s.date) AS month_num,
    s.state,
    SUM(s.units_sold) AS total_units_sold
FROM dbo.Sales AS s
GROUP BY
    YEAR(s.date),
    MONTH(s.date),
    s.state
ORDER BY
    year_num,
    month_num,
    s.state;

--Units Sold by Pizza Name
SELECT
    p.pizza_name,
    SUM(s.units_sold) AS total_units_sold
FROM dbo.Sales AS s
JOIN dbo.Pizza AS p
    ON s.pizza_id = p.pizza_id
GROUP BY p.pizza_name
ORDER BY total_units_sold DESC;

--Units Sold by Pizza Size
SELECT
    p.size,
    SUM(s.units_sold) AS total_units_sold
FROM dbo.Sales AS s
JOIN dbo.Pizza AS p
    ON s.pizza_id = p.pizza_id
GROUP BY p.size
ORDER BY total_units_sold DESC;

--Units Sold by Order Channel
SELECT
    s.order_channel,
    SUM(s.units_sold) AS total_units_sold
FROM dbo.Sales AS s
GROUP BY s.order_channel
ORDER BY total_units_sold DESC;
