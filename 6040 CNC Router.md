## Introduction

The CNC machine is a 6040 CNC, which simply means that the bed is 60 cm
long and 40 cm wide. This is a standard architecture that is made by
many manufacturers.

It is a 3-axis machine, with the axes as shown below. The Y is the
longest axis, along the bed, X is across the bed, and Z is the vertical
axis. The origin is near the front, left hand corner.

There is a stepper motor for each axis and a Pulse Width Modulation
(PWM) spindle motor that drives the cutting tool. In addition, there are
limit switches at the ends of some of the axes to prevent the motors
trying to drive the head past the limits.

![A machine with a red line Description automatically
generated](media/image2.png){width="5.236805555555556in"
height="5.236805555555556in"}

The second component is the Controller, which contains the driver
electronics and the connections through to the 3 stepper motors.

The PC is connected to the Controller via a parallel cable. In this
configuration the signals that drive the stepper motors are sent via the
parallel cable. The PC does most of the synchonisation and timing of
these signals to control the speed and direction of all the motors and
sense the limit switches.

More modern CNCs typically use an intelligent Controller, typically
running GRBL on an Arduino. Such a Controller does all of the timing,
and the PC just sends commands via a USB or WiFi connection.

## Communicating with the CNC

CNCs receive instructions via G-Code, which is a low-level command
language, similar in status to an assembly language. Normally you don't
need to know much about G-Code, except when things go wrong.

Typical G-Code instructions are:

~~~ gcode
G00 X3 Y4 Z0           - move the head to the coordinates (3, 4, 0)
G01 X0.33 Y54.68 Z0.1  - cut from the current position to coordinates (0.33, 54.68, 0.1)
G92 X0 Y0 Z0           - set the current working coordinates to (0, 0, 0)
M30                    - stop the G-Code program and rewind to the beginning
~~~

