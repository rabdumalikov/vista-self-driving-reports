
-----

# Report 2:

# Progress:
I did manage to fix the shape of the road. The problem was with `yaw_rate` in this topic `/novatel/oem7/corrimu`. The values there are wrong. I found this formula `curvature = yaw_rate / speed` how vista computes curvature, so I could rearrange it in order to compute **yaw_rate**. The thing left was to find the correct **curvature**. I found it in the following topic `/ssc/arbitrated_steering_commands`. After I computed the correct **yaw_rate** the shape of the road was fixed. In the video below, for demonstration purpose I'm using ground truth curvature: 

<a href="https://www.youtube.com/watch?v=kzDO--4uQsI"><img src="https://img.youtube.com/vi/kzDO--4uQsI/0.jpg" alt="How we started"></a>

Even after the fix, the training still wasn't successful. The reward slowly grows for around 200 episodes but then undergoes a rapid drop. To tackle this problem, I started experimenting with parameters with the original vista dataset. What I found out was that the training is less sensitive for parameter `position` and `yaw` i.e. the agent still able to learn a good control policy. But with parameter `quaternions` situation is different even slight changes in those values and the agent fails to learn control policy moreover, the training is similar to what I experienced with our dataset. Basicaly in both cases the agent immediately kill itself.

Then I decided to compare driving with both ground truth. I noticed that the agent drifts away from the road with ground truth data. But it also happening with vista dataset.

<a href="https://www.youtube.com/watch?v=WlENp6FVmug"><img src="https://img.youtube.com/vi/WlENp6FVmug/0.jpg" alt="How we started"></a>
<a href="https://www.youtube.com/watch?v=gF9ZoxnG9u8"><img src="https://img.youtube.com/vi/gF9ZoxnG9u8/0.jpg" alt="How we started"></a>

Here ground truth drive with `link3`, which also eventually drifts away from the road.
<a href="https://www.youtube.com/watch?v=e9KT1KTKebg"><img src="https://img.youtube.com/vi/e9KT1KTKebg/0.jpg" alt="How we started"></a>

# Problem:

Seems like drifting away from the road is not a problem. But `quaternions` values seem to be a major source of problems. Since training is so sensitive that even fails with the vista dataset.

# Plan:
I created an [issue](https://github.com/vista-simulator/vista/issues/12) in vista repository regarding "How to computer quaternions?".
We use quaternions for rotation. And for me, it is a mystery why even a slight deviation affects the training. Maybe you have additional ideas or suggestions?
