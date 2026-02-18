# Traffic Light FSM Controller

To illustrate the design of FSMs, consider the problem of inventing a controller for a traffic light at a busy intersection on campus. Engineering students are moseying between their dorms and the labs on Academic Ave. They are busy reading about FSMs in their favorite textbook and aren't looking where they are going. Football players are hustling between the athletic fields and the dining hall on Bravado Boulevard. They are tossing the ball back and forth and aren't looking where they are going either. Several serious injuries have already occurred at the intersection of these two roads, and the Dean of Students asks Ben Bitdiddle to install a traffic light before there are fatalities.

Ben decides to solve the problem with an FSM. He installs two traffic sensors, T<sub>A</sub> and T<sub>B</sub>, on Academic Ave. and Bravado Blvd., respectively. Each sensor indicates TRUE if students are present and FALSE if the street is empty. He also installs two traffic lights, L<sub>A</sub> and L<sub>B</sub>, to control traffic. Each light receives digital inputs specifying whether it should be green, yellow, or red. Hence, his FSM has two inputs, T<sub>A</sub> and T<sub>B</sub>, and two outputs, L<sub>A</sub> and L<sub>B</sub>. Ben provides a clock with a 5-second period. On each clock tick (rising edge), the lights may change based on the traffic sensors. He also provides a reset button so that Physical Plant technicians can put the controller in a known initial state when they turn it on.

![FSM Diagram](https://github.com/user-attachments/assets/4f438058-5648-4baa-a243-408fc238a144)

## Inputs & Clock

| Signal | Description |
|--------|-------------|
| T<sub>A</sub> | Traffic present on Academic Ave. (`1` = present, `0` = empty) |
| T<sub>B</sub> | Traffic present on Bravado Blvd. (`1` = present, `0` = empty) |
| Clock  | 5-second period; state transitions occur on each rising edge |

## State Transition Summary

On **reset**, the FSM transitions to **S0** and the lights are initialized to their default state. The state transitions proceed as follows:

- **S0** — If T<sub>A</sub> = 1 (traffic present on Academic Ave.), remain in S0. If T<sub>A</sub> = 0 (no traffic), transition to **S1**.
- **S1** — On the next clock edge, unconditionally transition to **S2**, regardless of traffic sensor values.
- This pattern continues through subsequent states until the FSM cycles back to **S0**.
