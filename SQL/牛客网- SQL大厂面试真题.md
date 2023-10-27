SQL156 各个视频的平均完播率:

简单  通过率：16.95%  时间限制：1秒  空间限制：256M

描述

用户-视频互动表tb_user_video_log
|id|uid|video_id|start_time|end_time|if_follow|if_like|if_retweet|comment_id|
|---|---|---|---|---|---|---|---|---|
|1|101|2001|2021-10-01 10:00:00|2021-10-01 10:00:30|0|1|1|NULL|
|2|102|2001|2021-10-01 10:00:00|2021-10-01 10:00:24|0|0|1|NULL|
|3|103|2001|2021-10-01 11:00:00|2021-10-01 11:00:34|0|1|0|1732526|
|4|101|2002|2021-09-01 10:00:00|2021-9-01  10:00:42|1|0|1|NULL|
|5|102|2002|2021-10-01 11:00:00|2021-10-01  10:00:30|1|0|1|NULL|

（uid-用户ID, video_id-视频ID, start_time-开始观看时间, end_time-结束观看时间, if_follow-是否关注, if_like-是否点赞, if_retweet-是否转发, comment_id-评论ID）

短视频信息表tb_video_info
|id|video_id|author|tag|duration|release_time|
|---|---|---|---|---|---|
|1|2001|901|影视|30|2021-01-01 07:00:00|
|2|2002|901|美食|60|2021-01-01 07:00:00|
|3|2003|902|旅游|90|2021-01-01 07:00:00|

（video_id-视频ID, author-创作者ID, tag-类别标签, duration-视频时长（秒）, release_time-发布时间）

问题：计算2021年里有播放记录的每个视频的完播率(结果保留三位小数)，并按完播率降序排序<br/>
注：视频完播率是指完成播放次数占总播放次数的比例。简单起见，结束观看时间与开始播放时间的差>=视频时长时，视为完成播放。

输出示例：

示例数据的结果如下：
|video_id|avg_comp_play_rate|
|---|---|
|2001|0.667|
|2002|0.000|

解释：
视频2001在2021年10月有3次播放记录，观看时长分别为30秒、24秒、34秒，视频时长30秒，因此有两次是被认为完成播放了的，故完播率为0.667；<br/>
视频2002在2021年9月和10月共2次播放记录，观看时长分别为42秒、30秒，视频时长60秒，故完播率为0.000。

```sql
select a.video_id, 
round(sum(case when timestampdiff(second,start_time,end_time) >= duration then 1 else 0 end)/count(a.video_id),3) rate from tb_user_video_log a 
left join tb_video_info b on a.video_id = b.video_id
where year(start_time) = 2021
group by a.video_id
order by rate desc
```

SQL157 平均播放进度大于60%的视频类别:

简单  通过率：12.00%  时间限制：1秒  空间限制：256M

描述

用户-视频互动表tb_user_video_log
|id|uid|video_id|start_time|end_time|if_follow|if_like|if_retweet|comment_id|
|---|---|---|---|---|---|---|---|---|
|1|101|2001|2021-10-01 10:00:00|2021-10-01 10:00:30|0|1|1|NULL|
|2|102|2001|2021-10-01 10:00:00|2021-10-01 10:00:21|0|0|1|NULL|
|3|103|2001|2021-10-01 11:00:50|2021-10-01 11:01:20|0|1|0|1732526|
|4|102|2002|2021-10-01 11:00:00|2021-10-01 11:00:30|1|0|1|NULL|
|5|103|2002|2021-10-01 10:59:05|2021-10-01 11:00:05|1|0|1|NULL|

（uid-用户ID, video_id-视频ID, start_time-开始观看时间, end_time-结束观看时间, if_follow-是否关注, if_like-是否点赞, if_retweet-是否转发, comment_id-评论ID）

短视频信息表tb_video_info
|id|video_id|author|tag|duration|release_time|
|---|---|---|---|---|---|
|1|2001|901|影视|30|2021-01-01 07:00:00|
|2|2002|901|美食|60|2021-01-01 07:00:00|
|3|2003|902|旅游|90|2021-01-01 07:00:00|

（video_id-视频ID, author-创作者ID, tag-类别标签, duration-视频时长, release_time-发布时间）

问题：计算各类视频的平均播放进度，将进度大于60%的类别输出。

注：<br/>
播放进度=播放时长÷视频时长*100%，当播放时长大于视频时长时，播放进度均记为100%。<br/>
结果保留两位小数，并按播放进度倒序排序。

输出示例：

示例数据的输出结果如下：
|tag|avg_play_progress|
|---|---|
|影视|90.00%|
|美食|75.00%|

解释：<br/>
影视类视频2001被用户101、102、103看过，播放进度分别为：30秒（100%）、21秒（70%）、30秒（100%），平均播放进度为90.00%（保留两位小数）；<br/>
美食类视频2002被用户102、103看过，播放进度分别为：30秒（50%）、60秒（100%），平均播放进度为75.00%（保留两位小数）；

```sql
select tag, 
concat(format(avg(case when timestampdiff(second,start_time,end_time) > duration then 100.00 else timestampdiff(second,start_time,end_time)*100/duration end),2),'%') rate from tb_user_video_log a
left join tb_video_info b on
a.video_id = b.video_id
group by tag
having avg(case when timestampdiff(second,start_time,end_time) > duration then 100.00 else timestampdiff(second,start_time,end_time)*100/duration end) > 60
order by rate desc
```

SQL158 每类视频近一个月的转发量/率:

中等  通过率：17.17%  时间限制：1秒  空间限制：256M

描述

用户-视频互动表tb_user_video_log
|id|uid|video_id|start_time|end_time|if_follow|if_like|if_retweet|comment_id|
|---|---|---|---|---|---|---|---|---|
|1|101|2001|2021-10-01 10:00:00|2021-10-01 10:00:30|0|1|1|NULL|
|2|102|2001|2021-10-01 10:00:00|2021-10-01 10:00:21|0|0|1|NULL|
|3|103|2001|2021-10-01 11:00:50|2021-10-01 11:01:20|0|1|0|1732526|
|4|102|2002|2021-10-01 11:00:00|2021-10-01 11:00:30|1|0|1|NULL|
|5|103|2002|2021-10-01 10:59:05|2021-10-01 11:00:05|1|0|1|NULL|

（uid-用户ID, video_id-视频ID, start_time-开始观看时间, end_time-结束观看时间, if_follow-是否关注, if_like-是否点赞, if_retweet-是否转发, comment_id-评论ID）

短视频信息表tb_video_info
|id|video_id|author|tag|duration|release_time|
|---|---|---|---|---|---|
|1|2001|901|影视|30|2021-01-01 07:00:00|
|2|2002|901|美食|60|2021-01-01 07:00:00|
|3|2003|902|旅游|90|2021-01-01 07:00:00|

（video_id-视频ID, author-创作者ID, tag-类别标签, duration-视频时长, release_time-发布时间）

问题：统计在有用户互动的最近一个月（按包含当天在内的近30天算，比如10月31日的近30天为10.2~10.31之间的数据）中，每类视频的转发量和转发率（保留3位小数）。<br/>
注：转发率＝转发量÷播放量。结果按转发率降序排序。

输出示例：

示例数据的输出结果如下
|tag|retweet_cut|retweet_rate|
|---|---|---|
|影视|2|0.667|
|美食|1|0.500|

解释：<br/>
由表tb_user_video_log的数据可得，数据转储当天为2021年10月1日。近30天内，影视类视频2001共有3次播放记录，被转发2次，转发率为0.667；美食类视频2002共有2次播放记录，1次被转发，转发率为0.500。

```sql
select tag, sum(if_retweet), round(sum(if_retweet)/count(*),3) rate from 
(
    select *, max(start_time)over() storagetime from tb_user_video_log
)
a left join tb_video_info b on a.video_id = b.video_id
where date_sub(storagetime,interval 30 day) <= start_time
group by tag
order by rate desc
```

SQL159 每个创作者每月的涨粉率及截止当前的总粉丝量:

中等  通过率：15.32%  时间限制：1秒  空间限制：256M

描述

用户-视频互动表tb_user_video_log

|id|uid|video_id|start_time|end_time|if_follow|if_like|if_retweet|comment_id|
|---|---|---|---|---|---|---|---|---|
|1|101|2001|2021-09-01 10:00:00|2021-09-01 10:00:20|0|1|1|NULL|
|2|105|2002|2021-09-10 11:00:00|2021-09-10 11:00:30|1|0|1|NULL|
|3|101|2001|2021-10-01 10:00:00|2021-10-01 10:00:20|1|1|1|NULL|
|4|102|2001|2021-10-01 10:00:00|2021-10-01 10:00:15|0|0|1|NULL|
|5|103|2001|2021-10-01 11:00:50|2021-10-01 11:01:15|1|1|0|1732526|
|6|106|2002|2021-10-01 10:59:05|2021-10-01 11:00:05|2|0|0|NULL|

（uid-用户ID, video_id-视频ID, start_time-开始观看时间, end_time-结束观看时间, if_follow-是否关注, if_like-是否点赞, if_retweet-是否转发, comment_id-评论ID）

短视频信息表tb_video_info

|id|video_id|author|tag|duration|release_time|
|---|---|---|---|---|---|
|1|2001|901|影视|30|2021-01-01 07:00:00|
|2|2002|901|美食|60|2021-01-01 07:00:00|
|3|2003|902|旅游|90|2020-01-01 07:00:00|
|4|2004|902|美女|90|2020-01-01 08:00:00|

（video_id-视频ID, author-创作者ID, tag-类别标签, duration-视频时长, release_time-发布时间）

问题：计算2021年里每个创作者每月的涨粉率及截止当月的总粉丝量

注：<br/>
涨粉率=(加粉量 - 掉粉量) / 播放量。结果按创作者ID、总粉丝量升序排序。<br/>
if_follow-是否关注为1表示用户观看视频中关注了视频创作者，为0表示此次互动前后关注状态未发生变化，为2表示本次观看过程中取消了关注。

输出示例：

示例数据的输出结果如下
|author|month|fans_growth_rate|total_fans|
|---|---|---|---|
|901|2021-09|0.500|1|
|901|2021-10|0.250|2|

解释：<br/>
示例数据中表tb_user_video_log里只有视频2001和2002的播放记录，都来自创作者901，播放时间在2021年9月和10月；其中9月里加粉量为1，掉粉量为0，播放量为2，因此涨粉率为0.500（保留3位小数）；其中10月里加粉量为2，掉份量为1，播放量为4，因此涨粉率为0.250，截止当前总粉丝数为2。

```sql
select author, ym, rate, sum(new_fan)over(partition by author order by ym) tt from
(
    select author, date_format(start_time,'%Y-%m') ym,
    round(sum(case when if_follow = 0 then 0 when if_follow = 1 then 1 else -1 end)/count(*),3) rate, 
    sum(case when if_follow = 0 then 0 when if_follow = 1 then 1 else -1 end) new_fan
    from tb_user_video_log a
    left join tb_video_info b on a.video_id = b.video_id
    where year(start_time) = 2021
    group by author, ym
) temp
order by author, tt
```

SQL160 国庆期间每类视频点赞量和转发量:

较难  通过率：20.30%  时间限制：1秒  空间限制：256M

描述

用户-视频互动表tb_user_video_log
|id|uid|video_id|start_time|end_time|if_follow|if_like|if_retweet|comment_id|
|---|---|---|---|---|---|---|---|---|
|1|101|2001|2021-09-24 10:00:00|2021-09-24 10:00:20|1|1|0|NULL|
|2|105|2002|2021-09-25 11:00:00|2021-09-25 11:00:30|0|0|1|NULL|
|3|102|2002|2021-09-25 11:00:00|2021-09-25 11:00:30|1|1|1|NULL|
|4|101|2002|2021-09-26 11:00:00|2021-09-26 11:00:30|1|0|1|NULL|
|5|101|2002|2021-09-27 11:00:00|2021-09-27 11:00:30|1|1|0|NULL|
|6|102|2002|2021-09-28 11:00:00|2021-09-28 11:00:30|1|0|1|NULL|
|7|103|2002|2021-09-29 11:00:00|2021-10-02 11:00:30|1|0|1|NULL|
|8|102|2002|2021-09-30 11:00:00|2021-09-30 11:00:30|1|1|1|NULL|
|9|101|2001|2021-10-01 10:00:00|2021-10-01 10:00:20|1|1|0|NULL|
|10|102|2001|2021-10-01 10:00:00|2021-10-01 10:00:15|0|0|1|NULL|
|11|103|2001|2021-10-01 11:00:50|2021-10-01 11:01:15|1|1|0|1732526|
|12|106|2002|2021-10-02 10:59:05|2021-10-02 11:00:05|2|0|1|NULL|
|13|107|2002|2021-10-02 10:59:05|2021-10-02 11:00:05|1|0|1|NULL|
|14|108|2002|2021-10-02 10:59:05|2021-10-02 11:00:05|1|1|1|NULL|
|15|109|2002|2021-10-03 10:59:05|2021-10-03 11:00:05|0|1|0|NULL|

（uid-用户ID, video_id-视频ID, start_time-开始观看时间, end_time-结束观看时间, if_follow-是否关注, if_like-是否点赞, if_retweet-是否转发, comment_id-评论ID）

短视频信息表tb_video_info

|id|video_id|author|tag|duration|release_time|
|---|---|---|---|---|---|
|1|2001|901|旅游|30|2020-01-01 07:00:00|
|2|2002|901|旅游|60|2021-01-01 07:00:00|
|3|2003|902|影视|90|2020-01-01 07:00:00|
|4|2004|902|美女|90|2020-01-01 08:00:00|

（video_id-视频ID, author-创作者ID, tag-类别标签, duration-视频时长, release_time-发布时间）

问题：统计2021年国庆头3天每类视频每天的近一周总点赞量和一周内最大单天转发量，结果按视频类别降序、日期升序排序。假设数据库中数据足够多，至少每个类别下国庆头3天及之前一周的每天都有播放记录。

输出示例：

示例数据的输出结果如下
|tag|dt|sum_like_cnt_7d|max_retweet_cnt_7d|
|---|---|---|---|
|旅游|2021-10-01|5|2|
|旅游|2021-10-02|5|3|
|旅游|2021-10-03|6|3|

解释：<br/>
由表tb_user_video_log里的数据可得只有旅游类视频的播放，2021年9月25到10月3日每天的点赞量和转发量如下：
|tag|dt|like_cnt|retweet_cnt|
|---|---|---|---|
|旅游|2021-09-25|1|2|
|旅游|2021-09-26|0|1|
|旅游|2021-09-27|1|0|
|旅游|2021-09-28|0|1|
|旅游|2021-09-29|0|1|
|旅游|2021-09-30|1|1|
|旅游|2021-10-01|2|1|
|旅游|2021-10-02|1|3|
|旅游|2021-10-03|1|0|

