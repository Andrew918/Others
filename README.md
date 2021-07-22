# Others
SELECT * FROM (server name, 'SELECT * FROM DB WHERE Date >= ''3/11/20'') ORDER BY
select * from openquery(att_consumer, 'select * from app_consumer_stage.oracle_daily_link_level where campaign like ''%1138113%''
and sent_date between ''5/1/21'' and ''5/31/21''')

-----------------------------
Pivot Table
select segment_values, sum(React_SENT), sum(React_OPEN), sum(React_CLICK),
       (isnull(sum(React_SENT),0) + isnull(sum(React_OPEN),0) + isnull(sum(React_CLICK),0)) 'Saves'
FROM
(
SELECT T1.*, 
       T2.[SENT] 'React_SENT',
	   T2.[OPEN] 'React_OPEN', 
	   T2.[CLICK] 'React_CLICK'
FROM IREPORT_PROD.sandbox.[oracle_campaign_cell_data] AS T1
LEFT JOIN
(
SELECT * FROM   
(
    SELECT
	    cell_cd,
		contct_dt,
        cust_action, 
        reactivations
    FROM 
       IREPORT_PROD.dbo.ATTTV_PDIS_RECONNECTS
) t 
PIVOT(
    SUM(reactivations)
    FOR cust_action IN (
        [SENT], 
        [OPEN], 
        [CLICK])
) AS pivot_table
) AS T2
ON T1.sent_date = T2.contct_dt
AND T1.segment_values = T2.cell_cd
WHERE T1.campaign like '%1475913%'
) as T
---where T.sent_date between '2021-05-01' and '2021-05-30'
where T.sent_date = '2021-06-25'
GROUP BY ROLLUP(segment_values)

------------------------------


