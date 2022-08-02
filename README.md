# Report:

# Progress:

* Transformation calculation: I computed new **position** and **rpy** based on transforms from platform.urdf

  ```py
  lidar_center_pos = ...
  lidar_center_rpy = ...

  camera_pos = ...
  camera_rpy = ...

  lid_trans = vec2mat(lidar_center_pos, lidar_center_rpy)
  cam_trans = vec2mat(camera_pos, camera_rpy)

  # Also tried np.matmul(cam_trans, lid_trans) 
  trans = np.matmul(lid_trans, cam_trans)

  new_pos, new_rpy = mat2vec(trans)

  quat = euler2quat(new_rpy)
  ```
  I've got following quaternions `[-0.49737151, 0.51789681, -0.49125537, 0.49302397]` and following position `[ 1.93375348, -0.149484, 1.37534]`. Instead of **yaw** translations I've got **pitch** translations.

  <a href="https://www.youtube.com/watch?v=7_AoA41ANNQ"><img src="https://img.youtube.com/vi/7_AoA41ANNQ/0.jpg" alt="How we started"></a>

  Then I tried vista's position `[0, 1.7653, 0]`. Now I've got **roll** translations: 
  
  <a href="https://www.youtube.com/watch?v=WZKNWGx2gcg"><img src="https://img.youtube.com/vi/WZKNWGx2gcg/0.jpg" alt="How we started"></a>

  Then I tried relative link2 position `[0.91965, -0.1074, -0.412]`. And again I've got **roll** translations:  
  
  <a href="https://www.youtube.com/watch?v=GkcfCHTdTkI"><img src="https://img.youtube.com/vi/GkcfCHTdTkI/0.jpg" alt="How we started"></a>

  Then I tried relative vista's quaternions `[0.0, 0.0, -0.0436194, 0.9990482]` and our position `[1.93375348, -0.149484, 1.37534]`. 
  I've got distornions only at the top half:
  
  <a href="https://www.youtube.com/watch?v=OcqD_h5zPlU"><img src="https://img.youtube.com/vi/OcqD_h5zPlU/0.jpg" alt="How we started"></a>

  Then I tried relative link2 position `[0.91965, -0.1074, -0.412]`. And again I've got **roll** translations:  
  
  <a href="https://www.youtube.com/watch?v=S2SIo50avL8"><img src="https://img.youtube.com/vi/S2SIo50avL8/0.jpg" alt="How we started"></a>

  Then I tried vista's position `[0, 1.7653, 0]`. Now I've got **roll** translations:
  
  <a href="https://www.youtube.com/watch?v=99MkjcpV984"><img src="https://img.youtube.com/vi/99MkjcpV984/0.jpg" alt="How we started"></a>

  Somehow it seem that the most correct **quaternions** and **positions** are vista ones. But when I try to train with vista's **quaternions** and **positions** dispite the number of episodes the training platuing around ~100. I tried to compare transformation during training, and in vista's case transformation strictly left or right, however in our training transformation in **yaw** direction.
  
<table style="width:100%">
  <tr>
    <th>Vista training</th>
    <th>Our training</th>
  </tr>
  <tr>
    <td> <div align="center">
  <a href="https://www.youtube.com/watch?v=ufXnJpMp_A4"><img src="https://img.youtube.com/vi/ufXnJpMp_A4/0.jpg" alt="Vista"></a>
</div> </td>
    <td> <div align="center">
  <a href="https://www.youtube.com/watch?v=qO91C5Ry_KA"><img src="https://img.youtube.com/vi/qO91C5Ry_KA/0.jpg" alt="Ours"></a>
</div></td>
  </tr>
</table>

I accidently found parameters for our width and height in vista [repo](https://github.com/vista-simulator/vista/tree/1132f711c0889f9778a93efef32ca292576cc424), and tried them. Parameters such as `distortion`, `cx`, `cy`, `fx` and `fy` are really close, but their `quaternion` and `position` are different. In my experiments I tried a lot of variations for `position` and `quaternion` and in order to see the blue line and transformations, in parameter `position` y-value should be ~1.5, for `quaternion` the last value should be ~0.99. Otherwise I either wasn't able to see blue line or transformations were wrong or distortion was too severe. 

```
 <CAMERA Name="camera_front" Type="gmsl">
    <PROPERTY Name="Model" Value="pinhole"/>
    <PROPERTY Name="Quaternion" Value="0, 0, -0.035265025029250387, 0.9996807883070832"/>
    <PROPERTY Name="Position" Value="0, 1.6, 0"/>
    <PROPERTY Name="Yaw" Value="0.05"/>
    <PROPERTY Name="params" Value=""/>
    <PROPERTY Name="width" Hint="intrinsic" Value="1920"/>
    <PROPERTY Name="height" Hint="intrinsic" Value="1208"/>
    <PROPERTY Name="distortion" Hint="intrinsic" Value="-0.29318516, 0.06609604, -0.00103823, -0.0016309, 0."/>
    <PROPERTY Name="cx" Hint="intrinsic" Value="982.06345972"/>
    <PROPERTY Name="cy" Hint="intrinsic" Value="617.04862261"/>
    <PROPERTY Name="fx" Hint="intrinsic" Value="942.21001829"/>
    <PROPERTY Name="fy" Hint="intrinsic" Value="942.91483399"/>
    <PROPERTY Name="roi" Hint="region_of_interest" Value="590, 520, 950, 1440"/>
    <PROPERTY Name="roi_angle" Hint="angle of ROI" Value="0"/>
 </CAMERA>
```

Results:

<a href="https://www.youtube.com/watch?v=ozMPMzn5cEw"><img src="https://img.youtube.com/vi/ozMPMzn5cEw/0.jpg" alt="How we started"></a>

* I also wrote an email to Alexandle Amini, but he didn't respond yet. I also found that 
* They are using `max_curvature` as hyperparameter, so I found this value in the rosbag which equal to `0.15`. Used that but didn't get visible improvements.

# Problem: 
* In vista they are specifying `road width` to identify when the car outside the road. In their dataset the road width is the same. However, in our case width of the road varies. On top of that, there are moments when our car not going along the center of the road. This might be potential problem for us as well.
* Top view in our and vista case differs. For some reason, in our case the top view show that the road always straight, however in vista case the shape of the road is accurate. This also might be source of problems. 
* Here they are specifying `wheel_base` and `steering_ratio`. Do you think it is important?
  ```
  car = world.spawn_agent(
    config={
        'length': 5.,
        'width': 2.,
        'wheel_base': 2.78,
        'steering_ratio': 14.7,
        'lookahead_road': True
    })
  ```

# Plan: 
* **Clarification:** I think it would be better to create an issue on their repo, and clarify all the details there.
* **Top view:** Find out why in our case the shape of the road always straight.
* **Road width:** Find a segment where the width is fixed, and try training with such dataset.

