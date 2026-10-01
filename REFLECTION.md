# REFLECTION

This was my first time using Power BI with GitHub and Copilot together. At first I didn't understand how a Power BI file could be put in Git, but saving it as a .pbip project turned the model and measures into text files, so each commit shows exactly what I added.

Copilot was really useful for the DAX. It gave me all the measures in one answer, and Total Sales, Sales MoM % and Running Total worked straight away. But I learned that code that looks right is not always right. The Item Rank measure ranked the Total row as 1, and I only saw it when I tested it in a table. I fixed it with ISINSCOPE. For Cold Brew Share, Copilot's version worked but was too complicated, so I made it simpler.

The README was where Copilot surprised me the most, it invented folders that don't exist, a KPI section I never built, and it even said Cold Brew was stronger in the later months, which is the opposite of my data. I had to rewrite those parts.

Some problems weren't Copilot's fault at all. My laptop is in French, so sales_amount came in as text, and July only had one day of data. Next time I will check the data types first and test everything before trusting it.