因此国庆头3天（10.01~10.03）里10.01的近7天（9.25~10.01）总点赞量为5次，单天最大转发量为2次（9月25那天最大）；同理可得10.02和10.03的两个指标。

```sql
select tempa.tag, tempa.ymd, sum(if_like), max(if_retweet) from
(
    select distinct tag, date_format(start_time,"%Y-%m-%d") ymd from tb_user_video_log a
    left join tb_video_info b on a.video_id = b.video_id
    where date_sub(start_time,interval 3 day) <= "2021-10-01" and start_time >= "2021-10-01"
) tempa
left join  
(
    select tag, date_format(start_time,"%Y-%m-%d") ymd, sum(if_like) if_like, sum(if_retweet) if_retweet from tb_user_video_log a
    left join tb_video_info b on a.video_id = b.video_id
    group by tag, ymd
)
tempb on tempa.tag = tempb.tag and date_sub(tempa.ymd,interval 6 day) <= tempb.ymd and tempa.ymd >= tempb.ymd
group by tempa.tag, tempa.ymd
order by tempa.tag desc, tempa.ymd
```

SQL161 一个月发布的视频中热度最高的top3视频:

困难  通过率：11.81%  时间限制：1秒  空间限制：256M

描述

现有用户-视频互动表tb_user_video_log
|id|uid|video_id|start_time|end_time|if_follow|if_like|if_retweet|comment_id|
|---|---|---|---|---|---|---|---|---|
|1|101|2001|2021-09-24 10:00:00|2021-09-24 10:00:30|1|1|1|NULL|
|2|101|2001|2021-10-01 10:00:00|2021-10-01 10:00:31|1|1|0|NULL|
|3|102|2001|2021-10-01 10:00:00|2021-10-01 10:00:35|0|0|1|NULL|
|4|103|2001|2021-10-03 11:00:50|2021-10-03 10:00:35|1|1|0|1732526|
|5|106|2002|2021-10-02 11:00:05|2021-10-02 11:01:04|2|0|1|NULL|
|6|107|2002|2021-10-02 10:59:05|2021-10-02 11:00:06|1|0|0|NULL|
|7|108|2002|2021-10-02 10:59:05|2021-10-02 11:00:05|1|1|1|NULL|
|8|109|2002|2021-10-03 10:59:05|2021-10-03 11:00:01|0|1|0|NULL|
|9|105|2002|2021-09-25 11:00:00|2021-09-25 11:00:30|1|0|1|NULL|
|10|101|2003|2021-09-26 11:00:00|2021-09-26 11:00:30|1|0|0|NULL|
|11|101|2003|2021-09-30 11:00:00|2021-09-30 11:00:30|1|1|0|NULL|

（uid-用户ID, video_id-视频ID, start_time-开始观看时间, end_time-结束观看时间, if_follow-是否关注, if_like-是否点赞, if_retweet-是否转发, comment_id-评论ID）

短视频信息表tb_video_info

|id|video_id|author|tag|duration|release_time|
|---|---|---|---|---|---|
|1|2001|901|旅游|30|2021-09-05 07:00:00|
|2|2002|901|旅游|60|2021-09-05 07:00:00|
|3|2003|902|影视|90|2021-09-05 07:00:00|
|4|2004|902|影视|90|2021-09-05 08:00:00|

（video_id-视频ID, author-创作者ID, tag-类别标签, duration-视频时长, release_time-发布时间）

问题：找出近一个月发布的视频中热度最高的top3视频。

注：<br/>
热度=(a*视频完播率+b*点赞数+c*评论数+d*转发数)*新鲜度；<br/>
新鲜度=1/(最近无播放天数+1)；<br/>
当前配置的参数a,b,c,d分别为100、5、3、2。<br/>
最近播放日期以end_time-结束观看时间为准，假设为T，则最近一个月按[T-29, T]闭区间统计。<br/>
结果中热度保留为整数，并按热度降序排序。<br/>

输出示例：

示例数据的输出结果如下
|video_id|hot_index|
|---|---|
|2001|122|
|2002|56|
|2003|1|

解释：<br/>
最近播放日期为2021-10-03，记作当天日期；近一个月（2021-09-04及之后）发布的视频有2001、2002、2003、2004，不过2004暂时还没有播放记录；<br/>
视频2001完播率1.0（被播放次数4次，完成播放4次），被点赞3次，评论1次，转发2次，最近无播放天数为0，因此热度为：(100*1.0+5*3+3*1+2*2)/(0+1)=122<br/>
同理，视频2003完播率0，被点赞数1，评论和转发均为0，最近无播放天数为3，因此热度为：(100*0+5*1+3*0+2*0)/(3+1)=1（1.2保留为整数）。

```sql
select a.video_id, 
round((sum(case when timestampdiff(second,start_time,end_time) < duration then 0 else 1 end)/count(a.video_id)*100 + sum(if_like)*5 + count(comment_id)*3 +sum(if_retweet)*2)/(datediff(max(max(end_time))over(),max(end_time))+1),0) hot_index
from tb_user_video_log a
left join tb_video_info on a.video_id = tb_video_info.video_id
where a.video_id in(select video_id from tb_video_info where date_sub((select max(end_time) from tb_user_video_log),interval 29 day)<=release_time)
group by a.video_id
order by hot_index desc
limit 3
```

SQL162 2021年11月每天的人均浏览文章时长:

简单  通过率：16.85%  时间限制：1秒  空间限制：256M

描述

用户行为日志表tb_user_log
|id|uid|artical_id|in_time|out_time|sign_cin|
|---|---|---|---|---|---|
|1|101|9001|2021-11-01 10:00:00|2021-11-01 10:00:31|0|
|2|102|9001|2021-11-01 10:00:00|2021-11-01 10:00:24|0|
|3|102|9002|2021-11-01 11:00:00|2021-11-01 11:00:11|0|
|4|101|9001|2021-11-02 10:00:00|2021-11-02 10:00:50|0|
|5|102|9002|2021-11-02 11:00:01|2021-11-02 11:00:24|0|

（uid-用户ID, artical_id-文章ID, in_time-进入时间, out_time-离开时间, sign_in-是否签到）

场景逻辑说明：artical_id-文章ID代表用户浏览的文章的ID，artical_id-文章ID为0表示用户在非文章内容页（比如App内的列表页、活动页等）。

问题：统计2021年11月每天的人均浏览文章时长（秒数），结果保留1位小数，并按时长由短到长排序。

输出示例：

示例数据的输出结果如下
|dt|avg_viiew_len_sec|
|---|---|
|2021-11-01|33.0|
|2021-11-02|36.5|

解释：<br/>
11月1日有2个人浏览文章，总共浏览时长为31+24+11=66秒，人均浏览33秒；<br/>
11月2日有2个人浏览文章，总共时长为50+23=73秒，人均时长为36.5秒。

```sql
select date_format(in_time, "%Y-%m-%d") ymd,
round(sum(timestampdiff(second,in_time,out_time))/count(distinct uid),1) avg_sec
from tb_user_log
where year(in_time) = 2021 and month(in_time) = 11 and artical_id != 0
group by ymd
order by avg_sec
```

SQL163 每篇文章同一时刻最大在看人数:

中等  通过率：17.97%  时间限制：1秒  空间限制：256M

描述

用户行为日志表tb_user_log
|id|uid|artical_id|in_time|out_time|sign_cin|
|---|---|---|---|---|---|
|1|101|9001|2021-11-01 10:00:00|2021-11-01 10:00:11|0|
|2|102|9001|2021-11-01 10:00:09|2021-11-01 10:00:38|0|
|3|103|9001|2021-11-01 10:00:28|2021-11-01 10:00:58|0|
|4|104|9002|2021-11-01 11:00:45|2021-11-01 11:01:11|0|
|5|105|9001|2021-11-01 10:00:51|2021-11-01 10:00:59|0|
|6|106|9002|2021-11-01 11:00:55|2021-11-01 11:01:24|0|
|7|107|9001|2021-11-01 10:00:01|2021-11-01 10:01:50|0|

（uid-用户ID, artical_id-文章ID, in_time-进入时间, out_time-离开时间, sign_in-是否签到）

场景逻辑说明：artical_id-文章ID代表用户浏览的文章的ID，artical_id-文章ID为0表示用户在非文章内容页（比如App内的列表页、活动页等）。

问题：统计每篇文章同一时刻最大在看人数，如果同一时刻有进入也有离开时，先记录用户数增加再记录减少，结果按最大人数降序。

输出示例：

示例数据的输出结果如下
|artical_id|max_uv|
|---|---|
|9001|3|
|9002|2|

解释：10点0分10秒时，有3个用户正在浏览文章9001；11点01分0秒时，有2个用户正在浏览文章9002。

```sql
select artical_id, max(res) max_uv from
(
    select artical_id, timestampmark, 
    sum(res)over(partition by artical_id order by timestampmark,res desc) res
    from
    (
        select artical_id, in_time timestampmark, 1 res from tb_user_log
        where artical_id != 0
        union all
        select artical_id, out_time, -1 from tb_user_log
        where artical_id != 0
    ) test 
) a group by artical_id
order by max_uv desc
```

SQL164 2021年11月每天新用户的次日留存率:

中等  通过率：14.47%  时间限制：1秒  空间限制：256M

描述

用户行为日志表tb_user_log
|id|uid|artical_id|in_time|out_time|sign_cin|
|---|---|---|---|---|---|
|1|101|0|2021-11-01 10:00:00|2021-11-01 10:00:42|1|
|2|102|9001|2021-11-01 10:00:00|2021-11-01 10:00:09|0|
|3|103|9001|2021-11-01 10:00:01|2021-11-01 10:01:50|0|
|4|101|9002|2021-11-02 10:00:09|2021-11-02 10:00:28|0|
|5|103|9002|2021-11-02 10:00:51|2021-11-02 10:00:59|0|
|6|104|9001|2021-11-02 11:00:28|2021-11-02 11:01:24|0|
|7|101|9003|2021-11-03 11:00:55|2021-11-03 11:01:24|0|
|8|104|9003|2021-11-03 11:00:45|2021-11-03 11:00:55|0|
|9|105|9003|2021-11-03 11:00:53|2021-11-03 11:00:59|0|
|10|101|9002|2021-11-04 11:00:55|2021-11-04 11:00:59|0|

（uid-用户ID, artical_id-文章ID, in_time-进入时间, out_time-离开时间, sign_in-是否签到）

问题：统计2021年11月每天新用户的次日留存率（保留2位小数）

注：<br/>
次日留存率为当天新增的用户数中第二天又活跃了的用户数占比。
如果in_time-进入时间和out_time-离开时间跨天了，在两天里都记为该用户活跃过，结果按日期升序。

输出示例：
示例数据的输出结果如下
|dt|uv_left_rate|
|---|---|
|2021-11-01|0.67|
|2021-11-02|1.00|
|2021-11-03|0.00|

解释：<br/>
11.01有3个用户活跃101、102、103，均为新用户，在11.02只有101、103两个又活跃了，因此11.01的次日留存率为0.67；<br/>
11.02有104一位新用户，在11.03又活跃了，因此11.02的次日留存率为1.00；<br/>
11.03有105一位新用户，在11.04未活跃，因此11.03的次日留存率为0.00；<br/>
11.04没有新用户，不输出。

```sql
select ymd, round(count(distinct b.uid)/count(distinct a.uid),2) from
(
    select uid, min(date_format(in_time,"%Y-%m-%d")) ymd from tb_user_log
    where in_time > '2021-10-30' and out_time < '2021-12-02'
    group by uid
) a left join tb_user_log b on a.uid = b.uid and (datediff(ymd,in_time) = -1 or datediff(ymd,out_time) = -1)
where ymd >= '2021-11-01' and ymd < '2021-12-01'
group by ymd
having count(distinct a.uid) > 0
```

SQL165 统计活跃间隔对用户分级结果:

较难  通过率：17.09%  时间限制：1秒  空间限制：256M

描述

用户行为日志表tb_user_log
|id|uid|artical_id|in_time|out_time|sign_cin|
|---|---|---|---|---|---|
|1|109|9001|2021-08-31 10:00:00|2021-08-31 10:00:09|0|
|2|109|9002|2021-11-04 11:00:55|2021-11-04 11:00:59|0|
|3|108|9001|2021-09-01 10:00:01|2021-09-01 10:01:50|0|
|4|108|9001|2021-11-03 10:00:01|2021-11-03 10:01:50|0|
|5|104|9001|2021-11-02 10:00:28|2021-11-02 10:00:50|0|
|6|104|9003|2021-09-03 11:00:45|2021-09-03 11:00:55|0|
|7|105|9003|2021-11-03 11:00:53|2021-11-03 11:00:59|0|
|8|102|9001|2021-10-30 10:00:00|2021-10-30 10:00:09|0|
|9|103|9001|2021-10-21 10:00:00|2021-10-21 10:00:09|0|
|10|101|0|2021-10-01 10:00:00|2021-10-01 10:00:42|1|

（uid-用户ID, artical_id-文章ID, in_time-进入时间, out_time-离开时间, sign_in-是否签到）

问题：统计活跃间隔对用户分级后，各活跃等级用户占比，结果保留两位小数，且按占比降序排序。

注：<br/>
用户等级标准简化为：忠实用户(近7天活跃过且非新晋用户)、新晋用户(近7天新增)、沉睡用户(近7天未活跃但更早前活跃过)、流失用户(近30天未活跃但更早前活跃过)。<br/>
假设今天就是数据中所有日期的最大值。<br/>
近7天表示包含当天T的近7天，即闭区间[T-6, T]。

输出示例：

示例数据的输出结果如下
|user_grade|ratio|
|---|---|
|忠实用户|0.43|
|新晋用户|0.29|
|沉睡用户|0.14|
|流失用户|0.14|

解释：<br/>
今天日期为2021.11.04，根据用户分级标准，用户行为日志表tb_user_log中忠实用户有：109、108、104；新晋用户有105、102；沉睡用户有103；流失用户有101；共7个用户，因此他们的比例分别为0.43、0.29、0.14、0.14。

```sql
select category, round(count(uid)/sum(count(*))over(),2) rate from
(
    select uid,  
    case when datediff(today, sign_time) < 7 then '新晋用户' 
    when datediff(today, active_time) < 7 and datediff(active_time, sign_time) > 0 then '忠实用户'
    when datediff(today, active_time) > 29 then '流失用户'
    else '沉睡用户' end category
    from
    (
        select uid, min(in_time) sign_time, max(out_time) active_time, max(max(out_time))over() today
        from tb_user_log
        group by uid
    ) temp
) test group by category
order by rate desc
```

