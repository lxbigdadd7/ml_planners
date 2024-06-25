## Author Table
Author | Date  
------ | -----  
Xiang LI | 20240625 

## Content
Here I listed all the features and their dimensions required by plantf model.

### planTF features (unnormalized)
- egostates: 
  - ego_cur_state(1x7)
    - rear_axle.x
    - rear_axle.y
    - rear_axle.heading
    - rear_axle.rear_axle_velocity_2d.x (rear-velocity is in local frame)
    - rear_axle_acceleration_2d.x
    - steering_angle (deduced)
    - yaw_rate (deduced)

  - ego_states (dict)
    - position (FRAME_NUM x 2)(rear)(global): x, y
    - heading (FRAME_NUM x 1)(rear)(global)
    - velocity (FRAME_NUM x 2)(rear)(global): vx, vy
    - acceleration (FRAME_NUM x 2)(rear)(global): ax, ay
    - shape (FRAME_NUM x 2): width, length
    - category (FRAME_NUM x 1): int index of several type
    - valid_mask (FRAME_NUM x 1): all true

- agentstates  

  - agent_features (max agent num is 64)(closer agent lower indice)
    - position (CURRENT_NUM_AGENTS x FRAME_NUM X 2)(center)(global): x, y
    - heading (CURRENT_NUM_AGENTS x FRAME_NUM x 1)(center)(global)
    - velocity (CURRENT_NUM_AGENTS x FRAME_NUM x 2)(center)(global): vx, vy
    - acceleration (CURRENT_NUM_AGENTS x FRAME_NUM x 2)(global): ax, ay
    - shape (CURRENT_NUM_AGENTS x FRAME_NUM x 2): width, length
    - category (CURRENT_NUM_AGENTS x 1): int index of several type
    - valid_mask (CURRENT_NUM_AGENTS x FRAME_NUM): only filled data is true

  - "agent": concat of ego_states and agent_features, ego is index 0 in axis 0.

- map (include lanes, crosswalk and traffic lights; crosswalk is treated as lane)
  - point_position (NUM_MAP_OBJS x 3 x SAMPLES x 2): 3 denotes centerline, left boundary, right boundary. 2 denotes xy.
  - point_vector (NUM_MAP_OBJS x 3 x SAMPLES x 2): vector start at point position.
  - point_side = (NUM_MAP_OBJS x 3): 0-center, 1-left, 2-right. Normally it's arange(3).
  - point_orientation (NUM_MAP_OBJS x 3 x SAMPLES): vector orientation
  - polygon_center (NUM_MAP_OBJS x 3): 3 denotes middle and orientation of centerline.
  - polygon_position (NUM_MAP_OBJS x 2): first point of centerline.
  - polygon_orientation (NUM_MAP_OBJS): orientation of first point of centerline.
  - polygon_type (NUM_MAP_OBJS): type of this map object,which is lane, laneconnector or crosswalk.
  - polygon_on_route (NUM_MAP_OBJS): if this lane is on roadblock included in route.
  - polygon_tl_status (NUM_MAP_OBJS): TrafficLightStatusType
  - polygon_speed_limit (NUM_MAP_OBJS): speedlimit of lane
  - polygon_has_speed_limit (NUM_MAP_OBJS): if lane has speedlimit.

### planTF features (normalized, from global to local)

- ego_cur_state(1x7)
    - rear_axle.x -> 0
    - rear_axle.y -> 0
    - rear_axle.heading -> 0

- ego_states, agent_features:
    - position (global) -> (local)
    - heading (global) -> (local)
    - velocity (global) -> (local)

- map:
  - point_position (global) -> (local)
  - point_vector (global) -> (local)
  - point_orientation (global) -> (local)
  - polygon_center (global) -> (local)
  - polygon_position (global) -> (local)
  - polygon_orientation (global) -> (local)