There are dozens of G-Code instructions to control every aspect of CNC,
3D printing, robots and laser engraving machines. See [*All3DP G-Code
Commands*](https://all3dp.com/2/3d-printer-g-code-commands-list-tutorial/) for a
more detailed introduction to G-Code.

All coordinates are in millimetres.

## Turning on the CNC

1.  Turn on the power switch and check the status of the CNC Controller.

    -   The DRIVE switch should be turned on, and lit. It is turned on
        in the UP position.

    -   The Emergency button may be depressed. To ensure that it is not
        depressed, rotate it clockwise until it either clicks out, or
        can't be turned any further.\
        \
        **The Emergency button is the Panic button. When something goes
        wrong, press it to immediately stop the CNC.**\
        \
        Once pressed, the Emergency button will remain pressed until it is released by turning it.

![A blue machine with buttons and a
button](media/image3.jpeg){width="2.1875in"
height="2.9167366579177605in"}

2.  Start the PC by pressing the On button. The PC will boot into
    LinuxCNC, which is a version of Linux with a Real Time kernel to
    ensure that it meets the timing demands of communication with the
    CNC.

	Login into the PC using the username/password: `hacker/cnc`

3.  Run the CNC interface software, which is confusingly called
    LinuxCNC, by clicking on the "Launch CNC6040" icon. This will
    display the LinuxCNC user interface. There are many interfaces to
    the LinuxCNC system -- this version is the original, called Axis.

    ![](media/image4.png){width="7.672520778652668in" height="5.625in"}

    The interface has the following components:

    -   Top row menu for File handling, Machine control and changing the
        Preview panel.
    -   A list of command buttons, discussed below
    -   A Manual Control panel that allows the user to Home the CNC, jog the
        head, and set the spindle speed
    -   A Manual Data Interface (MDI) panel (currently hidden) that allows
        the user to directly enter and execute G-Code commands
    -   A list of G-Code options that are currently enabled on the CNC
    -   Near the bottom, a list of G-Code that is currently being executed.
        This is scrolled up as G-Code commands are executed.
    -   In the centre, the Preview Panel which graphically shows the tool
        path of the currently loaded G-Code program.
    -   At the top of the Preview Panel are some positional data, given the
        current coordinates of the tool head on the CNC. This positional
        data is also shown in the Digital Read-Out panel, which is currently
        hidden.
    -   At the bottom, below the Preview Panel, the Position status is
        shown: as using either Machine or Work Coordinates -- these will be
        discussed further below.
    -   On the right a panel with:
        -   The current spindle speed. This speed can either be set
            manually, or by G-Code commands;
        -   A Clear Offsets button that sets the machine coordinates to
            known values;
        -   A Clear Job Coordinates which resets the current Work
            Coordinates;
        -   A Touch Off Z button which resets the Z coordinate of the tool
            head to zero.

    You may see a dialogue box that complains about not being able to see
    the Pendant -- ignore this because we are not using the Pendant.

![A screenshot of a computer Description automatically
generated](media/image5.png){width="2.2916666666666665in"
height="1.2065671478565179in"}

4.  Before using the CNC interface you need to click on the Emergency
    Stop (E-Stop) button ![](media/image6.emf)
    ![](media/image7.png){width="0.27774278215223097in"
    height="0.27774278215223097in"} in the top left corner. This is the
    software equivalent of the Emergency Stop button on the controller,
    and will stop any existing activity.

5.  Next click on the Machine Power button
    ![](media/image8.png){width="0.2916305774278215in"
    height="0.2916305774278215in"} near the top left corner. This
    effectively turns the CNC interface on and enables the buttons under
    Manual Control \> Axis & Spindle. The CNC interface is now ready to
    use.

## Coordinate Systems

CNC machines have several coordinate systems, depending on their mode of
use.

The **machine coordinates** are the absolute coordinates that the
machine uses for the whole machine. They are set at start up, and
Homing. The extent of these machine coordinates closely match the extent
of movement of the head along each of the axes.

The **work coordinates** are normally set for each job. The user needs
to decide where the zero origin is for the work job, then move the head
to that position and zero the machine by setting the work coordinates to
zero for that position. All subsequent work on that job is done relative
to these work coordinates.

G-Code also has the ability to set and unset multiple **subsets of work
coordinates** for sub-parts of a job. This is normally down when the job
requires multiple identical sub-parts.

The status bar at the bottom of the LinuxCNC interface shows the
coordinate system being used:

- **Position: Machine Actual** The CNC is using the machine coordinates
- **Position: Relative Actual** The CNC is using work coordinates.

The respective G-Code commands are:

~~~ gcode
G90                    - Use absolute machine coordinates for the XYZ axes
G91                    - Use relative work coordinates for the XYZ axes
~~~

## Digital Read-Out (DRO)

An important part of the LinuxCNC display is the Digital Read-Out, which
displays the current coordinates of the tool head in the X, Y, and Z
axes. This is essential information to understand the progress of the
CNC milling, and to diagnose when problems occur, such as hitting limit
switches, or making unwanted cuts.

The DRO is constantly updated and displayed as millimetres to 3 decimal
places.

By switching to the DRO panel the user can see the DRO data, without the
underling Preview panel graphics.

## Homing

Stepper motors don't have origin -- they just rotate forwards or
backwards relative to their previous position.

When the CNC is turned on, its machine coordinates are (0, 0, 0). This
can be anywhere in the CNC tool space.

To standardise the work coordinates the machine needs to be **Homed** to
establish a standard position for the origin of the machine coordinates.
This is done by moving the tool head to limit switches whose positions
are known on each of the axes.

At present LinuxCNC has been configured to home to zero on the X axis,
the far limit on the Y axis (coordinate 585), and the upper limit on the
Z axis.

1.  Ensure that the bed of the CNC is free from any obstruction or
    clutter.
2.  Click on the Home All button.\
    The head should now start to each of the limit switches in the order
    Z, X, Y. As it does so, it will update the coordinates in DRO
    display of LinuxCNC screen.
3.  Click on the **Clear Offsets** button. This sets the machine
    coordinates to known values that correspond to the positions of the
    limit switches on each of the axes.

	![](media/image9.png){width="7.268055555555556in" height="5.9319444444444445in"}

	Unfortunately, this leaves the head at the positive end of the Y axis.

    In the DRO panel the machine coordinates are set to the pre-defined
    positions of the limit switches (0, 595,58).

    The Distance-To-Go (DTG) coordinates are set for each move, and indicate
    the distance that the current move needs to make along each axis before
    completion of the current move.

    The G54 coordinates are the part being processed. The coordinates are
    relative to the machine coordinates.

    The G92 coordinates are the work coordinates, relative to the work
    origin, defined by the G92 command.

    The Tool Length Offset (TLO) coordinates are the position of the tool
    tip, relative to the spindle.

## Jogging

Jogging is a manual process by which the user can move the head in
either direction along each of the axes. It is normally used to position
the head for the next job.

Jogging can be done either step-by-step or continuously. The user can
only jog along one axes at a time.

To jog the head:

1.  Select the axis to be jogged:

	![](media/image10.png){width="2.7079943132108486in" height="0.26385608048993875in"}

2.  Select either **Continuous** or step mode, by selecting one of the
    options, 5mm, 1mm, 0.5mm, 0.1mm, 0.05mm, 0.01mm or 0.005mm.

3.  Click on either the **+** or **--** buttons to jog the head in the
    required direction. You will notice the coordinates change in the
    upper right window as the head is moved.

> In Step mode the head will move one step at the specified length and
> direction on each click of the **+** or **--** button.
>
> In Continuous mode, the head will move in the specified direction as
> long as the **+** or **--** button is held down. This is normally for
> gross movement of the head over long distances.

## Spindle Speed

The spindle speed can be set manually or in G-Code by the G96 or G97
commands.

To set the spindle speed, use the buttons under **Spindle:

![](media/image11.png){width="2.1941699475065617in"
height="0.666583552055993in"}

To start the spindle rotating in the clockwise direction click
![](media/image12.emf)![](media/image13.png){width="0.31940398075240595in"
height="0.26385608048993875in"}, and then select the speed by clicking
on the **+** or **--** buttons.

To start the spindle rotating in the anti-clockwise direction (for a
left-head threaded bit) click
![](media/image14.png){width="0.3055172790901137in"
height="0.2916305774278215in"} and then select the speed.

To stop the spindle, click on the **Stop** button.

The spindle speed will be determined by:

-   The type of stock material -- plastics and polymers require must
    lower speeds than wood or aluminium, otherwise they melt

-   The type of tools -- some tool bits are designed to operate at
    higher or lower speeds than others

-   The type of finish -- depending on the type of stock material,
    different speeds will give different quality results

The current spindle speed is shown in the top right of the interface
screen.

![A close up of a sign Description automatically
generated](media/image15.png){width="1.9764238845144357in"
height="1.3593121172353455in"}

## Preview Screen

The Preview Screen in the centre of the LinuxCNC interface shows a
preview of the tool paths when a G-Code program is loaded.

Non-cutting moves (G0 commands) are shown as cyan lines.

Cutting moves (G1 commands) are shown as solid white lines.

The positions of Pauses or Dwells (G4 commands) are shown as small pink
X marks.

Non-cutting moves (G0 commands) prior to the first cut, and after a tool
change, but before he next cut are not shown.

The maximum extents or dimensions of the G-Code program in each axis
direction are shown as pink lines with the lengths in mm shown near the
middle of these lines. If any of these extents exceed a limit of the
CNC, they are shown in a red rectangle, and will also cause an error
message when the program is run.

As the program is run and the tool head moves, it leaves a back-plot
trace:

  -----------------------------------------------------------------------
     Manual jogs    Non-cutting Moves   Straight Cuts     Circular Cuts
  ----------------- ----------------- ----------------- -----------------
       Yellow             Green              Red             Magenta

  -----------------------------------------------------------------------

The tool head is shown as a white, inverted cone.

![](media/image16.png){width="4.406626202974628in"
height="3.5324857830271217in"}

The Preview display can be manipulated by the following commands:
![](/img/preview-sqmqtehl2yaev3drrlg7vdl4gqxf.svg){width="50%"}

> ![Zoom In](media/image17.png){width="0.22916666666666666in"
> height="0.22916666666666666in"} Zoom In
>
> ![](media/image1.png) Zoom Out
>
> ![Top view](media/image18.png){width="0.25in" height="0.25in"} Top
> view, looking down the Z axis, for viewing the XY plane from above.
>
> ![Rotated Top view](media/image19.png){width="0.25in" height="0.25in"}
> Rotated Top view, looking down the Z axis, with the X & Y axes rotated
> through 90 degrees, for viewing the rotated XY plane from above.
>
> ![Side view](media/image20.png){width="0.25in" height="0.25in"} Side
> view, looking from the positive end of the X axis, for viewing the YZ
> plane from above.
>
> ![Front view](media/image21.png){width="0.25in" height="0.25in"} Front
> view, looking from the origin end of the Y axis, for viewing the XZ
> plane from below.
>
> ![Perspective view](media/image22.png){width="0.28125in"
> height="0.25in"} Perspective view (default), adjustable using the
> mouse and Drag/Rotate toggle selector
> ![](media/image23.png){width="0.3332917760279965in"
> height="0.3055172790901137in"}
>
> ![Clear live backplot](media/image24.png){width="0.25in"
> height="0.25in"} Clear the back-plot trace \[Ctrl-K\].

## Setting up a Job

When a job has been designed (CAD) and converted to G-Code (CAM), it is
ready to be machined.

It is assumed that the tool bit has been chosen and loaded into the
chuck / collet of the CNC, and that the stock material has been chosen
and shaped for the job.

1.  **Clamping

The stock material needs to be clamped to the bed of the CNC. There are
2 types of clamps normally used:

-   **Stays**, Clamps or Hold-downs are levers that are bolted to the
    T-slot channels in CNC bed to securely hold the stock material from
    moving. During machining there is a lot of lateral force on the
    stock, and there needs to be sufficient Stays that are securely
    tightened to hold the stock.

-   **Stops** are solid pieces, typically cylindrical or square that are
    bolted to the T-slot channels in the CNC bed, and used for multiple
    jobs, so that for each job the stock material can be aligned to the
    same position for machining.

> The Stays, Stops and their Bolts must not be in the path of the head
> or any part of the head assembly, otherwise a collision will occur,
> and the job will be ruined.
>
> Stays and Stops should not be moved during the job. Trying to move any
> clamp while the CNC is operating is destined to fail because the stock
> material is likely to move before the clamps can be reapplied, or the
> user may come in contact with the head and be injured.
>
> If Stays or Stops have to be moved then it is best to Pause the job,
> and carefully move and reapply the clamps, making sure that the stock
> material is not moved, and that the head is not jolted.

2.  **Safe Height

The CAM process should establish a **Safe Height** which is the distance
above the maximum height of the stock that the tool head can move
without colliding with any clamps, bolts or other parts of the stock
material. Typically a Safe Height of 5 or 10 mm is used, with the
requirement that all non-cutting moves of the head are done at the Safe
Height.

> This especially applies to the movement of the tool head at the end of
> a job. It is very easy to complete a job, leave the tool head in the
> final cutting position, press the **Home** button, and then helplessly
> watch while the tool head makes a bee-line for the zero Home position,
> by cutting straight through the finished product!

3.  **Zeroing the Job

Once the stock material has been clamped to the CNC bed, the tool head
needs to be moved to the zero or origin position to start the job. This
position is normally marked on the stock material.

a.  Ensure that the tool head is at a Safe height before proceeding.

b.  If the spindle is on, turn it off so that it doesn't scar the stock
    material.

c.  Jog the tool head along the X and Y axes to the marked zero point.

d.  Slowly jog the tool head down along the Z axis using 0.1mm steps or
    less, until the tool head is just above the height of the stock
    material. This can be done by placing a piece of 80 gsm paper on top
    of the stock material, and jogging the head down until the tool just
    touches the paper, so that the paper can only be moved with some
    resistance. If the paper cannot be moved, the tool is too low. 80
    gsm paper is about 0.1 mm thick, so this gives a good consistent
    positioning of the tool in the Z axis.

e.  Set the origin of the work coordinates by using the G-Code command:

> G92 X0 Y0 Z0

![A screenshot of a computer program Description automatically
generated](media/image25.png){width="6.59375in"
height="5.38159886264217in"}

Go to the Manual Data Input (MDI) tab and enter the command: G92 X0 Y0
Z0

Click the **Go** button.

4.  **Spindle Speed

Set the spindle speed.

## Running a Job

A G-Code job can be run using the following buttons:

> ![Open G Code file](media/image26.png){width="0.25in" height="0.25in"}
> Open a G-Code file \[O\]. The G-Code path will be displayed in the
> Preview panel, and the G-Code will be displayed in the lower panel.
>
> ![Reload current file](media/image27.png){width="0.25in"
> height="0.25in"} Reload current file \[Ctrl-R\]. Useful if the G-Code
> file is being edited concurrently.
>
> ![Begin executing the current
> file](media/image28.png){width="0.22916666666666666in"
> height="0.22916666666666666in"} Begin executing the current file
> \[R\]. A back-plot trace will be created on the Preview panel, and the
> code will scroll through the lower panel.
>
> ![Execute next line](media/image29.png){width="0.22916666666666666in"
> height="0.22916666666666666in"} Execute next line \[T\] after a pause.
> Useful for debugging.
>
> ![Pause Execution - Resume
> Execution](media/image30.png){width="0.21875in" height="0.21875in"}
> Pause Execution \[P\] Resume Execution \[S\]. Useful for ad hoc
> changes of clamps or tools.
>
> ![Stop Program Execution](media/image31.png){width="0.21875in"
> height="0.21875in"} Stop Program Execution \[ESC\].

While the job is running:

-   Keep a close watch on the job. Errors in design can frequently
    occur, and you may need to hit the Emergency Stop to avoid further
    damage to the stock material and the CNC.

-   Watch for overheating. If the bit is stationary in wood it can start
    smouldering and ultimately catch alight. When cutting metal, such as
    aluminium, it is important to have a mist spray of water on the
    cutting tool to avoid overheating.

-   Watch for melting when cutting plastics or other synthetics. These
    materials have low melting points and require low spindle speeds to
    be cut accurately, without the cut surface melting.

-   Watch for movement of the stock material, typically caused by
    insufficient or weak clamping. This usually means discarding the
    ruined stock material.

-   Remove cut waste with a vacuum cleaner. This keeps the cut space
    tidy, allows you to see what is happening, reduces dust, and reduces
    the chances of cut shards falling into the cut area and marking the
    cut surface. This is particularly the case with plastics and
    aluminium.

-   Ensure that the cut path and cut order are efficient to achieve the
    best result in a reasonable time.

-   Watch for the tool bit becoming blunt. The best option is usually to
    start again with a new tool.

-   Watch for the spindle overheating. This spindle is air cooled, and
    may overheat on long, hard jobs.

-   Watch for the final cut of a loose object. When cutting a complete
    object out of the stock material, then final cut will sever the
    object from the surrounding material. At this point it is easy for
    the object to fall back against the rotating tool bit and be ruined.
    You need to plan to capture the final object before it is damaged.

-   Make sure that when the job is finished you raise the tool bit to a
    Safe Height before returning to Home, so that the tool bit doesn't
    cut through your finished object. Often the return to Home is
    programming in the G-Code as a penultimate instruction. This also
    applies to moving the tool bit between different objects in the one
    job.

-   In the event of a power failure, give and start again.

-   The job may be paused and resumed at any time to allow adjustments
    or a break.