SQL166 每天的日活数及新用户占比:

较难  通过率：21.90%  时间限制：1秒  空间限制：256M

描述

用户行为日志表tb_user_log
|id|uid|artical_id|in_time|out_time|sign_cin|
|---|---|---|---|---|---|
|1|101|9001|2021-10-31 10:00:00|2021-10-31 10:00:09|0|
|2|102|9001|2021-10-31 10:00:00|2021-10-31 10:00:09|0|
|3|101|0|2021-11-01 10:00:00|2021-11-01 10:00:42|1|
|4|102|9001|2021-11-01 10:00:00|2021-11-01 10:00:09|0|
|5|108|9001|2021-11-01 10:00:01|2021-11-01 10:00:50|0|
|6|108|9001|2021-11-02 10:00:01|2021-11-02 10:00:50|0|
|7|104|9001|2021-11-02 10:00:28|2021-11-02 10:00:50|0|
|8|106|9001|2021-11-02 10:00:28|2021-11-02 10:00:50|0|
|9|108|9001|2021-11-03 10:00:01|2021-11-03 10:00:50|0|
|10|109|9002|2021-11-03 11:00:55|2021-11-03 11:00:59|0|
|11|104|9003|2021-11-03 11:00:45|2021-11-03 11:00:55|0|
|12|105|9003|2021-11-03 11:00:53|2021-11-03 11:00:59|0|
|13|106|9003|2021-11-03 11:00:45|2021-11-03 11:00:55|0|

（uid-用户ID, artical_id-文章ID, in_time-进入时间, out_time-离开时间, sign_in-是否签到）

问题：统计每天的日活数及新用户占比

注：<br/>
新用户占比=当天的新用户数÷当天活跃用户数（日活数）。<br/>
如果in_time-进入时间和out_time-离开时间跨天了，在两天里都记为该用户活跃过。<br/>
新用户占比保留2位小数，结果按日期升序排序。

输出示例：

示例数据的输出结果如下
|dt|dau|uv_new_ratio|
|---|---|---|
|2021-10-30|2|1.00|
|2021-11-01|3|0.33|
|2021-11-02|3|0.67|
|2021-11-03|5|0.40|

解释：<br/>
2021年10月31日有2个用户活跃，都为新用户，新用户占比1.00；<br/>
2021年11月1日有3个用户活跃，其中1个新用户，新用户占比0.33；

```sql
select b.ymd, dau, ifnull(round(newu/dau,2),0.00) from
(
    select ymd, count(distinct uid) dau from
    (
        select date_format(in_time,"%Y-%m-%d") ymd, uid from tb_user_log
        union
        select date_format(out_time,"%Y-%m-%d") ymd, uid from tb_user_log
    ) a group by ymd
) b left join
(
    select ymd, count(distinct uid) newu from
    (
        select uid, min(date_format(in_time,"%Y-%m-%d")) ymd from tb_user_log group by uid
    ) c group by ymd
) d on b.ymd = d.ymd
order by b.ymd
```

SQL167 连续签到领金币:

困难  通过率：14.76%  时间限制：1秒  空间限制：256M

描述

用户行为日志表tb_user_log
|id|uid|artical_id|in_time|out_time|sign_in|
|---|---|---|---|---|---|
|1|101|0|2021-07-07 10:00:00|2021-07-07 10:00:09|1|
|2|101|0|2021-07-08 10:00:00|2021-07-08 10:00:09|1|
|3|101|0|2021-07-09 10:00:00|2021-07-09 10:00:42|1|
|4|101|0|2021-07-10 10:00:00|2021-07-10 10:00:09|1|
|5|101|0|2021-07-11 23:59:55|2021-07-11 23:59:59|1|
|6|101|0|2021-07-12 10:00:28|2021-07-12 10:00:50|1|
|7|101|0|2021-07-13 10:00:28|2021-07-13 10:00:50|1|
|8|102|0|2021-10-01 10:00:28|2021-10-01 10:00:50|1|
|9|102|0|2021-10-02 10:00:01|2021-10-02 10:01:50|1|
|10|102|0|2021-10-03 10:00:55|2021-10-03 11:00:59|1|
|11|102|0|2021-10-04 10:00:45|2021-10-04 11:00:55|0|
|12|102|0|2021-10-05 10:00:53|2021-10-05 11:00:59|1|
|13|102|0|2021-10-06 10:00:45|2021-10-06 11:00:55|1|

（uid-用户ID, artical_id-文章ID, in_time-进入时间, out_time-离开时间, sign_in-是否签到）

场景逻辑说明：<br/>
artical_id-文章ID代表用户浏览的文章的ID，特殊情况artical_id-文章ID为0表示用户在非文章内容页（比如App内的列表页、活动页等）。注意：只有artical_id为0时sign_in值才有效。<br/>
从2021年7月7日0点开始，用户每天签到可以领1金币，并可以开始累积签到天数，连续签到的第3、7天分别可额外领2、6金币。<br/>
每连续签到7天后重新累积签到天数（即重置签到天数：连续第8天签到时记为新的一轮签到的第一天，领1金币）<br/>
问题：计算每个用户2021年7月以来每月获得的金币数（该活动到10月底结束，11月1日开始的签到不再获得金币）。结果按月份、ID升序排序。

注：如果签到记录的in_time-进入时间和out_time-离开时间跨天了，也只记作in_time对应的日期签到了。

输出示例：

示例数据的输出结果如下：
|uid|month|coin|
|---|---|---|
|101|202107|15|
|102|202110|7|

解释：<br/>
101在活动期内连续签到了7天，因此获得1*7+2+6=15金币；<br/>
102在10.01\~10.03连续签到3天获得5金币<br/>
10.04断签了，10.05\~10.06连续签到2天获得2金币，共得到7金币。

```sql
with temp as
(
    select uid, ymd, ymd - row_number()over(partition by uid order by ymd) as class from 
    (
        select uid, date_format(in_time,"%Y%m%d") ymd from tb_user_log
        where in_time >= '2021-07-07' and in_time < '2021-11-01' and artical_id = 0 and sign_in = 1
    ) test
)
select uid, ym, 
sum(case when mod(rn,7) = 3 then 3 when mod(rn,7) = 0 then 7 else 1 end)
from
(
    select uid, date_format(ymd,"%Y%m") ym, row_number()over(partition by uid,class order by ymd) rn from temp
) a
group by uid, ym
order by ym, uid
```

SQL168 计算商城中2021年每月的GMV:

简单  通过率：19.71%  时间限制：1秒  空间限制：256M

描述

现有订单总表tb_order_overall
|id|order_id|uid|event_time|total_amount|total_cnt|status|
|---|---|---|---|---|---|---|
|1|301001|101|2021-10-01 10:00:00|15900|2|1|
|2|301002|101|2021-10-01 11:00:00|15900|2|1|
|3|301003|102|2021-10-02 10:00:00|34500|8|0|
|4|301004|103|2021-10-12 10:00:00|43500|9|1|
|5|301005|105|2021-11-01 10:00:00|31900|7|1|
|6|301006|102|2021-11-02 10:00:00|24500|6|1|
|7|301007|102|2021-11-03 10:00:00|-24500|6|2|
|8|301008|104|2021-11-04 10:00:00|55500|12|0|

（order_id-订单号, uid-用户ID, event_time-下单时间, total_amount-订单总金额, total_cnt-订单商品总件数, status-订单状态）

场景逻辑说明：<br/>
用户将购物车中多件商品一起下单时，订单总表会生成一个订单（但此时未付款，status-订单状态为0，表示待付款）；<br/>
当用户支付完成时，在订单总表修改对应订单记录的status-订单状态为1，表示已付款；<br/>
若用户退货退款，在订单总表生成一条交易总金额为负值的记录（表示退款金额，订单号为退款单号，status-订单状态为2表示已退款）。

问题：请计算商城中2021年每月的GMV，输出GMV大于10w的每月GMV，值保留到整数。

注：GMV为已付款订单和未付款订单两者之和。结果按GMV升序排序。

输出示例：

示例数据输出如下：
|month|GMV|
|---|---|
|2021-10|109800|
|2021-11|111900|

解释：<br/>
2021年10月有3笔已付款的订单，1笔未付款订单，总交易金额为109800；2021年11月有2笔已付款订单，1笔未付款订单，<br/>
总交易金额为111900（还有1笔退款订单由于已计算了付款的订单金额，无需计算在GMV中）。

```sql
select date_format(event_time,"%Y-%m") ym, sum(total_amount) GMV from tb_order_overall
where status != 2 and year(event_time) = 2021
group by ym
having GMV > 100000
order by GMV
```

SQL169 统计2021年10月每个退货率不大于0.5的商品各项指标:

中等  通过率：25.96%  时间限制：1秒  空间限制：256M

描述

现有用户对展示的商品行为表tb_user_event
|id|uid|product_id|event_time|if_click|if_cart|if_payment|if_refund|
|---|---|---|---|---|---|---|---|
|1|101|8001|2021-10-01 10:00:00|0|0|0|0|
|2|102|8001|2021-10-01 10:00:00|1|0|0|0|
|3|103|8001|2021-10-01 10:00:00|1|1|0|0|
|4|104|8001|2021-10-02 10:00:00|1|1|1|0|
|5|105|8001|2021-10-02 10:00:00|1|1|1|0|
|6|101|8002|2021-10-03 10:00:00|1|1|1|0|
|7|109|8001|2021-10-04 10:00:00|1|1|1|1|

（uid-用户ID, product_id-商品ID, event_time-行为时间, if_click-是否点击, if_cart-是否加购物车, if_payment-是否付款, if_refund-是否退货退款）

问题：请统计2021年10月每个有展示记录的退货率不大于0.5的商品各项指标，

注：<br/>
商品点展比=点击数÷展示数；<br/>
加购率=加购数÷点击数；<br/>
成单率=付款数÷加购数；退货率=退款数÷付款数，<br/>
当分母为0时整体结果记为0，结果中各项指标保留3位小数，并按商品ID升序排序。

输出示例：

示例数据的输出结果如下
|product_id|ctr|cart_rate|payment_rate|refund_rate\
|---|---|---|---|---|
|8001|0.833|0.800|0.750|0.333|
|8002|1.000|1.000|1.000|0.000|

解释：<br/>
在2021年10月商品8001被展示了6次，点击了5次，加购了4次，付款了3次，退款了1次，因此点击率为5/6=0.833，加购率为4/5=0.800，<br/>
成单率为3/4=0.750，退货率为1/3=0.333（保留3位小数）；

```sql
select product_id, 
round(ifnull(sum(if_click)/count(product_id),0),3) ctr,
round(ifnull(sum(if_cart)/sum(if_click),0) ,3) cart_rate,
round(ifnull(sum(if_payment)/sum(if_cart),0),3) payment_rate,
round(ifnull(sum(if_refund)/sum(if_payment),0),3) refund_rate
from tb_user_event
where year(event_time) = 2021 and month(event_time) = 10
group by product_id
having refund_rate <= 0.5
order by product_id
```

SQL170 某店铺的各商品毛利率及店铺整体毛利率:

中等  通过率：11.52%  时间限制：1秒  空间限制：256M

描述

商品信息表tb_product_info
|id|product_id|shop_id|tag|in_price|quantity|release_time|
|---|---|---|---|---|---|---|
|1|8001|901|家电|6000|100|2020-01-01 10:00:00|
|2|8002|902|家电|12000|50|2020-01-01 10:00:00|
|3|8003|901|3C数码|12000|50|2020-01-01 10:00:00|

（product_id-商品ID, shop_id-店铺ID, tag-商品类别标签, in_price-进货价格, quantity-进货数量, release_time-上架时间）

订单总表tb_order_overall
|id|order_id|uid|event_time|total_amount|total_cnt|status|
|---|---|---|---|---|---|---|
|1|301001|101|2021-10-01 10:00:00|30000|3|1|
|2|301002|102|2021-10-01 11:00:00|23900|2|1|
|3|301003|103|2021-10-02 10:00:00|31000|2|1|

（order_id-订单号, uid-用户ID, event_time-下单时间, total_amount-订单总金额, total_cnt-订单商品总件数, status-订单状态）

订单明细表tb_order_detail
|id|order_id|product_id|price|cnt|
|---|---|---|---|---|
|1|301001|8001|8500|2|
|2|301001|8002|15000|1|
|3|301002|8001|8500|1|
|4|301002|8002|16000|1|
|5|301003|8002|14000|1|
|6|301003|8003|18000|1|

（order_id-订单号, product_id-商品ID, price-商品单价, cnt-下单数量）

场景逻辑说明：<br/>
用户将购物车中多件商品一起下单时，订单总表会生成一个订单（但此时未付款，status-订单状态为0表示待付款），在订单明细表生成该订单中每个商品的信息；<br/>
当用户支付完成时，在订单总表修改对应订单记录的status-订单状态为1表示已付款；<br/>
若用户退货退款，在订单总表生成一条交易总金额为负值的记录（表示退款金额，订单号为退款单号，status-订单状态为2表示已退款）。

问题：请计算2021年10月以来店铺901中商品毛利率大于24.9%的商品信息及店铺整体毛利率。

注：商品毛利率=(1-进价/平均单件售价)*100%；<br/>
店铺毛利率=(1-总进价成本/总销售收入)*100%。<br/>
结果先输出店铺毛利率，再按商品ID升序输出各商品毛利率，均保留1位小数。

输出示例：

示例数据的输出结果如下：
|product_id|profit_rate|
|---|---|
|店铺汇总|31.0%|
|8001|29.4%|
|8003|33.3%|

解释：<br/>
店铺901有两件商品8001和8003；8001售出了3件，销售总额为25500，进价总额为18000，毛利率为1-18000/25500=29.4%，8003售出了1件，售价为18000，进价为12000，毛利率为33.3%；<br/>
店铺卖出的这4件商品总销售额为43500，总进价为30000，毛利率为1-30000/43500=31.0%

```sql
select "店铺汇总" as Total, concat(round((1-sum(in_price*cnt)/sum(price*cnt))*100,1),"%") from tb_order_overall a
left join tb_order_detail b on a.order_id = b.order_id
left join tb_product_info c on b.product_id = c.product_id
where event_time >= '2021-10-01 'and shop_id = 901 and status != 0
union all
(
    select b.product_id, concat(round((1-sum(in_price*cnt)/sum(price*cnt))*100,1),"%") rate from tb_order_overall a
    left join tb_order_detail b on a.order_id = b.order_id
    left join tb_product_info c on b.product_id = c.product_id
    where event_time >= '2021-10-01' and shop_id = 901 and status != 0
    group by b.product_id
    having 1-sum(in_price*cnt)/sum(price*cnt) > 0.249
    order by rate
)
```

