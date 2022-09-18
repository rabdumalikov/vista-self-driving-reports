
# Report 4:

-----

**curvature**: `/ssc/curvature_feedback.curvature`

**speed**: `/ssc/velocity_accel_cov.velocity`

**frequence:** 30Hz

<table style="width:100%">
  <tr>
    <th>Ground-Truth:</th>
    <th>PilotNet Nvidia:</th>
  </tr>
  <tr>
    <td> <div align="center">
  <a href="https://www.youtube.com/watch?v=bZELpneieKk"><img src="https://img.youtube.com/vi/bZELpneieKk/0.jpg" alt="How we started"></a>
</div> </td>
    <td> <div align="center">
  <a href="https://www.youtube.com/watch?v=IrB3lfJ73Bw"><img src="https://img.youtube.com/vi/IrB3lfJ73Bw/0.jpg" alt="How we started"></a>
</div></td>
  </tr>
</table>

-----

**yaw rate**: `/current_velocity.twist.angular.z`

**speed**: `/current_velocity.twist.linear.x`

**frequence:** 50Hz

<table style="width:100%">
  <tr>
    <th>Ground-Truth:</th>
    <th>PilotNet Nvidia:</th>
  </tr>
  <tr>
    <td> <div align="center">
<a href="https://www.youtube.com/watch?v=yw5W_m6XDOw"><img src="https://img.youtube.com/vi/yw5W_m6XDOw/0.jpg" alt="How we started"></a>
</div> </td>
    <td> <div align="center">
<a href="https://www.youtube.com/watch?v=dvgowutxJ14"><img src="https://img.youtube.com/vi/dvgowutxJ14/0.jpg" alt="How we started"></a>
</div></td>
  </tr>
</table>

-----

## Summary:

|duration|mean|std|speed|curvature|yaw_rate|frequency|
|---|---|---|---|---|---|---|
|3:27|3:26|0.4|`/ssc/velocity_accel_cov.velocity`| - |`/ssc/curvature_feedback.curvature`|30|
|3:15|3:18|1.6|`/current_velocity.twist.linear.x`|`/current_velocity.twist.angular.z`| - |50|
