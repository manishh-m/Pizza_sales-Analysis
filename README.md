<div align="center"> <h1>Analysis of Pizza Sales</h1>
</div>



----

__Tools used : Excel,MYSQL Server,Power BI__

[Dataset used](https://1drv.ms/x/s!AmKU00K1sOXkiXYGCmYAQfrCOGMJ?e=lz8diP)


__Business Problem__
-------
To handle the huge amount of data (about 50k rows of data combined), a pizza company needs an effective and scalable data analytics solution to analyze and extract meaningful insights such as trends, order patterns, menu performance, customer preferences, and other critical business metrics. In addition to identifying trends and insights, visualizations must be made for sales by pizza type and size, top/bottom sellers, and daily and monthly order volume. The goal is to provide the company with data-driven insights that guide strategic choices and boost productivity and profitability.

__Solution Plan__
-------
+ We will use Power BI's data visualization capabilities and SQL to extract relevant information and carry out insightful analyses to assist the pizza company in gaining important insights from their sales data CSV file.
+ We can find important metrics like total revenue, average order size, and best/worst selling menu items by utilizing SQL's functions. After the data has been prepared and extracted, we will use Power BI to create interactive visualizations that show the results. 
+ we intend to create a dynamic Power BI dashboard that allows users to view daily order volume and sales data by pizza type/size. The ultimate goal is to provide the business with data-driven insights to inform strategic decisions that improve operations and profitability.

__Execution__
---------

__Questions Answered from the Dataset__

__1) What are the Key Performance Indicators obtained from the Dataset?__

+ __Total Revenue:__
  
        select sum(total_price) as Total_revenue from pizza_sales 
   
![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/9ea0098f-7e84-46c7-99f8-a3237ca7d213)

This metric provides a clear measure of the overall financial performance of the pizza sales. It indicates the total amount of money generated from pizza 
orders over a specific period, reflecting the revenue potential of the business.
      
+ __Average Order Value:__
    
        select sum(total_price)/ count(distinct order_id) as avg_orderValue from pizza_sales
    
![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/135c47f6-0d4c-4dfc-bfed-36f2d59438db))

Average order value helps in understanding customer spending habits and the effectiveness of marketing strategies. A higher average order value indicates that 
customers are spending more per transaction, which can contribute to increased revenue and profitability.

+ __Total Pizzas Sold:__
    
        select sum(quantity) as total_count from pizza_sales
     
![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/5306723d-55da-41c2-9810-058b5956bef8)

    
Total pizzas sold is a fundamental metric that directly reflects product demand. It provides insights into consumption patterns and helps in inventory management and production planning. Understanding total pizzas sold enables businesses to meet customer demand efficiently.
    
+ __Total Orders:__
    
        SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales
     
![image](https://github.com/sharanya-27/pizza_sales_analysis/assets/142989454/bc9cebcf-3b72-4bcf-8e00-2e73690efc84)

Total orders represent the volume of transactions processed within a specific period. Tracking total orders helps in evaluating sales performance and 
identifying trends in customer behavior. It provides a basis for assessing business growth and operational efficiency.

+ __Average Pizzas Per Order:__
    
        select sum(quantity)/ count(distinct order_id) as avg_pizza_per_order from pizza_sales 
        
![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/393cae7e-a07b-4fb6-99c5-cde9bc823bd1)


Average pizzas per order indicates the average quantity of pizzas purchased in each transaction. This metric offers insights into customer preferences and 
ordering behavior. Higher average pizzas per order may indicate upselling opportunities or popular menu items.
    
__2) Daily Trend for Total Orders__

        	SELECT DAYNAME(STR_TO_DATE(order_date, '%d-%m-%Y')) AS order_day, COUNT(DISTINCT order_id) AS total_orders
			    FROM pizza_sales
			    GROUP BY DAYNAME(STR_TO_DATE(order_date, '%d-%m-%Y'));
       
Note: the data is in dd-mm-yyyy format but mysql stores it in yyyy-mm-dd format, so convert that and then query for results.
        
![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/f372c5b4-5311-44af-8c77-cb98a7d4aef7)


By analyzing the daily trend of total orders over a specific time period, we can identify patterns and fluctuations in order volumes on a daily basis. This 
helps in understanding the demand patterns throughout the week or month, enabling better inventory management and resource allocation.

 __3) Monthly Trend for Orders__

        	SELECT MONTHNAME(STR_TO_DATE(order_date, '%d-%m-%Y')) AS order_day, COUNT(DISTINCT order_id) AS total_orders
				FROM pizza_sales
				GROUP BY MONTHNAME(STR_TO_DATE(order_date, '%d-%m-%Y'));

![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/8ff93461-ca09-4608-a07e-b167d5f99100)

The monthly trend of total orders offers insights into the overall order activity throughout the month. Identifying peak periods of high order activity can aid in resource allocation, production planning, and promotional efforts. By recognizing monthly trends, businesses can adjust their operations to capitalize on high-demand periods and optimize efficiency during slower periods.

__4) % of Sales by Pizza Category__

        			select pizza_category, sum(total_price)*100 / (select sum(total_price) from pizza_sales where month(STR_TO_DATE(order_date, '%d-%m-%Y'))=1) as total_Sales_percentage from pizza_sales
			        where month(STR_TO_DATE(order_date, '%d-%m-%Y'))=1
			        group by pizza_category
           
NOTE: here we have are getting percentage of sales of a pizza category for the month of january,ie, 1

![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/879d3857-9e29-40a1-ac31-61c79c9783a3)