SQL171 零食类商品中复购率top3高的商品:

中等  通过率：11.45%  时间限制：1秒  空间限制：256M

描述

商品信息表tb_product_info
|id|product_id|shop_id|tag|in_price|quantity|release_time|
|---|---|---|---|---|---|---|
|1|8001|901|零食|60|1000|2020-01-01 10:00:00|
|2|8002|901|零食|140|500|2020-01-01 10:00:00|
|3|8003|901|零食|160|500|2020-01-01 10:00:00|

（product_id-商品ID, shop_id-店铺ID, tag-商品类别标签, in_price-进货价格, quantity-进货数量, release_time-上架时间）

订单总表tb_order_overall
|id|order_id|uid|event_time|total_amount|total_cnt|status|
|---|---|---|---|---|---|---|
|1|301001|101|2021-09-30 10:00:00|140|1|1|
|2|301002|102|2021-10-01 11:00:00|235|2|1|
|3|301011|102|2021-10-31 11:00:00|250|2|1|
|4|301003|101|2021-10-02 10:00:00|300|2|1|
|5|301013|105|2021-10-02 10:00:00|300|2|1|
|6|301005|104|2021-10-03 10:00:00|170|1|1|

（order_id-订单号, uid-用户ID, event_time-下单时间, total_amount-订单总金额, total_cnt-订单商品总件数, status-订单状态）

订单明细表tb_order_detail
|id|order_id|product_id|price|cnt|
|---|---|---|---|---|
|1|301001|8002|150|1|
|2|301011|8003|200|1|
|3|301011|8001|80|1|
|4|301002|8001|85|1|
|5|301002|8003|180|1|
|6|301003|8002|140|1|
|7|301003|8003|180|1|
|8|301013|8002|140|2|
|9|301005|8003|180|1|

（order_id-订单号, product_id-商品ID, price-商品单价, cnt-下单数量）

场景逻辑说明：<br/>
用户将购物车中多件商品一起下单时，订单总表会生成一个订单（但此时未付款， status-订单状态-订单状态为0表示待付款），在订单明细表生成该订单中每个商品的信息；<br/>
当用户支付完成时，在订单总表修改对应订单记录的status-订单状态-订单状态为1表示已付款；<br/>
若用户退货退款，在订单总表生成一条交易总金额为负值的记录（表示退款金额，订单号为退款单号，订单状态为2表示已退款）。

问题：请统计零食类商品中复购率top3高的商品。

注：复购率指用户在一段时间内对某商品的重复购买比例，复购率越大，则反映出消费者对品牌的忠诚度就越高，也叫回头率<br/>
此处我们定义：某商品复购率 = 近90天内购买它至少两次的人数 ÷ 购买它的总人数<br/>
近90天指包含最大日期（记为当天）在内的近90天。结果中复购率保留3位小数，并按复购率倒序、商品ID升序排序

输出示例：

示例数据的输出结果如下：
|product_id|repurchase_rate|
|---|---|
|8001|1.000|
|8002|0.500|
|8003|0.333|

解释：<br/>
商品8001、8002、8003都是零食类商品，8001只被用户102购买了两次，复购率1.000；<br/>
商品8002被101购买了两次，被105购买了1次，复购率0.500；<br/>
商品8003被102购买两次，被101和105各购买1次，复购率为0.333。

```sql
select product_id, round(sum(case when cnt > 1 then 1 else 0 end)/count(uid),3) rate from
(
    select b.product_id, uid, count(*) cnt from
    (select *,max(event_time)over() maxdate from tb_order_overall) a
    left join tb_order_detail b on a.order_id = b.order_id
    left join tb_product_info c on b.product_id = c.product_id
    where date_add(event_time,interval 89 day) >= maxdate and tag = "零食"
    group by product_id, uid
) test
group by product_id
order by rate desc, product_id
limit 3
```

SQL172 10月的新户客单价和获客成本:

较难  通过率：12.77%  时间限制：1秒  空间限制：256M
描述

商品信息表tb_product_info
|id|product_id|shop_id|tag|in_price|quantity|release_time|
|---|---|---|---|---|---|---|
|1|8001|901|日用|60|1000|2020-01-01 10:00:00|
|2|8002|901|零食|140|500|2020-01-01 10:00:00|
|3|8003|901|零食|160|500|2020-01-01 10:00:00|
|4|8004|902|零食|130|500|2020-01-01 10:00:00|

（product_id-商品ID, shop_id-店铺ID, tag-商品类别标签, in_price-进货价格, quantity-进货数量, release_time-上架时间）

订单总表tb_order_overall
|id|order_id|uid|event_time|total_amount|total_cnt|status|
|---|---|---|---|---|---|---|
|1|301002|102|2021-10-01 11:00:00|235|2|1|
|2|301003|101|2021-10-02 10:00:00|300|2|1|
|3|301005|104|2021-10-03 10:00:00|160|1|1|

（order_id-订单号, uid-用户ID, event_time-下单时间, total_amount-订单总金额, total_cnt-订单商品总件数, status-订单状态）

订单明细表tb_order_detail
|id|order_id|product_id|price|cnt|
|---|---|---|---|---|
|1|301002|8001|85|1|
|2|301002|8003|180|1|
|3|301003|8004|140|1|
|4|301003|8003|180|1|
|5|301005|8003|180|1|

（order_id-订单号, product_id-商品ID, price-商品单价, cnt-下单数量）

问题：请计算2021年10月商城里所有新用户的首单平均交易金额（客单价）和平均获客成本（保留一位小数）。

注：订单的优惠金额 = 订单明细里的{该订单各商品单价×数量之和} - 订单总表里的{订单总金额} 。

输出示例：

示例数据的输出结果如下
|avg_amount|avg_cost|
|---|---|
|231.7|23.3|

解释：<br/>
2021年10月有3个新用户，102的首单为301002，订单金额为235，商品总金额为85+180=265，优惠金额为30；<br/>
101的首单为301003，订单金额为300，商品总金额为140+180=320，优惠金额为20；<br/>
104的首单为301005，订单金额为160，商品总金额为180，优惠金额为20；<br/>
平均首单客单价为(235+300+160)/3=231.7，平均获客成本为(30+20+20)/3=23.3

```sql
select round(avg(total_amount),1), round(avg(total-total_amount),1)  from
(
    select uid, order_id, event_time, min(event_time)over(partition by uid) mintime, total_amount from tb_order_overall where status = 1 
) a left join 
(
    select order_id, sum(price * cnt) total from tb_order_detail
    group by order_id
) b on a.order_id = b.order_id
where event_time = mintime and date_format(event_time,"%Y%m") = '202110'
```

SQL173 店铺901国庆期间的7日动销率和滞销率:

困难  通过率：11.09%  时间限制：1秒  空间限制：256M
描述
商品信息表tb_product_info
|id|product_id|shop_id|tag|in_price|quantity|release_time|
|---|---|---|---|---|---|---|
|1|8001|901|日用|60|1000|2020-01-01 10:00:00|
|2|8002|901|零食|140|500|2020-01-01 10:00:00|
|3|8003|901|零食|160|500|2020-01-01 10:00:00|

（product_id-商品ID, shop_id-店铺ID, tag-商品类别标签, in_price-进货价格, quantity-进货数量, release_time-上架时间）

订单总表tb_order_overall
|id|order_id|uid|event_time|total_amount|total_cnt|status|
|---|---|---|---|---|---|---|
|1|301004|102|2021-09-30 10:00:00|170|1|1|
|2|301005|104|2021-10-01 10:00:00|160|1|1|
|3|301003|101|2021-10-02 10:00:00|300|2|1|
|4|301002|102|2021-10-03 11:00:00|235|2|1|

（order_id-订单号, uid-用户ID, event_time-下单时间, total_amount-订单总金额, total_cnt-订单商品总件数, status-订单状态）

订单明细表tb_order_detail
|id|order_id|product_id|price|cnt|
|---|---|---|---|---|
|1|301004|8002|180|1|
|2|301005|8002|170|1|
|3|301002|8001|85|1|
|4|301002|8003|180|1|
|5|301003|8002|150|1|
|6|301003|8003|180|1|

（order_id-订单号, product_id-商品ID, price-商品单价, cnt-下单数量）

问题：请计算店铺901在2021年国庆头3天的7日动销率和滞销率，结果保留3位小数，按日期升序排序。

注：<br/>
动销率定义为店铺中一段时间内有销量的商品占当前已上架总商品数的比例（有销量的商品/已上架总商品数)。<br/>
滞销率定义为店铺中一段时间内没有销量的商品占当前已上架总商品数的比例。（没有销量的商品/已上架总商品数)。<br/>
只要当天任一店铺有任何商品的销量就输出该天的结果，即使店铺901当天的动销率为0。

输出示例：

示例数据的输出结果如下：
|dt|sale_rate|unsale_rate|
|---|---|---|
|2021-10-01|0.333|0.667|
|2021-10-02|0.667|0.333|
|2021-10-03|1.000|0.000|

解释：<br/>
10月1日的近7日（9月25日---10月1日）店铺901有销量的商品有8002，截止当天在售商品数为3，动销率为0.333，滞销率为0.667；<br/>
10月2日的近7日（9月26日---10月2日）店铺901有销量的商品有8002、8003，截止当天在售商品数为3，动销率为0.667，滞销率为0.333；<br/>
10月3日的近7日（9月27日---10月3日）店铺901有销量的商品有8002、8003、8001，截止当天店铺901在售商品数为3，动销率为1.000，滞销率为0.000；

```sql
with date_temp as
(select distinct date_format(event_time,"%Y-%m-%d") datestamp from tb_order_overall where date_format(event_time,"%Y-%m-%d") >= '2021-10-01' and date_format(event_time,"%Y-%m-%d") <= '2021-10-03'),
cnt_by_day as
(select distinct date_format(release_time,"%Y-%m-%d") by_day, count(product_id)over(partition by date_format(release_time,"%Y-%m-%d")) cnt_day from tb_product_info where shop_id = 901),
cnt_by_order as
(select distinct date_format(event_time,"%Y-%m-%d") eventdate, tb_order_detail.product_id from tb_order_overall left join tb_order_detail on tb_order_overall.order_id = tb_order_detail.order_id left join tb_product_info on tb_order_detail.product_id = tb_product_info.product_id where shop_id = 901)
select datestamp, round(count(distinct product_id)/max(cnt_day),3), round(1-count(distinct product_id)/max(cnt_day),3) from date_temp
left join cnt_by_order on date_add(eventdate,interval 6 day) >= datestamp and eventdate <= datestamp
left join cnt_by_day on datestamp >= by_day
group by datestamp
```

SQL174 2021年国庆在北京接单3次及以上的司机统计信息:

简单  通过率：13.62%  时间限制：1秒  空间限制：256M

描述

用户打车记录表tb_get_car_record
|id|uid|city|event_time|end_time|order_id|
|---|---|---|---|---|---|
|1|101|北京|2021-10-01 07:00:00|2021-10-01 07:02:00|NULL|
|2|102|北京|2021-10-01 09:00:30|2021-10-01 09:01:00|9001|
|3|101|北京|2021-10-01 08:28:10|2021-10-01 08:30:00|9002|
|4|103|北京|2021-10-02 07:59:00|2021-10-02 08:01:00|9003|
|5|104|北京|2021-10-03 07:59:20|2021-10-03 08:01:00|9004|
|6|105|北京|2021-10-01 08:00:00|2021-10-01 08:02:10|9005|
|7|106|北京|2021-10-01 17:58:00|2021-10-01 18:01:00|9006|
|8|107|北京|2021-10-02 11:00:00|2021-10-02 11:01:00|9007|
|9|108|北京|2021-10-02 21:00:00|2021-10-02 21:01:00|9008|

（uid-用户ID, city-城市, event_time-打车时间, end_time-打车结束时间, order_id-订单号）

打车订单表tb_get_car_order
|id|order_id|uid|driver_id|order_time|start_time|finish_time|mileage|fare|grade|
|---|---|---|---|---|---|---|---|---|---|
|1|9002|101|201|2021-10-01 08:30:00|NULL|2021-10-01 08:31:00|NULL|NULL|NULL|
|2|9001|102|202|2021-10-01 09:01:00|2021-10-01 09:06:00|2021-10-01 09:31:00|10|41.5|5|
|3|9003|103|202|2021-10-02 08:01:00|2021-10-02 08:15:00|2021-10-02 08:31:00|11|41.5|4|
|4|9004|104|202|2021-10-03 08:01:00|2021-10-03 08:13:00|2021-10-03 08:31:00|7.5|22|4|
|5|9005|105|203|2021-10-01 08:02:10|2021-10-01 08:18:00|2021-10-01 08:31:00|15|44|5|
|6|9006|106|203|2021-10-01 18:01:00|2021-10-01 18:09:00|2021-10-01 18:31:00|8|25|5|
|7|9007|107|203|2021-10-02 11:01:00|2021-10-02 11:07:00|2021-10-02 11:31:00|9.9|30|5|
|8|9008|108|203|2021-10-02 21:01:00|2021-10-02 21:10:00|2021-10-02 21:31:00|13.2|38|4|

（order_id-订单号, uid-用户ID, driver_id-司机ID, order_time-接单时间, start_time-开始计费的上车时间,  finish_time-订单完成时间, mileage-行驶里程数, fare-费用, grade-评分）

场景逻辑说明：<br/>
用户提交打车请求后，在用户打车记录表生成一条打车记录，order_id-订单号设为null；<br/>
当有司机接单时，在打车订单表生成一条订单，填充order_time-接单时间及其左边的字段，start_time-开始计费的上车时间及其右边的字段全部为null，并把order_id-订单号和order_time-接单时间（end_time-打车结束时间）写入打车记录表；若一直无司机接单，超时或中途用户主动取消打车，则记录end_time-打车结束时间。<br/>
若乘客上车前，乘客或司机点击取消订单，会将打车订单表对应订单的finish_time-订单完成时间填充为取消时间，其余字段设为null。<br/>
当司机接上乘客时，填充订单表中该start_time-开始计费的上车时间。<br/>
当订单完成时填充订单完成时间、里程数、费用；评分设为null，在用户给司机打1~5星评价后填充。

问题：请统计2021年国庆7天期间在北京市接单至少3次的司机的平均接单数和平均兼职收入（暂不考虑平台佣金，直接计算完成的订单费用总额），结果保留3位小数。

输出示例：

示例数据的输出结果如下
|city|avg_order_num|avg_income|
|---|---|---|
|北京|3.500|121.000|

