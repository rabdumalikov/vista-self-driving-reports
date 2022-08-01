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

# Plan: 

# Problem: 
