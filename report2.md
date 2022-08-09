
-----

# Report 2:

# Progress:
I did manage to fix the shape of the road. The problem was with `yaw_rate` in this topic `/novatel/oem7/corrimu`. The values there are wrong. I found this formula `curvature = yaw_rate / speed` how vista computes curvature, so I could rearrange it in order to compute **yaw_rate**. The thing left was to find the correct **curvature**. I found it in the following topic `/ssc/arbitrated_steering_commands`. After I computed the correct **yaw_rate** the shape of the road was fixed. In the video below, for demonstration purpose I'm using ground truth curvature: 

<a href="https://www.youtube.com/watch?v=kzDO--4uQsI"><img src="https://img.youtube.com/vi/kzDO--4uQsI/0.jpg" alt="How we started"></a>

Even after the fix, the training still wasn't successful. The reward slowly grows for around 200 episodes but then undergoes a rapid drop. To tackle this problem, I started experimenting with parameters with the original vista dataset. What I found out was that the training is less sensitive for parameter `position` and `yaw` i.e. the agent still able to learn a good control policy. But with parameter `quaternions` situation is different even slight changes in those values and the agent fails to learn control policy moreover, the training is similar to what I experienced with our dataset. Basicaly in both cases the agent immediately kill itself.

Then I decided to compare driving with both ground truth.

<a href="https://www.youtube.com/watch?v=e9KT1KTKebg"><img src="https://img.youtube.com/vi/e9KT1KTKebg/0.jpg" alt="How we started"></a>


Attempt with pos=`0, 1.375, 0`, quaternions=`0, 0, 0, 1`, and yas=`1`:

<a href="https://www.youtube.com/watch?v=tBy-dTc7zgI"><img src="https://img.youtube.com/vi/tBy-dTc7zgI/0.jpg" alt="How we started"></a>