解释：<br/>
在2021年国庆期间北京市的订单中，202共接了3单，兼职收入105；203接了4单，兼职收入137；201共接了1单，但取消了； 接单至少3次的司机有202和203，他两人全部总共接单数为7，总收入为242。因此平均接单数为3.500，平均收入为121.000；

```sql
select city, round(avg(cnt),3), round(avg(fee),3) from 
(
    select city, count(driver_id) cnt, sum(fare) fee from tb_get_car_order
    left join tb_get_car_record on  tb_get_car_order.order_id = tb_get_car_record.order_id
    where date_format(order_time,"%Y-%m-%d") >= "2021-10-01" and date_format(order_time,"%Y-%m-%d") <= "2021-10-07" and city = '北京'
    group by city, driver_id
    having count(driver_id) >= 3
) temp
group by city
```

SQL175 有取消订单记录的司机平均评分

简单  通过率：22.43%  时间限制：1秒  空间限制：256M

描述

现有用户打车记录表tb_get_car_record
|id|uid|city|event_time|end_time|order_id|
|---|---|---|---|---|---|
|1|101|北京|2021-10-01 07:00:00|2021-10-01 07:02:00|NULL|
|2|102|北京|2021-10-01 09:00:30|2021-10-01 09:01:00|9001|
|3|101|北京|2021-10-01 08:28:10|2021-10-01 08:30:00|9002|
|4|103|北京|2021-10-02 07:59:00|2021-10-02 08:01:00|9003|
|5|104|北京|2021-10-03 07:59:20|2021-10-03 08:01:00|9004|
|6|105|北京|2021-10-01 08:00:00|2021-10-01 08:02:10|9005|
|7|106|北京|2021-10-01 17:58:00|2021-10-01 18:01:00|9006|
|8|107|北京|2021-10-02 11:00:00|2021-10-02 11:01:00|9007|
|9|108|北京|2021-10-02 21:00:00|2021-10-02 21:01:00|9008|
|10|109|北京|2021-10-08 18:00:00|2021-10-08 18:01:00|9009|

（uid-用户ID, city-城市, event_time-打车时间, end_time-打车结束时间, order_id-订单号）

打车订单表tb_get_car_order
|id|order_id|uid|driver_id|order_time|start_time|finish_time|mileage|fare|grade|
|---|---|---|---|---|---|---|---|---|---|
|1|9002|101|202|2021-10-01 08:30:00|null|2021-10-01 08:31:00|null|null|null|
|2|9001|102|202|2021-10-01 09:01:00|2021-10-01 09:06:00|2021-10-01 09:31:00|10.0|41.5|5|
|3|9003|103|202|2021-10-02 08:01:00|2021-10-02 08:15:00|2021-10-02 08:31:00|11.0|41.5|4|
|4|9004|104|202|2021-10-03 08:01:00|2021-10-03 08:13:00|2021-10-03 08:31:00|7.5|22|4|
|5|9005|105|203|2021-10-01 08:02:10|null|2021-10-01 08:31:00|null|null|null|
|6|9006|106|203|2021-10-01 18:01:00|2021-10-01 18:09:00|2021-10-01 18:31:00|8.0|25.5|5|
|7|9007|107|203|2021-10-02 11:01:00|2021-10-02 11:07:00|2021-10-02 11:31:00|9.9|30|5|
|8|9008|108|203|2021-10-02 21:01:00|2021-10-02 21:10:00|2021-10-02 21:31:00|13.2|38|4|
|9|9009|109|203|2021-10-08 18:01:00|2021-10-08 18:11:50|2021-10-08 18:51:00|13|40|5|

（order_id-订单号, uid-用户ID, driver_id-司机ID, order_time-接单时间, start_time-开始计费的上车时间,  finish_time-订单完成时间, mileage-行驶里程数, fare-费用, grade-评分）

场景逻辑说明：<br/>
用户提交打车请求后，在用户打车记录表生成一条打车记录，order_id-订单号设为null；<br/>
当有司机接单时，在打车订单表生成一条订单，填充order_time-接单时间及其左边的字段，start_time-开始计费的上车时间及其右边的字段全部为null，并把order_id-订单号和order_time-接单时间（end_time-打车结束时间）写入打车记录表；若一直无司机接单，超时或中途用户主动取消打车，则记录end_time-打车结束时间。<br/>
若乘客上车前，乘客或司机点击取消订单，会将打车订单表对应订单的finish_time-订单完成时间填充为取消时间，其余字段设为null。<br/>
当司机接上乘客时，填充订单表中该start_time-开始计费的上车时间。<br/>
当订单完成时填充订单完成时间、里程数、费用；评分设为null，在用户给司机打1~5星评价后填充。

问题：请找到2021年10月有过取消订单记录的司机，计算他们每人全部已完成的有评分订单的平均评分及总体平均评分，保留1位小数。先按driver_id升序输出，再输出总体情况。

输出示例:

示例数据的输出结果如下
|driver_id|avg_grade|
|---|---|
|202|4.3|
|203|4.8|
|总体|4.6|

解释：<br/>
2021年国庆有未完成订单的司机有202和203；202的所有订单评分有：5、4、4，平均分为4.3；203的所有订单评分有：5、5、4、5，平均评分为4.8；总体平均评分为(5+4+4+5+5+4+5)/7=4.6

```sql
select * from
(
    select driver_id, round(avg(grade),1) from tb_get_car_order
    where driver_id in
    (
        select distinct driver_id from tb_get_car_order
        where finish_time is not null and date_format(finish_time,"%Y-%m") = '2021-10' and start_time is null
    )  
    group by driver_id
    order by driver_id
) test
union all
select '总体', round(avg(grade),1) from tb_get_car_order where driver_id in
    (
        select distinct driver_id from tb_get_car_order
        where finish_time is not null and date_format(finish_time,"%Y-%m") = '2021-10' and start_time is null
    ) 
```

SQL176 每个城市中评分最高的司机信息:

中等  通过率：14.87%  时间限制：1秒  空间限制：256M

描述

用户打车记录表tb_get_car_record
|id|uid|city|event_time|end_time|order_id|
|---|---|---|---|---|---|
|1|101|北京|2021-10-01 07:00:00|2021-10-01 07:02:00|NULL|
|2|102|北京|2021-10-01 09:00:30|2021-10-01 09:01:00|9001|
|3|101|北京|2021-10-01 08:28:10|2021-10-01 08:30:00|9002|
|4|103|北京|2021-10-02 07:59:00|2021-10-02 08:01:00|9003|
|5|104|北京|2021-10-03 07:59:20|2021-10-03 08:01:00|9004|
|6|105|北京|2021-10-01 08:00:00|2021-10-01 08:02:10|9005|
|7|106|北京|2021-10-01 17:58:00|2021-10-01 18:01:00|9006|
|8|107|北京|2021-10-02 11:00:00|2021-10-02 11:01:00|9007|
|9|108|北京|2021-10-02 21:00:00|2021-10-02 21:01:00|9008|
|10|109|北京|2021-10-08 18:00:00|2021-10-08 18:01:00|9009|

（uid-用户ID, city-城市, event_time-打车时间, end_time-打车结束时间, order_id-订单号）

打车订单表tb_get_car_order
|id|order_id|uid|driver_id|order_time|start_time|finish_time|mileage|fare|grade|
|---|---|---|---|---|---|---|---|---|---|
|1|9002|101|202|2021-10-01 08:30:00|NULL|2021-10-01 08:31:00|NULL|NULL|NULL|
|2|9001|102|202|2021-10-01 09:01:00|2021-10-01 09:06:00|2021-10-01 09:31:00|10|41.5|5|
|3|9003|103|202|2021-10-02 08:01:00|2021-10-02 08:15:00|2021-10-02 08:31:00|11|41.5|4|
|4|9004|104|202|2021-10-03 08:01:00|2021-10-03 08:13:00|2021-10-03 08:31:00|7.5|22|4|
|5|9005|105|203|2021-10-01 08:02:10|NULL|2021-10-01 08:31:00|NULL|NULL|NULL|
|6|9006|106|203|2021-10-01 18:01:00|2021-10-01 18:09:00|2021-10-01 18:31:00|8|25.5|5|
|7|9007|107|203|2021-10-02 11:01:00|2021-10-02 11:07:00|2021-10-02 11:31:00|9.9|30|5|
|8|9008|108|203|2021-10-02 21:01:00|2021-10-02 21:10:00|2021-10-02 21:31:00|13.2|38|4|
|9|9009|109|203|2021-10-08 18:01:00|2021-10-08 18:11:50|2021-10-08 18:51:00|13|40|5|

（order_id-订单号, uid-用户ID, driver_id-司机ID, order_time-接单时间, start_time-开始计费的上车时间,  finish_time-订单完成时间, mileage-行驶里程数, fare-费用, grade-评分）

场景逻辑说明：<br/>
用户提交打车请求后，在用户打车记录表生成一条打车记录，order_id-订单号设为null；<br/>
当有司机接单时，在打车订单表生成一条订单，填充order_time-接单时间及其左边的字段，start_time-开始计费的上车时间及其右边的字段全部为null，并把order_id-订单号和order_time-接单时间（end_time-打车结束时间）写入打车记录表；若一直无司机接单，超时或中途用户主动取消打车，则记录end_time-打车结束时间。<br/>
若乘客上车前，乘客或司机点击取消订单，会将打车订单表对应订单的finish_time-订单完成时间填充为取消时间，其余字段设为null。<br/>
当司机接上乘客时，填充订单表中该start_time-开始计费的上车时间。<br/>
当订单完成时填充订单完成时间、里程数、费用；评分设为null，在用户给司机打1~5星评价后填充。

问题：请统计每个城市中评分最高的司机平均评分、日均接单量和日均行驶里程数。

注：有多个司机评分并列最高时，都输出。<br/>
平均评分和日均接单量保留1位小数，<br/>
日均行驶里程数保留3位小数，按日均接单数升序排序。

2285068<br/>
示例数据的输出结果如下
|city|driver_id|avg_grade|avg_order_num|avg_mileage|
|---|---|---|---|---|
|北京|203|4.8|1.7|14.700|

解释：<br/>
示例数据中，在北京市，共有2个司机接单，202的平均评分为4.3，203的平均评分为4.8，因此北京的最高评分的司机为203；203的共在3天里接单过，一共接单5次（包含1次接单后未完成），因此日均接单数为1.7；总行驶里程数为44.1，因此日均行驶里程数为14.700

```sql
select city, driver_id, round(avg_grade,1), round(daily_num,1), round(daily_mile,3) from
(
    select driver_id, city, avg(grade) avg_grade,
    count(order_time)/count(distinct date_format(order_time,"%Y%m%d")) daily_num,
    sum(mileage)/count(distinct date_format(order_time,"%Y%m%d")) daily_mile,
    rank()over(partition by city order by avg(grade) desc) rk
    from tb_get_car_order
    left join tb_get_car_record on tb_get_car_order.order_id = tb_get_car_record.order_id
    where order_time is not null
    group by driver_id, city
) temp where rk = 1 order by daily_num
```

SQL177 国庆期间近7日日均取消订单量:

中等  通过率：27.70%  时间限制：1秒  空间限制：256M

描述

现有用户打车记录表tb_get_car_record
|id|uid|city|event_time|end_time|order_id|
|---|---|---|---|---|---|
|1|101|北京|2021-09-25 08:28:10|2021-09-25 08:30:00|9011|
|2|102|北京|2021-09-25 09:00:30|2021-09-25 09:01:00|9012|
|3|103|北京|2021-09-26 07:59:00|2021-09-26 08:01:00|9013|
|4|104|北京|2021-09-26 07:59:00|2021-09-26 08:01:00|9023|
|5|104|北京|2021-09-27 07:59:20|2021-09-27 08:01:00|9014|
|6|105|北京|2021-09-28 08:00:00|2021-09-28 08:02:10|9015|
|7|106|北京|2021-09-29 17:58:00|2021-09-29 18:01:00|9016|
|8|107|北京|2021-09-30 11:00:00|2021-09-30 11:01:00|9017|
|9|108|北京|2021-09-30 21:00:00|2021-09-30 21:01:00|9018|
|10|102|北京|2021-10-01 09:00:30|2021-10-01 09:01:00|9002|
|11|106|北京|2021-10-01 17:58:00|2021-10-01 18:01:00|9006|
|12|101|北京|2021-10-02 08:28:10|2021-10-02 08:30:00|9001|
|13|107|北京|2021-10-02 11:00:00|2021-10-02 11:01:00|9007|
|14|108|北京|2021-10-02 21:00:00|2021-10-02 21:01:00|9008|
|15|103|北京|2021-10-02 07:59:00|2021-10-02 08:01:00|9003|
|16|104|北京|2021-10-03 07:59:20|2021-10-03 08:01:00|9004|
|17|109|北京|2021-10-03 18:00:00|2021-10-03 18:01:00|9009|

（uid-用户ID, city-城市, event_time-打车时间, end_time-打车结束时间, order_id-订单号）

打车订单表tb_get_car_order
|id|order_id|uid|driver_id|order_time|start_time|finish_time|mileage|fare|grade|
|---|---|---|---|---|---|---|---|---|---|
|1|9011|101|211|2021-09-25 08:30:00|2021-09-25 08:31:00|2021-09-25 08:54:00|10|35|5|
|2|9012|102|211|2021-09-25 09:01:00|2021-09-25 09:01:50|2021-09-25 09:28:00|11|32|5|
|3|9013|103|212|2021-09-26 08:01:00|2021-09-26 08:03:00|2021-09-26 08:27:00|12|31|4|
|4|9023|104|213|2021-09-26 08:01:00|NULL|2021-09-26 08:27:00|NULL|NULL|NULL|
|5|9014|104|212|2021-09-27 08:01:00|2021-09-27 08:04:00|2021-09-27 08:21:00|11|31|5|
|6|9015|105|212|2021-09-28 08:02:10|2021-09-28 08:04:10|2021-09-28 08:25:10|12|31|4|
|7|9016|106|213|2021-09-29 18:01:00|"2021-09-29 18:02:10"|2021-09-29 18:23:00|11|39|4|
|8|9017|107|213|2021-09-3011:01:00|2021-09-30 11:01:40|2021-09-30 11:31:00|11|38|5|
|9|9018|108|214|2021-09-30 21:01:00|2021-09-30 21:02:50|2021-09-30 21:21:00|14|38|5|
|10|9002|102|202|2021-10-01 09:01:00|2021-10-01 0 9:06:00|2021-10-01 09:31:00|10|41.5|5|
|11|9006|106|203|2021-10-0118:01:00|2021-10-01 18:09:00|2021-10-01 18:31:00|8|25.5|4|
|12|9001|101|202|2021-10-02 08:30:00|NULL|2021-10-02 08:31:00|NULL|NULL|NULL|
|13|9007|107|203|2021-10-02 11:01:00|"2021-10-02 11:07:00"|2021-10-02 11:31:00|9.9|30|5|
|14|9008|108|204|2021-10-02 21:01:00|2021-10-02 21:10:00|2021-10-02 21:31:00|13.2|38|4|
|15|9003|103|202|2021-10-02 08:01:00|2021-10-02 08:15:00|2021-10-02 08:31:00|11|41.5|4|
|16|9004|104|202|2021-10-03 08:01:00|2021-10-03 08:13:00|2021-10-03 08:31:00|7.5|22|4|
|17|9009|109|204|2021-10-0318:01:00|NULL|2021-10-03 18:51:00|NULL|NULL|NULL|

