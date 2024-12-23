# Novel grid-style programming
## Excel-like Grid-style programming
![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/1.png)

### Debugging SQL can be extremely cumbersome
**Extract SQL to debug intermediate steps**
![image](https://github.com/hoocx1/myTest/blob/main/test/QQ20241223-112120.jpg?raw=true)

### Debugging Python is also tedious
**Employ print method to output intermediate results**
![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/3-3.png)

## SPLUser-friendly Debugging IDE
![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/3-1.png)

# High Interactivity for Exploratory Analysis
![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/4.gif)

## XLL Plugin Helps Excel
Write SPL code in Excel directly
**Finding periods during which stocks have risen consecutively for more than 5 days**

```
= spl （“=E（？1）.sort（CODE，DT）.group @ i（CODE！=CODE [-1] || CL <CL [-1]）.select（~.len（）> = 5）.conj（）” ，A1：D253 ）
```
![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/5.png)

# Concise and Powerful Code
## Comprehensive and Simple Operations
![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/6-1.png)
![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/6-2.png)
![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/6-3.png)
![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/6-4.png)

## Unique Set and Ordered Operations
**calculate the longest consecutive rising days for each stock**

- **SPL**

|  | A|
| --- | --- |
| 2 | StockRecords.xlsx |
| 3 | =T(A1).sort(DT) |
| 4 | =A2.group(CODE;\~.group@i(CL < CL[-1]).max(\~.len()):max_increase_days) |

Especially skilled at complex scenarios such as order-related operations, sliding windows, and cross-row computations,much simpler than SQL or Python

- **Python**

```
import pandas as pd
stock_file = "StockRecords.txt"
stock_info = pd.read_csv(stock_file,sep="\t")
stock_info.sort_values(by=['CODE','DT'],inplace=True)
stock_group = stock_info.groupby(by='CODE')
stock_info['label'] = stock_info.groupby('CODE')['CL'].diff().fillna(0).le(0).astype(int).cumsum()
max_increase_days = {}
for code, group in stock_info.groupby('CODE'):
    max_increase_days[code] = group.groupby('label').size().max() – 1
max_rise_df = pd.DataFrame(list(max_increase_days.items()), columns=['CODE', 'max_increase_days'])
```
- **SQL**

```
SELECT CODE, MAX(con_rise) AS longest_up_days
FROM (
    SELECT CODE, COUNT(*) AS con_rise
    FROM (
        SELECT CODE, DT,  SUM(updown_flag) OVER (PARTITION BY CODE ORDER BY CODE, DT) AS no_up_days
        FROM (
            SELECT CODE, DT, 
                    CASE WHEN CL &gt; LAG(CL) OVER (PARTITION BY CODE ORDER BY CODE, DT)  THEN 0
                    ELSE 1 END AS updown_flag
            FROM stock
        )
    )
    GROUP BY CODE, no_up_days
)
GROUP BY CODE
```

**[What to use for data analysis programming: SPL,Python or SPL?](https://www.scudata.com/langding-page-scientist/)**

## Easy Big Data and Parallel Support
- **Big Data**

In Memory
|   | A |
| --- | --- |
| 1 | StockRecords.txt |
| 2 | =file(A1).import@t().sort(CODE,DT) |
| 3 | =A2.group(CODE;\~.group@i(CL < CL[-1]).max(\~.len()):mi) |

External Storage
|   | A |
| --- | --- |
| 1 | StockRecords.txt |
| 2 | =file(A1).**cursor**@t().sort(CODE,DT) |
| 3 | =A2.group(CODE;\~.group@i(CL < CL[-1]).max(\~.len()):mi) |

- **Parallel Computation**

In Memory
|   | A |
| --- | --- |
| 1 | StockRecords.txt |
| 2 | =file(A1).cursor@t().sortx(CODE,DT) |
| 3 | =A2.group@i(CODE!=CODE[-1]||CL< CL[-1]) |
| 4 | =A3.select(~.len()>=5).conj() |

External Storage
|   | A |
| --- | --- |
| 1 | StockRecords.txt |
| 2 | =file(A1).cursor@t**m**().sortx(CODE,DT) |
| 3 | =A2.group@i(CODE!=CODE[-1]||CL< CL[-1]) |
| 4 | =A3.select(~.len()>=5).conj() |

## Lightweight and Portable
![image](https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-SPL-Data-Analysis/9.png)
