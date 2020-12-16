## Operational vs Analytical business processes
Operational Processes:
- Find goods & make orders
- Stock and find goods
- Pick up & deliver goods

Analytical Processes:
- Assess the performance of sales staff
- See the effect of different sales channels 
- Monitor sales growth 

Operational database:
- Excellent for operations
- No redundancy, high integrity
- Too slow for analytics, too many joins
- Too hard to understand

Solution: create 2 processing modes, create a system for them to co-exist
- OLTP: online transactional processing
- OLAP: online analytical processing
- Data Warehouse is system (inclusing processes, technologies and data representations) that enable us to support analytical processes