（order_id-订单号, uid-用户ID, driver_id-司机ID, order_time-接单时间, start_time-开始计费的上车时间,  finish_time-订单完成时间, mileage-行驶里程数, fare-费用, grade-评分）

场景逻辑说明：<br/>
用户提交打车请求后，在用户打车记录表生成一条打车记录，order_id-订单号设为null；<br/>
当有司机接单时，在打车订单表生成一条订单，填充order_time-接单时间及其左边的字段，start_time-开始计费的上车时间及其右边的字段全部为null，并把order_id-订单号和order_time-接单时间（end_time-打车结束时间）写入打车记录表；若一直无司机接单，超时或中途用户主动取消打车，则记录end_time-打车结束时间。<br/>
若乘客上车前，乘客或司机点击取消订单，会将打车订单表对应订单的finish_time-订单完成时间填充为取消时间，其余字段设为null。<br/>
当司机接上乘客时，填充订单表中该start_time-开始计费的上车时间。<br/>
当订单完成时填充订单完成时间、里程数、费用；评分设为null，在用户给司机打1~5星评价后填充。

问题：请统计国庆头3天里，每天的近7日日均订单完成量和日均订单取消量，按日期升序排序。结果保留2位小数。

输出示例：

示例输出如下
|dt|finish_num_7d|cancel_num_7d|
|---|---|---|
|2021-10-01|1.43|0.14|
|2021-10-02|1.57|0.29|
|2021-10-03|1.57|0.29|

解释：<br/>
2021年9月25到10月3日每天的订单完成量为：2、1、1、1、1、2、2、3、1；每天的订单取消量为：0、1、0、0、0、0、0、1、1；<br/>
因此10.1到10.3期间的近7日订单完成量分别为10、11、11，因此日均订单完成量为：1.43、1.57、1.57；<br/>
近7日订单取消量分别为1、2、2，因此日均订单取消量为0.14、0.29、0.29；

```sql
with test as
(
    select '2021-10-01' datetime union select '2021-10-02' union select '2021-10-03'
)
select datetime, round(count(start_time)/7,2), round((count(order_time)-count(start_time))/7,2) from test
left join tb_get_car_order on date_add(order_time, interval 6 day) >= datetime and date_format(order_time,"%Y-%m-%d") <= datetime
group by datetime
order by datetime
```

SQL178 工作日各时段叫车量、等待接单时间和调度时间:

较难  通过率：19.47%  时间限制：1秒  空间限制：256M

描述

用户打车记录表tb_get_car_record
|id|uid|city|event_time|end_time|order_id|
|---|---|---|---|---|---|
|1|107|北京|2021-09-20 11:00:00|2021-09-20 11:00:30|9017|
|2|108|北京|2021-09-20 21:00:00|2021-09-20 21:00:40|9008|
|3|108|北京|2021-09-20 18:59:30|2021-09-20 19:01:00|9018|
|4|102|北京|2021-09-21 08:59:00|2021-09-21 09:01:00|9002|
|5|106|北京|2021-09-21 17:58:00|2021-09-21 18:01:00|9006|
|6|103|北京|2021-09-22 07:58:00|2021-09-22 08:01:00|9003|
|7|104|北京|2021-09-23 07:59:00|2021-09-23 08:01:00|9004|
|8|103|北京|2021-09-24 19:59:20|2021-09-24 20:01:00|9019|
|9|101|北京|2021-09-24 08:28:10|2021-09-24 08:30:00|9011|

（uid 用户ID, city-城市, event_time-打车时间, end_time-打车结束时间, order_id-订单号）

打车订单表tb_get_car_order
|id|order_id|uid|driver_id|order_time|start_time|finish_time|mileage|fare|grade|
|---|---|---|---|---|---|---|---|---|---|
|1|9017|107|213|2021-09-20 11:00:30|2021-09-20 11:02:10|2021-09-20 11:31:00|11|38|5|
|2|9008|108|204|2021-09-20 21:00:40|2021-09-20 21:03:00|2021-09-20 21:31:00|13.2|38|4|
|3|9018|108|214|2021-09-20 19:01:00|2021-09-20 19:04:50|2021-09-20 19:21:00|14|38|5|
|4|9002|102|202|2021-09-21 09:01:00|2021-09-21 09:06:00|2021-09-21 09:31:00|10|41.5|5|
|5|9006|106|203|2021-09-21 18:01:00|2021-09-21 18:09:00|2021-09-21 18:31:00|8|25.5|4|
|6|9007|107|203|2021-09-22 11:01:00|2021-09-22 11:07:00|2021-09-22 11:31:00|9.9|30|5|
|7|9003|103|202|2021-09-22 08:01:00|2021-10-22 08:15:00|2021-10-22 08:31:00|11|41.5|4|
|8|9004|104|202|2021-09-23 08:01:00|2021-09-23 08:13:00|2021-09-23 08:31:00|7.5|22|4|
|9|9005|105|202|2021-09-23 10:01:00|2021-09-23 10:13:00|2021-09-23 10:31:00|9|29|5|
|10|9019|103|202|2021-09-24 20:01:00|2021-09-24 20:11:00|2021-09-24 20:51:00|10|39|4|
|11|9011|101|211|2021-09-24 08:30:00|2021-09-24 08:31:00|2021-09-24 08:54:00|10|35|5|

（order_id-订单号, uid-用户ID, driver_id-司机ID, order_time-接单时间, start_time-开始计费的上车时间, finish_time-订单完成时间, mileage-行驶里程数, fare-费用, grade-评分）

场景逻辑说明：<br/>
用户提交打车请求后，在用户打车记录表生成一条打车记录，订单号-order_id设为null；<br/>
当有司机接单时，在打车订单表生成一条订单，填充接单时间-order_time 及其左边的字段，上车时间-start_time及其右边的字段全部为null，并把订单号-order_id和接单时间-order_time（end_time-打车结束时间）写入打车记录表；若一直无司机接单，超时或中途用户主动取消打车，则记录打车结束时间-end_time。<br/>
若乘客上车前，乘客或司机点击取消订单，会将打车订单表对应订单的finish_time-订单完成时间填充为取消时间，其余字段设为null。<br/>
当司机接上乘客时，填充订单表中该订单的start_time-上车时间。<br/>
当订单完成时填充订单完成时间、里程数、费用；评分设为null，在用户给司机打1~5星评价后填充。

问题：统计周一到周五各时段的叫车量、平均等待接单时间和平均调度时间。全部以event_time-开始打车时间为时段划分依据，平均等待接单时间和平均调度时间均保留1位小数，平均调度时间仅计算完成了的订单，结果按叫车量升序排序。

