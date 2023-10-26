SQL156:
简单  通过率：16.95%  时间限制：1秒  空间限制：256M
描述
用户-视频互动表tb_user_video_log
id	uid	video_id	start_time	end_time	if_follow	if_like
if_retweet
comment_id
1	101	2001	2021-10-01  10:00:00	2021-10-01  10:00:30
0	1	1	NULL
2	102
2001
2021-10-01  10:00:00
2021-10-01  10:00:24
0	0	1	NULL
3	103
2001
2021-10-01  11:00:00
2021-10-01  11:00:34
0	1	0	1732526
4	101
2002
2021-09-01  10:00:00
2021-9-01  10:00:42
1	0	1	NULL
5	102
2002
2021-10-01  11:00:00
2021-10-01  10:00:30
1	0	1	NULL
（uid-用户ID, video_id-视频ID, start_time-开始观看时间, end_time-结束观看时间, if_follow-是否关注, if_like-是否点赞, if_retweet-是否转发, comment_id-评论ID）

短视频信息表tb_video_info
id	video_id	author	tag	duration	release_time
1	2001	901	影视	30	2021-01-01 07:00:00
2	2002	901
美食	60	2021-01-01 07:00:00
3	2003	902
旅游	90	2021-01-01 07:00:00
（video_id-视频ID, author-创作者ID, tag-类别标签, duration-视频时长（秒）, release_time-发布时间）

问题：计算2021年里有播放记录的每个视频的完播率(结果保留三位小数)，并按完播率降序排序
注：视频完播率是指完成播放次数占总播放次数的比例。简单起见，结束观看时间与开始播放时间的差>=视频时长时，视为完成播放。

输出示例：
示例数据的结果如下：
video_id	avg_comp_play_rate
2001	0.667
2002	0.000
解释：
视频2001在2021年10月有3次播放记录，观看时长分别为30秒、24秒、34秒，视频时长30秒，因此有两次是被认为完成播放了的，故完播率为0.667；
视频2002在2021年9月和10月共2次播放记录，观看时长分别为42秒、30秒，视频时长60秒，故完播率为0.000。

```sql
select a.video_id, 
round(sum(case when timestampdiff(second,start_time,end_time) >= duration then 1 else 0 end)/count(a.video_id),3) rate from tb_user_video_log a 
left join tb_video_info b on a.video_id = b.video_id
where year(start_time) = 2021
group by a.video_id
order by rate desc
```