The distribution of sales across different pizza categories helps in understanding the popularity of each category and its contribution to overall sales. This 
insight informs menu optimization strategies, marketing campaigns, and inventory management decisions to maximize revenue and customer satisfaction.

 __5) % of Sales by Pizza Size__

        			select pizza_size, sum(total_price)*100 / (select sum(total_price) from pizza_sales where month(STR_TO_DATE(order_date, '%d-%m-%Y'))=1) as total_Sales_percentage from pizza_sales
			        where month(STR_TO_DATE(order_date, '%d-%m-%Y'))=1
			        group by pizza_size

![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/82be72c7-ddc2-498c-ab39-b1182f5856a1)


The chart depicting the percentage of sales attributed to different pizza sizes provides insights into customer preferences regarding pizza size. 
Understanding these preferences allows for better inventory management and menu optimization to meet customer demand effectively.

__6) Total Pizzas Sold by Pizza Category__

        		select pizza_category, sum(quantity) as total_quantity from pizza_sales
		        group by pizza_category
		        order by total_quantity desc

![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/ca71686b-1e2d-4f6a-b132-85951e5d767d)


The metric showing total number of pizzas sold for each pizza category enables comparison of sales performance across different categories. This 
visualization helps in identifying popular and less popular pizza options, informing marketing strategies and menu adjustments accordingly.

__7) Top 5 Pizzas by Revenue__

        SELECT Top 5 pizza_name, SUM(total_price) AS Total_Revenue
        FROM pizza_sales
        GROUP BY pizza_name
        ORDER BY Total_Revenue DESC
        
![image](https://github.com/sharanya-27/pizza_sales_analysis/assets/142989454/69f9c56f-509c-4eaa-b938-2321ac706c2a)

The chart highlighting the top 5 best-selling pizzas based on revenue, total quantity, and total orders identifies the most popular pizza options. This 
insight aids in understanding customer preferences and allows for targeted promotions or menu enhancements to capitalize on high-demand items.

 __8) Bottom 5 Pizzas by Revenue__

        select pizza_name, sum(total_price) as revenue
        from pizza_sales
        group by pizza_name
        order by revenue asc
        limit 5

![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/f1bd2a45-0ec1-4624-8293-816fbc932e83)


Conversely, the chart showcasing the bottom 5 worst-selling pizzas based on revenue, total quantity, and total orders helps identify underperforming or less 
popular pizza options. This information is valuable for making strategic decisions such as discontinuing unpopular items or adjusting pricing and marketing 
strategies to boost sales.
 
__9) Top 5 Pizzas by Quantity__

        select pizza_name, sum(quantity) as quantity
        from pizza_sales
        group by pizza_name
        order by quantity desc
        limit 5
![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/fac5c22e-353c-4e16-8845-abd284d9b251)


Identifying the top 5 pizzas by quantity sold provides insight into the most popular pizza varieties among customers. This information helps in understanding 
customer preferences and allows for targeted marketing efforts or menu optimizations to capitalize on high-demand items.
 
__10) Bottom 5 Pizzas by Quantity__

        select pizza_name, sum(quantity) as quantity
        from pizza_sales
        group by pizza_name
        order by quantity asc
        limit 5

![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/40840614-3d58-4093-aff4-be5bebaa187e)


Conversely, identifying the bottom 5 pizzas by quantity sold highlights less popular or underperforming pizza options. This insight can inform menu 
adjustments, promotional strategies, or inventory management decisions to minimize waste and maximize profitability.
  
__11) Top 5 Pizzas by Total Orders__

        select pizza_name, count(distinct order_id) as total_orders
        from pizza_sales
        group by pizza_name
        order by total_orders desc
        limit 5

![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/fb00168c-1405-4aa0-8039-40a49ba992fe)


Identifying the top 5 pizzas by total orders sheds light on the most frequently ordered pizza varieties. This information helps in understanding customer 
behavior and preferences, enabling businesses to tailor marketing campaigns and menu offerings to meet customer demand effectively.

 __12) Bottom 5 Pizzas by Total Orders__

        select pizza_name, count(distinct order_id) as total_orders
        from pizza_sales
        group by pizza_name
        order by total_orders asc
        limit 5

![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/dc2f3b2f-8987-42fc-b3fc-753b1bd70942)


Identifying the bottom 5 pizzas by total orders reveals less popular or niche pizza options that may require attention or adjustments. Understanding these trends allows businesses to make informed decisions regarding menu adjustments, promotional strategies, or inventory management to optimize sales and customer satisfaction.

__13) Generated various Charts and Visuals on Power BI to identify trends/patterns and gain more insights into the data.__

![image](https://github.com/notmanishh/Pizza_sales-Analysis/assets/106374799/c32944b7-f521-4bdb-bff7-d76d8e1a9117)


__Conclusion__
---------

By exploring various aspects of the pizza sales dataset, a thorough understanding of consumer behavior and market trends was obtained. The analysis revealed important insights, such as the identification of peak and off-peak periods based on daily and monthly order patterns. This knowledge enables businesses to effectively optimize staffing and inventory management in response to varying demand levels. Furthermore, analyzing sales distribution across different pizza categories revealed customer preferences, which helped with menu optimization and marketing strategies to improve sales performance.

Furthermore, calculating the average number of pizzas sold per order gave businesses valuable insights into consumer behavior and consumption patterns, allowing them to optimize pricing strategies and portion sizes to maximize profitability and customer satisfaction. Overall, this analysis enables the pizza establishment to make data-driven decisions, streamline operations, and improve the overall customer experience, ensuring long-term success in the competitive pizza market.