注：<br/>
不同时段定义：早高峰 [07:00:00 , 09:00:00)、工作时间 [09:00:00 , 17:00:00）、晚高峰 [17:00:00 , 20:00:00）、休息时间 [20:00:00 , 07:00:00）
时间区间左闭右开（即7:00:00算作早高峰，而9:00:00不算做早高峰）<br/>
从开始打车到司机接单为等待接单时间，从司机接单到上车为调度时间。

输出示例：

示例数据的输出结果如下：
|period|get_car_num|avg_wait_time|avg_dispatch_time|
|---|---|---|---|
|工作时间|1|0.5|1.7|
|休息时间|1|0.7|2.3|
|晚高峰|3|2.1|7.3|
|早高峰|4|2.2|8.0|

解释：订单9017打车开始于11点整，属于工作时间，等待时间30秒，调度时间为1分40秒，示例数据中工作时间打车订单就一个，平均等待时间0.5分钟，平均调度时间1.7分钟。

```sql
select 
case when hour(event_time) >= 7 and hour(event_time) < 9 then '早高峰'
when hour(event_time) >= 9 and hour(event_time) < 17 then '工作时间'
when hour(event_time) >= 17 and hour(event_time) < 20 then '晚高峰'
else '休息时间' end,
count(event_time),
round(avg(timestampdiff(second,event_time,order_time))/60,1),
round(avg(timestampdiff(second,order_time,start_time))/60,1)
from tb_get_car_order
left join tb_get_car_record on  tb_get_car_order.order_id = tb_get_car_record.order_id
where weekday(event_time) < 5
group by case when hour(event_time) >= 7 and hour(event_time) < 9 then '早高峰'
when hour(event_time) >= 9 and hour(event_time) < 17 then '工作时间'
when hour(event_time) >= 17 and hour(event_time) < 20 then '晚高峰'
else '休息时间' end
order by count(event_time)
```

SQL179 各城市最大同时等车人数:

较难  通过率：17.12%  时间限制：1秒  空间限制：256M

描述

用户打车记录表tb_get_car_record
|id|uid|city|event_time|end_time|order_id|
|---|---|---|---|---|---|
|1|108|北京|2021-10-20 08:00:00|2021-10-20 08:00:40|9008|
|2|118|北京|2021-10-20 08:00:10|2021-10-20 08:00:45|9018|
|3|102|北京|2021-10-20 08:00:30|2021-10-20 08:00:50|9002|
|4|106|北京|2021-10-20 08:05:41|2021-10-20 08:06:00|9006|
|5|103|北京|2021-10-20 08:05:50|2021-10-20 08:07:10|9003|
|6|104|北京|2021-10-20 08:01:01|2021-10-20 08:01:20|9004|
|7|105|北京|2021-10-20 08:01:15|2021-10-20 08:01:30|9019|
|8|101|北京|2021-10-20 08:28:10|2021-10-20 08:30:00|9011|

（uid-用户ID, city-城市, event_time-打车时间, end_time-打车结束时间, order_id-订单号）

打车订单表tb_get_car_order
|id|order_id|uid|driver_id|order_time|start_time|finish_time|mileage|fare|grade|
|---|---|---|---|---|---|---|---|---|---|
|1|9008|108|204|2021-10-20 08:00:40|2021-10-20 08:03:00|2021-10-20 08:31:00|13.2|38|4|
|2|9018|108|214|2021-10-20 08:00:45|2021-10-20 08:04:50|2021-10-20 08:21:00|14|38|5|
|3|9002|102|202|2021-10-20 08:00:50|2021-10-20 08:06:00|2021-10-20 08:31:00|10|41.5|5|
|4|9006|106|206|2021-10-20 08:06:00|2021-10-20 08:09:00|2021-10-20 08:31:00|8|25.5|4|
|5|9003|103|203|2021-10-20 08:07:10|2021-10-20 08:15:00|2021-10-20 08:31:00|11|41.5|4|
|6|9004|104|204|2021-10-20 08:01:20|2021-10-20 08:13:00|2021-10-20 08:31:00|7.5|22|4|
|7|9019|105|205|2021-10-20 08:01:30|2021-10-20 08:11:00|2021-10-20 08:51:00|10|39|4|
|8|9011|101|211|2021-10-20 08:30:00|2021-10-20 08:31:00|2021-10-20 08:54:00|10|35|5|

（order_id-订单号, uid-用户ID, driver_id-司机ID, order_time-接单时间, start_time-开始计费的上车时间, finish_time-订单完成时间, mileage-行驶里程数, fare-费用, grade-评分）

场景逻辑说明：<br/>
用户提交打车请求后，在用户打车记录表生成一条打车记录，订单号-order_id设为null；<br/>
当有司机接单时，在打车订单表生成一条订单，填充接单时间-order_time及其左边的字段，上车时间及其右边的字段全部为null，并把订单号和接单时间（打车结束时间）写入打车记录表；若一直无司机接单、超时或中途用户主动取消打车，则记录打车结束时间。<br/>
若乘客上车前，乘客或司机点击取消订单，会将打车订单表对应订单的订单完成时间-finish_time填充为取消时间，其余字段设为null。<br/>
当司机接上乘客时，填充打车订单表中该订单的上车时间start_time。<br/>
当订单完成时填充订单完成时间、里程数、费用；评分设为null，在用户给司机打1~5星评价后填充。

问题：请统计各个城市在2021年10月期间，单日中最大的同时等车人数。

注: <br/>
等车指从开始打车起，直到取消打车、取消等待或上车前的这段时间里用户的状态。<br/>
如果同一时刻有人停止等车，有人开始等车，等车人数记作先增加后减少。<br/>
结果按各城市最大等车人数升序排序，相同时按城市升序排序。<br/>

输出示例：

示例结果如下
|city|max_wait_uv|
|---|---|
|北京|5|

解释：由打车订单表可以得知北京2021年10月20日有8条打车记录，108号乘客从08:00:00等到08:03:00，118号乘客从08:00:10等到08:04:50....,由此得知08:02:00秒时刻，共有5人在等车。

```sql
select city, max(num) max_num from
(
    select city, sum(tag)over(partition by city order by start_time, tag desc) num from 
(
    select a.order_id, a.uid, start_time, city, -1 as tag from 
        (
            select order_id, uid, start_time from tb_get_car_order
            where start_time is not null and date_format(start_time,"%Y%m") = '202110'
            union 
            select order_id, uid, finish_time from tb_get_car_order
            where finish_time is not null and start_time is null and date_format(finish_time,"%Y%m") = '202110'
        ) a left join tb_get_car_record on a.order_id = tb_get_car_record.order_id
        union
        select order_id, uid, event_time, city, 1 from tb_get_car_record where date_format(event_time,"%Y%m") = '202110'
    ) b
) c group by city
order by max_num, city
```

SQL180 某宝店铺的SPU数量:

简单  通过率：61.47%  时间限制：1秒  空间限制：256M

描述

11月结束后，小牛同学需要对其在某宝的网店就11月份用户交易情况和产品情况进行分析以更好的经营小店。<br/>
已知产品情况表product_tb如下（其中，item_id指某款号的具体货号，style_id指款号，tag_price表示标签价格，inventory指库存量）：
|item_id|style_id|tag_price|inventory|
|---|---|---|---|
|A001|A|100|20|
|A002|A|120|30|
|A003|A|200|15|
|B001|B|130|18|
|B002|B|150|22|
|B003|B|125|10|
|B004|B|155|12|
|C001|C|260|25|
|C002|C|280|18|

请你统计每款的SPU（货号）数量，并按SPU数量降序排序，以上例子的输出结果如下：
|style_id|SPU_num|
|---|---|
|B|4|
|A|3|
|C|2|

```sql
select style_id, count(*) cnt from product_tb
group by style_id
order by cnt desc
```

SQL181 某宝店铺的实际销售额与客单价:

简单  通过率：48.44%  时间限制：1秒  空间限制：256M

描述

11月结束后，小牛同学需要对其在某宝的网店就11月份用户交易情况和产品情况进行分析以更好的经营小店。<br/>
已知11月份销售数据表sales_tb如下（其中，sales_date表示销售日期，user_id指用户编号，item_id指货号，sales_num表示销售数量，sales_price表示结算金额）：
|sales_date|user_id|item_id|sales_num|sales_price|
|---|---|---|---|---|
|2021-11-01|1|A001|1|90|
|2021-11-01|2|A002|2|220|
|2021-11-01|2|B001|1|120|
|2021-11-02|3|C001|2|500|
|2021-11-02|4|B001|1|120|
|2021-11-03|5|C001|1|240|
|2021-11-03|6|C002|1|270|
|2021-11-04|7|A003|1|180|
|2021-11-04|8|B002|1|140|
|2021-11-04|9|B001|1|125|
|2021-11-05|10|B003|1|120|
|2021-11-05|10|B004|1|150|
|2021-11-05|10|A003|1|180|
|2021-11-06|11|B003|1|120|
|2021-11-06|10|B004|1|150|

请你统计实际总销售额与客单价（人均付费，总收入/总用户数，结果保留两位小数），以上例子的输出结果如下：
|sales_total|per_trans|
|---|---|
|2725|247.73|

```sql
select sum(sales_price), round(sum(sales_price)/count(distinct user_id),2) from sales_tb
```

SQL182 某宝店铺折扣率:

中等  通过率：35.64%  时间限制：1秒  空间限制：256M

描述

11月结束后，小牛同学需要对其在某宝的网店就11月份用户交易情况和产品情况进行分析以更好的经营小店。<br/>
已知产品情况表product_tb如下（其中，item_id指某款号的具体货号，style_id指款号，tag_price表示标签价格，inventory指库存量）：
|item_id|style_id|tag_price|inventory|
|---|---|---|---|
|A001|A|100|20|
|A002|A|120|30|
|A003|A|200|15|
|B001|B|130|18|
|B002|B|150|22|
|B003|B|125|10|
|B004|B|155|12|
|C001|C|260|25|
|C002|C|280|18|

11月份销售数据表sales_tb如下（其中，sales_date表示销售日期，user_id指用户编号，item_id指货号，sales_num表示销售数量，sales_price表示结算金额）：
|sales_date|user_id|item_id|sales_num|sales_price|
|---|---|---|---|---|
|2021-11-01|1|A001|1|90|
|2021-11-01|2|A002|2|220|
|2021-11-01|2|B001|1|120|
|2021-11-02|3|C001|2|500|
|2021-11-02|4|B001|1|120|
|2021-11-03|5|C001|1|240|
|2021-11-03|6|C002|1|270|
|2021-11-04|7|A003|1|180|
|2021-11-04|8|B002|1|140|
|2021-11-04|9|B001|1|125|
|2021-11-05|10|B003|1|120|
|2021-11-05|10|B004|1|150|
|2021-11-05|10|A003|1|180|
|2021-11-06|11|B003|1|120|
|2021-11-06|10|B004|1|150|

请你统计折扣率（GMV/吊牌金额，GMV指的是成交金额），以上例子的输出结果如下（折扣率保留两位小数）：
|discount_rate(%)|
|---|
|93.97|

```sql
select round(sum(sales_price)*100/sum(tag_price*sales_num),2) from sales_tb
left join product_tb on
sales_tb.item_id = product_tb.item_id
```

SQL183 某宝店铺动销率与售罄率:

较难  通过率：23.04%  时间限制：1秒  空间限制：256M

描述

11月结束后，小牛同学需要对其在某宝的网店就11月份用户交易情况和产品情况进行分析以更好的经营小店。<br/>
已知产品情况表product_tb如下（其中，item_id指某款号的具体货号，style_id指款号，tag_price表示标签价格，inventory指库存量）：
|item_id|style_id|tag_price|inventory|
|---|---|---|---|
|A001|A|100|20|
|A002|A|120|30|
|A003|A|200|15|
|B001|B|130|18|
|B002|B|150|22|
|B003|B|125|10|
|B004|B|155|12|
|C001|C|260|25|
|C002|C|280|18|

11月份销售数据表sales_tb如下（其中，sales_date表示销售日期，user_id指用户编号，item_id指货号，sales_num表示销售数量，sales_price表示结算金额）：
|sales_date|user_id|item_id|sales_num|sales_price|
|---|---|---|---|---|
|2021-11-01|1|A001|1|90|
|2021-11-01|2|A002|2|220|
|2021-11-01|2|B001|1|120|
|2021-11-02|3|C001|2|500|
|2021-11-02|4|B001|1|120|
|2021-11-03|5|C001|1|240|
|2021-11-03|6|C002|1|270|
|2021-11-04|7|A003|1|180|
|2021-11-04|8|B002|1|140|
|2021-11-04|9|B001|1|125|
|2021-11-05|10|B003|1|120|
|2021-11-05|10|B004|1|150|
|2021-11-05|10|A003|1|180|
|2021-11-06|11|B003|1|120|
|2021-11-06|10|B004|1|150|

请你统计每款的动销率（pin_rate，有销售的SKU数量/在售SKU数量）与售罄率（sell-through_rate，GMV/备货值，备货值=吊牌价*库存数），按style_id升序排序，以上例子的输出结果如下：
|style_id|pin_rate(%)|sell-through_rate(%)|
|---|---|---|
|A|8.33|7.79|
|B|14.81|11.94|
|C|10.26|8.75|

```sql
select style_id, round(sum(salesnum)*100/(sum(inventory)-sum(salesnum)),2), 
round(sum(salesprice)*100/sum(tag_price*inventory),2)
from
(
    select item_id, sum(sales_num) salesnum, sum(sales_price) salesprice from sales_tb group by item_id
) a
left join product_tb on product_tb.item_id = a.item_id
group by style_id
order by style_id
```

SQL184 某宝店铺连续2天及以上购物的用户及其对应的天数:

较难  通过率：33.25%  时间限制：1秒  空间限制：256M

描述

11月结束后，小牛同学需要对其在某宝的网店就11月份用户交易情况和产品情况进行分析以更好的经营小店。<br/>
11月份销售数据表sales_tb如下（其中，sales_date表示销售日期，user_id指用户编号，item_id指货号，sales_num表示销售数量，sales_price表示结算金额）：
|sales_date|user_id|item_id|sales_num|sales_price|
|---|---|---|---|---|
|2021-11-01|1|A001|1|90|
|2021-11-01|2|A002|2|220|
|2021-11-01|2|B001|1|120|
|2021-11-02|3|C001|2|500|
|2021-11-02|4|B001|1|120|
|2021-11-03|5|C001|1|240|
|2021-11-03|6|C002|1|270|
|2021-11-04|7|A003|1|180|
|2021-11-04|8|B002|1|140|
|2021-11-04|9|B001|1|125|
|2021-11-05|10|B003|1|120|
|2021-11-05|10|B004|1|150|
|2021-11-05|10|A003|1|180|
|2021-11-06|11|B003|1|120|
|2021-11-06|10|B004|1|150|

请你统计连续2天及以上在该店铺购物的用户及其对应的次数（若有多个用户，按user_id升序排序），以上例子的输出结果如下：
|user_id|days_count|
|---|---|
|10|2|

```sql
select user_id, count(distinct sales_date) from sales_tb
where user_id in
(
    select distinct a.user_id from sales_tb a
    inner join sales_tb b on a.user_id = b.user_id and date_add(a.sales_date, interval 1 day) = b.sales_date
)
group by user_id
order by user_id
```

SQL185 牛客直播转换率:

简单  通过率：31.99%  时间限制：1秒  空间限制：256M

描述

牛客某页面推出了数据分析系列直播课程介绍。用户可以选择报名任意一场或多场直播课。<br/>
已知课程表course_tb如下（其中course_id代表课程编号，course_name表示课程名称，course_datetime代表上课时间）：
|course_id|course_name|course_datetime|
|---|---|---|
|1|Python|2021-12-1 19:00-21:00|
|2|SQL|2021-12-2 19:00-21:00|
|3|R|2021-12-3 19:00-21:00|

用户行为表behavior_tb如下（其中user_id表示用户编号、if_vw表示是否浏览、if_fav表示是否收藏、if_sign表示是否报名、course_id代表课程编号）：
|user_id|if_vw|if_fav|if_sign|course_id|
|---|---|---|---|---|
|100|1|1|1|1|
|100|1|1|1|2|
|100|1|1|1|3|
|101|1|1|1|1|
|101|1|1|1|2|
|101|1|0|0|3|
|102|1|1|1|1|
|102|1|1|1|2|
|102|1|1|1|3|
|103|1|1|0|1|
|103|1|0|0|2|
|103|1|0|0|3|
|104|1|1|1|1|
|104|1|1|1|2|
|104|1|1|0|3|
|105|1|0|0|1|
|106|1|0|0|1|
|107|1|0|0|1|
|107|1|1|1|2|
|108|1|1|1|3|

请你统计每个科目的转换率（sign_rate(%)，转化率=报名人数/浏览人数，结果保留两位小数）。<br/>
注：按照course_id升序排序。
|course_id|course_name|sign_rate(%)|
|---|---|---|
|1|Python|50.00|
|2|SQL|83.33|
|3|R|50.00|

```sql
select course_tb.course_id, course_name, 
round(sum(if_sign)*100/sum(if_vw),2)
from behavior_tb
left join course_tb on behavior_tb.course_id = course_tb.course_id
group by course_tb.course_id, course_name
order by course_tb.course_id
```

SQL186 牛客直播开始时各直播间在线人数:

中等  通过率：34.91%  时间限制：1秒  空间限制：256M

描述

牛客某页面推出了数据分析系列直播课程介绍。用户可以选择报名任意一场或多场直播课。<br/>
已知课程表course_tb如下（其中course_id代表课程编号，course_name表示课程名称，course_datetime代表上课时间）：
|course_id|course_name|course_datetime|
|---|---|---|
|1|Python|2021-12-1 19:00-21:00|
|2|SQL|2021-12-2 19:00-21:00|
|3|R|2021-12-3 19:00-21:00|

上课情况表attend_tb如下（其中user_id表示用户编号、course_id代表课程编号、in_datetime表示进入直播间的时间、out_datetime表示离开直播间的时间）：
|user_id|course_id|in_datetime|out_datetime|
|---|---|---|---|
|100|1|2021-12-01 19:00:00|2021-12-01 19:28:00|
|100|1|2021-12-01 19:30:00|2021-12-01 19:53:00|
|101|1|2021-12-01 19:00:00|2021-12-01 20:55:00|
|102|1|2021-12-01 19:00:00|2021-12-01 19:05:00|
|104|1|2021-12-01 19:00:00|2021-12-01 20:59:00|
|101|2|2021-12-02 19:05:00|2021-12-02 20:58:00|
|102|2|2021-12-02 18:55:00|2021-12-02 21:00:00|
|104|2|2021-12-02 18:57:00|2021-12-02 20:56:00|
|107|2|2021-12-02 19:10:00|2021-12-02 19:18:00|
|100|3|2021-12-03 19:01:00|2021-12-03 21:00:00|
|102|3|2021-12-03 18:58:00|2021-12-03 19:05:00|
|108|3|2021-12-03 19:01:00|2021-12-03 19:56:00|

请你统计直播开始时（19：00），各科目的在线人数，以上例子的输出结果为（按照course_id升序排序）：
|course_id|course_name|online_num|
|---|---|---|
|1|Python|4|
|2|SQL|2|
|3|R|1|

```sql
select course_tb.course_id, course_name, count(distinct user_id)
from course_tb
right join attend_tb on course_tb.course_id = attend_tb.course_id
where in_datetime <= date_format(course_datetime,"%Y-%m-%d 19:00:00") and out_datetime >= date_format(course_datetime,"%Y-%m-%d 19:00:00")
group by course_tb.course_id, course_name
order by course_tb.course_id
```

SQL187 牛客直播各科目平均观看时长:

中等  通过率：44.76%  时间限制：1秒  空间限制：256M

描述

牛客某页面推出了数据分析系列直播课程介绍。用户可以选择报名任意一场或多场直播课。<br/>
已知课程表course_tb如下（其中course_id代表课程编号，course_name表示课程名称，course_datetime代表上课时间）：
|course_id|course_name|course_datetime|
|---|---|---|
|1|Python|2021-12-1 19:00-21:00|
|2|SQL|2021-12-2 19:00-21:00|
|3|R|2021-12-3 19:00-21:00|

上课情况表attend_tb如下（其中user_id表示用户编号、course_id代表课程编号、in_datetime表示进入直播间的时间、out_datetime表示离开直播间的时间）：
|user_id|course_id|in_datetime|out_datetime|
|---|---|---|---|
|100|1|2021-12-01 19:00:00|2021-12-01 19:28:00|
|100|1|2021-12-01 19:30:00|2021-12-01 19:53:00|
|101|1|2021-12-01 19:00:00|2021-12-01 20:55:00|
|102|1|2021-12-01 19:00:00|2021-12-01 19:05:00|
|104|1|2021-12-01 19:00:00|2021-12-01 20:59:00|
|101|2|2021-12-02 19:05:00|2021-12-02 20:58:00|
|102|2|2021-12-02 18:55:00|2021-12-02 21:00:00|
|104|2|2021-12-02 18:57:00|2021-12-02 20:56:00|
|107|2|2021-12-02 19:10:00|2021-12-02 19:18:00|
|100|3|2021-12-03 19:01:00|2021-12-03 21:00:00|
|102|3|2021-12-03 18:58:00|2021-12-03 19:05:00|
|108|3|2021-12-03 19:01:00|2021-12-03 19:56:00|

请你统计每个科目的平均观看时长（观看时长定义为离开直播间的时间与进入直播间的时间之差，单位是分钟），输出结果按平均观看时长降序排序，结果保留两位小数。
|course_name|avg_Len|
|---|---|
|SQL|91.25|
|R|60.33|
|Python|58.00|

```sql
select course_name, 
round(avg(timestampdiff(minute, in_datetime, out_datetime)),2) avg_len
from attend_tb
left join course_tb on attend_tb.course_id = course_tb.course_id
group by course_name
order by avg_len desc
```


SQL188 牛客直播各科目出勤率:

较难  通过率：22.58%  时间限制：1秒  空间限制：256M

描述

牛客某页面推出了数据分析系列直播课程介绍。用户可以选择报名任意一场或多场直播课。<br/>
已知课程表course_tb如下（其中course_id代表课程编号，course_name表示课程名称，course_datetime代表上课时间）：
|course_id|course_name|course_datetime|
|---|---|---|
|1|Python|2021-12-1 19:00-21:00|
|2|SQL|2021-12-2 19:00-21:00|
|3|R|2021-12-3 19:00-21:00|

上课情况表attend_tb如下（其中user_id表示用户编号、course_id代表课程编号、in_datetime表示进入直播间的时间、out_datetime表示离开直播间的时间）：
|user_id|course_id|in_datetime|out_datetime|
|---|---|---|---|
|100|1|2021-12-01 19:00:00|2021-12-01 19:28:00|
|100|1|2021-12-01 19:30:00|2021-12-01 19:53:00|
|101|1|2021-12-01 19:00:00|2021-12-01 20:55:00|
|102|1|2021-12-01 19:00:00|2021-12-01 19:05:00|
|104|1|2021-12-01 19:00:00|2021-12-01 20:59:00|
|101|2|2021-12-02 19:05:00|2021-12-02 20:58:00|
|102|2|2021-12-02 18:55:00|2021-12-02 21:00:00|
|104|2|2021-12-02 18:57:00|2021-12-02 20:56:00|
|107|2|2021-12-02 19:10:00|2021-12-02 19:18:00|
|100|3|2021-12-03 19:01:00|2021-12-03 21:00:00|
|102|3|2021-12-03 18:58:00|2021-12-03 19:05:00|
|108|3|2021-12-03 19:01:00|2021-12-03 19:56:00|

请你统计每个科目的出勤率（attend_rate(%)，结果保留两位小数），出勤率=出勤（在线时长10分钟及以上）人数 / 报名人数，输出结果按course_id升序排序，以上数据的输出结果如下：
|course_id|course_name|attend_rate(%)|
|---|---|---|
|1|Python|75.00|
|2|SQL|60.00|
|3|R|66.67|

```sql
select a.course_id, course_name, round(100*a.cnt/b.cnt,2) from
(
    select course_id, count(distinct user_id) cnt from attend_tb
    where timestampdiff(minute,in_datetime,out_datetime) >= 10
    group by course_id 
) a 
left join
(
    select course_id, count(distinct user_id) cnt from behavior_tb
    where if_sign = 1 group by course_id
) b on a.course_id = b.course_id
left join course_tb c on a.course_id = c.course_id
order by a.course_id
```

SQL189 牛客直播各科目同时在线人数:

较难  通过率：35.23%  时间限制：1秒  空间限制：256M

描述

牛客某页面推出了数据分析系列直播课程介绍。用户可以选择报名任意一场或多场直播课。<br/>
已知课程表course_tb如下（其中course_id代表课程编号，course_name表示课程名称，course_datetime代表上课时间）：
|course_id|course_name|course_datetime|
|---|---|---|
|1|Python|2021-12-1 19:00-21:00|
|2|SQL|2021-12-2 19:00-21:00|
|3|R|2021-12-3 19:00-21:00|

上课情况表attend_tb如下（其中user_id表示用户编号、course_id代表课程编号、in_datetime表示进入直播间的时间、out_datetime表示离开直播间的时间）：
|user_id|course_id|in_datetime|out_datetime|
|---|---|---|---|
|100|1|2021-12-01 19:00:00|2021-12-01 19:28:00|
|100|1|2021-12-01 19:30:00|2021-12-01 19:53:00|
|101|1|2021-12-01 19:00:00|2021-12-01 20:55:00|
|102|1|2021-12-01 19:00:00|2021-12-01 19:05:00|
|104|1|2021-12-01 19:00:00|2021-12-01 20:59:00|
|101|2|2021-12-02 19:05:00|2021-12-02 20:58:00|
|102|2|2021-12-02 18:55:00|2021-12-02 21:00:00|
|104|2|2021-12-02 18:57:00|2021-12-02 20:56:00|
|107|2|2021-12-02 19:10:00|2021-12-02 19:18:00|
|100|3|2021-12-03 19:01:00|2021-12-03 21:00:00|
|102|3|2021-12-03 18:58:00|2021-12-03 19:05:00|
|108|3|2021-12-03 19:01:00|2021-12-03 19:56:00|

请你统计每个科目最大同时在线人数（按course_id排序），以上数据的输出结果如下：
|course_id|course_name|max_num|
|---|---|---|
|1|Python|4|
|2|SQL|4|
|3|R|3|

```sql
select course_id, course_name, max(cnt) from
(
        select test.course_id, course_name, sum(status)over(partition by course_name order by date_time, status desc) cnt from
    (
        select course_id, in_datetime as date_time, 1 as status from attend_tb
        union all
        select course_id, out_datetime, -1 as status from attend_tb
    ) test
    left join course_tb on test.course_id = course_tb.course_id
) temp
group by course_id, course_name
order by course_id
```

SQL190 某乎问答11月份日人均回答量:

简单  通过率：49.47%  时间限制：1秒  空间限制：256M

描述

现有某乎问答创作者回答情况表answer_tb如下（其中answer_date表示创作日期、author_id指创作者编号、issue_id表示问题id、char_len表示回答字数）：
|answer_date|author_id|issue_id|char_len|
|---|---|---|---|
|2021-11-01|101|E001|150|
|2021-11-01|101|E002|200|
|2021-11-01|102|C003|50|
|2021-11-01|103|P001|35|
|2021-11-01|104|C003|120|
|2021-11-01|105|P001|125|
|2021-11-01|102|P002|105|
|2021-11-02|101|P001|201|
|2021-11-02|110|C002|200|
|2021-11-02|110|C001|225|
|2021-11-02|110|C002|220|
|2021-11-03|101|C002|180|
|2021-11-04|109|E003|130|
|2021-11-04|109|E001|123|
|2021-11-05|108|C001|160|
|2021-11-05|108|C002|120|
|2021-11-05|110|P001|180|
|2021-11-05|106|P002|45|
|2021-11-05|107|E003|56|

请你统计11月份日人均回答量（回答问题数量/答题人数），按回答日期排序，结果保留两位小数，以上例子的输出结果如下：
|answer_date|per_num|
|---|---|
|2021-11-01|1.40|
|2021-11-02|2.00|
|2021-11-03|1.00|
|2021-11-04|2.00|
|2021-11-05|1.25|

```sql
select answer_date, 
round(count(issue_id)/count(distinct author_id),2)
from answer_tb
group by answer_date
order by answer_date
```

SQL191 某乎问答高质量的回答中用户属于各级别的数量:

中等  通过率：31.31%  时间限制：1秒  空间限制：256M

描述

现有某乎问答创作者信息表author_tb如下(其中author_id表示创作者编号、author_level表示创作者级别，共1-6六个级别、sex表示创作者性别)：
|author_id|author_level|sex|
|---|---|---|
|101|6|m|
|102|1|f|
|103|1|m|
|104|3|m|
|105|4|f|
|106|2|f|
|107|2|m|
|108|5|f|
|109|6|f|
|110|5|m|

创作者回答情况表answer_tb如下（其中answer_date表示创作日期、author_id指创作者编号、issue_id指问题编号、char_len表示回答字数）：
|answer_date|author_id|issue_id|char_len|
|---|---|---|---|
|2021-11-01|101|E001|150|
|2021-11-01|101|E002|200|
|2021-11-01|102|C003|50|
|2021-11-01|103|P001|35|
|2021-11-01|104|C003|120|
|2021-11-01|105|P001|125|
|2021-11-01|102|P002|105|
|2021-11-02|101|P001|201|
|2021-11-02|110|C002|200|
|2021-11-02|110|C001|225|
|2021-11-02|110|C002|220|
|2021-11-03|101|C002|180|
|2021-11-04|109|E003|130|
|2021-11-04|109|E001|123|
|2021-11-05|108|C001|160|
|2021-11-05|108|C002|120|
|2021-11-05|110|P001|180|
|2021-11-05|106|P002|45|
|2021-11-05|107|E003|56|

回答字数大于等于100字的认为是高质量回答，请你统计某乎问答高质量的回答中用户属于1-2级、3-4级、5-6级的数量分别是多少，按数量降序排列，以上例子的输出结果如下：
|level_cut|num|
|---|---|
|5-6级|12|
|3-4级|2|
|1-2级|1|

```sql
select case when author_level < 3 then '1-2级' when author_level < 5 then '3-4级' else '5-6级' end,
count(issue_id) num from answer_tb
left join author_tb on answer_tb.author_id = author_tb.author_id
where char_len >= 100
group by case when author_level < 3 then '1-2级' when author_level < 5 then '3-4级' else '5-6级' end
order by num desc
```

SQL192 某乎问答单日回答问题数大于等于3个的所有用户:

中等  通过率：34.32%  时间限制：1秒  空间限制：256M

描述

现有某乎问答创作者回答情况表answer_tb如下（其中answer_date表示创作日期、author_id指创作者编号、issue_id指回答问题编号、char_len表示回答字数）：
|answer_date|author_id|issue_id|char_len|
|---|---|---|---|
|2021-11-01|101|E001|150|
|2021-11-01|101|E002|200|
|2021-11-01|102|C003|50|
|2021-11-01|103|P001|35|
|2021-11-01|104|C003|120|
|2021-11-01|105|P001|125|
|2021-11-01|102|P002|105|
|2021-11-02|101|P001|201|
|2021-11-02|110|C002|200|
|2021-11-02|110|C001|225|
|2021-11-02|110|C002|220|
|2021-11-03|101|C002|180|
|2021-11-04|109|E003|130|
|2021-11-04|109|E001|123|
|2021-11-05|108|C001|160|
|2021-11-05|108|C002|120|
|2021-11-05|110|P001|180|
|2021-11-05|106|P002|45|
|2021-11-05|107|E003|56|

请你统计11月份单日回答问题数大于等于3个的所有用户信息（author_date表示回答日期、author_id表示创作者id，answer_cnt表示回答问题个数），以上例子的输出结果如下：
|answer_date|author_id|answer_cnt|
|---|---|---|
|2021-11-02|110|3|

注：若有多条数据符合条件，按answer_date、author_id升序排序。

```sql
select answer_date, author_id, count(*) from answer_tb
group by answer_date, author_id
having count(*) >= 3
order by answer_date, author_id
```

SQL193 某乎问答回答过教育类问题的用户里有多少用户回答过职场类问题:

中等  通过率：38.35%  时间限制：1秒  空间限制：256M

描述

现有某乎问答题目信息表issue_tb如下（其中issue_id代表问题编号，issue_type表示问题类型）：
|issue_id|issue_type|
|---|---|
|E001|Education|
|E002|Education|
|E003|Education|
|C001|Career|
|C002|Career|
|C003|Career|
|C004|Career|
|P001|Psychology|
|P002|Psychology|

创作者回答情况表answer_tb如下（其中answer_date表示创作日期、author_id指创作者编号、issue_id指回答问题编号、char_len表示回答字数）：
|answer_date|author_id|issue_id|char_len|
|---|---|---|---|
|2021-11-01|101|E001|150|
|2021-11-01|101|E002|200|
|2021-11-01|102|C003|50|
|2021-11-01|103|P001|35|
|2021-11-01|104|C003|120|
|2021-11-01|105|P001|125|
|2021-11-01|102|P002|105|
|2021-11-02|101|P001|201|
|2021-11-02|110|C002|200|
|2021-11-02|110|C001|225|
|2021-11-02|110|C002|220|
|2021-11-03|101|C002|180|
|2021-11-04|109|E003|130|
|2021-11-04|109|E001|123|
|2021-11-05|108|C001|160|
|2021-11-05|108|C002|120|
|2021-11-05|110|P001|180|
|2021-11-05|106|P002|45|
|2021-11-05|107|E003|56|

请你统计回答过教育类问题的用户里有多少用户回答过职场类问题，以上例子的输出结果如下：
|num|
|---|
|1|

```sql
select count(distinct author_id) from answer_tb
where issue_id like 'E%' and author_id in (select author_id from answer_tb
where issue_id like 'C%')
```

SQL194 某乎问答最大连续回答问题天数大于等于3天的用户及其对应等级:

较难  通过率：28.35%  时间限制：1秒  空间限制：256M

描述

现有某乎问答创作者信息表author_tb如下(其中author_id表示创作者编号、author_level表示创作者级别，共1-6六个级别、sex表示创作者性别)：
|author_id|author_level|sex|
|---|---|---|
|101|6|m|
|102|1|f|
|103|1|m|
|104|3|m|
|105|4|f|
|106|2|f|
|107|2|m|
|108|5|f|
|109|6|f|
|110|5|m|

创作者回答情况表answer_tb如下（其中answer_date表示创作日期、author_id指创作者编号、issue_id指回答问题编号、char_len表示回答字数）：
|answer_date|author_id|issue_id|char_len|
|---|---|---|---|
|2021-11-01|101|E001|150|
|2021-11-01|101|E002|200|
|2021-11-01|102|C003|50|
|2021-11-01|103|P001|35|
|2021-11-01|104|C003|120|
|2021-11-01|105|P001|125|
|2021-11-01|102|P002|105|
|2021-11-02|101|P001|201|
|2021-11-02|110|C002|200|
|2021-11-02|110|C001|225|
|2021-11-02|110|C002|220|
|2021-11-03|101|C002|180|
|2021-11-04|109|E003|130|
|2021-11-04|109|E001|123|
|2021-11-05|108|C001|160|
|2021-11-05|108|C002|120|
|2021-11-05|110|P001|180|
|2021-11-05|106|P002|45|
|2021-11-05|107|E003|56|

请你统计最大连续回答问题的天数大于等于3天的用户及其等级（若有多条符合条件的数据，按author_id升序排序），以上例子的输出结果如下：
|author_id|author_level|days_cnt|
|---|---|---|
|101|6|3|

```sql
select distinct b.author_id, author_level, max(cnt)over(partition by b.author_id) from
(
    select author_id, (answer_date-rn) ind, count(*) cnt from 
    (
        select answer_date, author_id, row_number()over(partition by author_id order by answer_date) rn
        from (select distinct answer_date, author_id from answer_tb) a
    ) test
    group by author_id, ind
    having cnt >= 3
) b left join author_tb on b.author_id = author_tb.author_id
```
