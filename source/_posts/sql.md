---
title: SQL
date: 2016-05-30 15:31:28
categories: Database
tags: [Oracle, SQL]
---
oracle以及sql的常用语句
<!--more-->

```
CREATE OR REPLACE VIEW V_NET_COURIERLABEL AS
SELECT LC.FCOURIERID,LC.FLABELID,L.FLABEL,LC.FCOUNT FROM
(
       SELECT  LC.FCOURIERID,FLABELID,COUNT(*) AS FCOUNT FROM T_NET_LABELCOMMENT LC
       GROUP BY LC.FCOURIERID, LC.FLABELID
) LC
INNER JOIN T_NET_LABEL L  ON LC.FLABELID = L.FID
ORDER BY LC.FCOURIERID;
```

```
select * from t_net_labelcomment t 
select t.fcourierid, t.flabelid, count(*) as count from t_net_labelcomment t group by t.fcourierid, t.flabelid order by count desc;
select count(*) from t_net_labelcomment t where t.fcourierid = 678446 and t.flabelid = 12
```

```
#limit用法：
select tt.* from (select rownum rn, t.* from t_net_company t) tt where rn between 5 and 10

#orcle毫秒数转换为timestamp:
SELECT TO_TIMESTAMP('1970-01-01 00:00:00.000','yyyy-MM-dd hh24:mi:ss.ff3')+1437840397390/1000/60/60/24 FROM dual;

#日期查询：
select count(1) from t_net_smshistory t where t.fcourierid = '677644'and to_char(t.fcreatetime,'yyyy/mm/dd') = '2015/11/19' order by t.fcreatetime desc

#手机号查看短信历史
select * from t_net_smshistory t left join t_net_courier c on t.fcourierid = c.fid where c.fcouriertel = '15312986697' and t.fsuccess = 0 and to_char(t.fcreatetime,'yyyy/mm/dd') = '2016/06/18' order by t.fcreatetime desc
```