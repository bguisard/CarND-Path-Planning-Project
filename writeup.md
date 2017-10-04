# Path Planning
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 50 m/s^3.


## The Model

In this model I used the same simplifications that were suggested in the learning material. Instead of directly controlling the vehicle's acceleration we slowly change the reference speed up or down as needed. We also use a spline to smooth our path, thus eliminating latidutinal jerk warnings.

### Using Frenet Coordinates

Cartesian coordinates are the usual way we think about coordinates in a map, generally using x and y coordinates to locate an object. These coordinates are useful when we are looking at a map from above, but if you are on the road, it is more natural to think about lanes and distances from cars in each lane.

Frenet coordinates work exactly like that, we take the middle of the road as y axis and define *d* as the distance from this axis, meaning that cars that have their d values within a certain range are in the same lane. We call *s* the distance alongside *d*, that way you can know exactly how far a vehicle in your lane is just by comparing *s*.

### Simplified Heuristics

For this toy problem I used a simple heuristic that our vehicle will attempt to move lanes every time it sees a slower car in our lane within 30m distance. The vehicle is only allowed to shift lanes if the gap in the adjacent lane is 60m. (30m ahead and 30m behind) - this is far from the most efficient implementation, but works as a good starting point for our project.

This Path Planning model takes a set of reference points in Cartesian coordinates and converts them to Frenet coordinates. It will then generate evenly spaced (30m) points ahead of the starting reference points and using the map waypoint coordinates that were provided.

A spline interpolation was used to ensure a smooth transition between points, this was useful to avoid lateral jerk warnings.

After building the path curve we just need to break it down in segments that allows us to travel between these points using a reference speed. The speed is initially set at 0 and to avoid acceleration jerk warnings we accelerate at a constant rate of 0.224 mph per 1/50 of a second.

## Final Considerations

This simple heuristics are sufficient for a safe behavior of our autonomous vehicle, but it's not the most efficient way of implementing a path planner. Ideally we would have an MPC and cost function fine tuned to ensure a smooth and safe drive.