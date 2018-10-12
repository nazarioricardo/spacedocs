# Flying Pawn

This is the ship.

1. [Constructor](##constructor)
2. [Events](##events)
3. [Inputs](##inputs)
4. [Properties](##properties)
5. [Methods](##methods)

## Constructor

Loads customizable settings such as:

- Pitch Inversion

## Events

1. [BeginPlay](###beginplay)
2. [Tick](###tick)
3. [Hit](###hit)

### BeginPlay

Sets up camera spring arm length and socket offset from [spring arm offset](###spring arm offset)

### Tick

Applies [motion](####motion), [rotation](####rotation), and [camera movement](####camera movement) based on rotation and speed.

Calls [remove forces](###remove forces)

#### Motion

Establishes [forward motion](#####forward motion), [strafe motion](#####strafe motion), and [upwards motion](#####upward motion).
Sums each direction's corresponding input value with its collision force value multiplied by delta seconds. 
Then it combines each resulting value into one X-Y-Z vector and calls adds this vector as an offset, creating movement.

##### Forward Motion

Adds [current forward speed](###current forward speed) with [current forward force](###current forward force), and multiplies the sum by delta seconds, before making it the X value of movement vector.

##### Strafe Motion

Adds [current strafe speed](###current strafe speed) with [current strafe force](###current strafe force), and multiplies the sum by delta seconds, before making it the Y value of movement vector.

##### Upward Motion

Adds [current up speed](###current up speed) with [current up force](###current up force), and multiplies the sum by delta seconds, before making it the Z value of movement vector.

#### Rotation

Establishes [pitch](#####pitch), [yaw](#####yaw), and [roll](#####roll) by multiplying corresponding properties by delta seconds into a rotator and adds the rotator to local rotation.

##### Pitch

Multiplies [current pitch speed](###current pitch speed) with delta seconds and makes it the Y value of the rotator.

##### Yaw

Multiplies [current yaw speed](###current yaw speed) with delta seconds and makes it the Z value of the rotator.

##### Roll

Multiplies [current roll speed](###current roll speed) with delta seconds and makes it the X value of the rotator.

#### Camera Movement

Offsets [srping arm](###spring arm) based on ship speed and rotation

##### Vertical Offset

Multplies [current pitch speed](###current pitch speed) by [throttle percentage](###throttle percentage)

##### Horizontal Offset 

Multplies [current yaw speed](###current yaw speed) by [throttle percentage](###throttle percentage)

### Hit

If state is not simulating physics, call [add forces](###add forces) then [deflect](####deflect)

#### Deflect

Alters rotation based on hit rotation, by multiplying [current forward speed](###current forward speed) with [max collision alpha](###max collision alpha) and dividing by [max speed](###max speed).
In theory, if allowed continually and repeatedly, final rotation of the ship will be parallel to the collision surface.

## Inputs

Axis: Value ranges from -1.0 to 1.0.

1. [Thrust](###inputaxis thrust)
2. [Pitch](###inputaxis pitch)
3. [Yaw](###inputaxis yaw)
4. [Roll/Strafe](###inputaxis roll/strafe)
5. [Elevate](###inputaxis elevate)

Action: Value is either 0 or 1.

1. [Forward Throttle](###inputaction forward throttle)
2. [Backward Throttle](###inputaction backward throttle)

### InputAxis Thrust
#### Positive (Forward)
##### Right Trigger
##### E
#### Negative (Backward)
##### Left Trigger
##### Q

Moves ship forwards or backwards.

Calls [Accelerate](###accelerate) function with parameters:
- [Acceleration](###acceleration)
- Input Value (-1.0 - 1.0)
- [Current Forward Speed](###current forward speed)
- [Max Speed](###max speed)
- [Min Speed](###min speed)

Sets [Current Forward Speed](###current forward speed)
Sets [Throttle Percentage](###throttle percentage)

Toggles `is simulating physics` to `on` if forward speed is `0.0`, and to `on` otherwise.

### InputAxis Pitch
#### Right Stick Up/Down
#### Mouse Up/Down

Rotates ship up or down.

Multiplies input value (0.0 - 1.0) by pitch inversion modifier, then multiplies by [Turn Speed](###turn speed), adds a little bit of [Current Yaw Speed](###current yaw speed) for variability result, sets [Current Pitch Speed](###current pitch speed).

### InputAxis Yaw
#### Right Stick Left/Right
#### Mouse Left/Right

Rotates ship left or right.

Multiplies input value (-1.0 - 1.0) by [Turn Speed](###turn speed), result sets [Current Yaw Speed](###current yaw speed).

Adds to current bank angle by:
Multiplying input value (-1.0 - 1.0) by [Max Bank Angle](###max bank angle) and [Current Forward Speed](###current forward speed) divided by [Max Speed](###max speed)

### InputAxis Roll/Strafe
#### Left Stick Left/Right
#### A/D

If in cruise mode, rolls ship. If in hover mode, strafes ship.

Sets [Current Roll Speed](###current roll speed) by multiplying input value (-1.0 - 1.0) by [Turn Speed](###turn speed)

Sets [Current Strafe Speed](###current strafe speed) by calling [Accelerate](###accelerate) function with parameters:
- [Acceleration](###acceleration)
- Input Value (-1.0 - 1.0)
- [Current Strafe Speed](###current strafe speed)
- 1000
- -1000

Sets [Up Percentage](###up percentage)

Adds to current bank angle by:
Multiplying input value (-1.0 - 1.0) by [Max Bank Angle](###max bank angle) and [Current Forward Speed](###current forward speed) divided by [Max Speed](###max speed)

### InputAxis Elevate
#### Left Stick Up/Down
#### W/S

Elevates ship up or down

Sets [Current Up Speed](###current strafe speed) by calling [Accelerate](###accelerate) function with parameters:
- [Acceleration](###acceleration)
- Input Value (-1.0 - 1.0)
- [Current Up Speed](###current strafe speed)
- 1000
- -1000

### InputAction Forward Throttle
#### Right Trigger
#### E

If backward throttle is also pressed, set to [active flight mode](###active flight mode) to hover.
Else, set to cruise


### InputAction Backward Throttle
#### Left Trigger
#### Q

If forward throttle is also pressed, set [active flight mode](###active flight mode) to hover.
Else, set to cruise.

## Properties

1. [Acceleration](###acceleration)
2. [Turn Speed](###turn speed)
3. [Max Speed](###max speed)
4. [Min Speed](###min speed)
5. [Max Up Speed](###max up speed)
6. [Max Bank Angle](###max bank angle)
7. [Max Collision Alpha](###max collision alpha)
8. [Inverted Pitch](###inverted pitch)
9. [Throttle Percentage](###throttle percentage)
10. [Strafe Percentage](###strafe percentage)
11. [Up Percentage](###up percentage)
12. [Current Forward Speed](###current forward speed)
13. [Current Up Speed](###current up speed)
14. [Current Strafe Speed](###current strafe speed)
15. [Current Pitch Speed](###current pitch speed)
16. [Current Yaw Speed](###current yaw speed)
17. [Current Roll Speed](###current roll speed)
18. [Current Forward Force](###current forward force)
19. [Current Strafe Force](###current strafe force)
20. [Current Up Force](###current up force)
21. [Current Bank Angle](###current bank angle)
22. [Spring Arm Length](###spring arm length)
23. [Spring Arm Offset](###spring arm offset)
24. [Pitch Inversion Modifier](###pitch inversion modifier)
25. [Active Flight Mode](###active flight mode)
26. [Forward Throttle Is Pressed](###forward throttle is pressed)
27. [Backward Throttle Is Pressed](###backward throttle is pressed)

### Accelereration
#### Public
#### Float

### Turn Speed
#### Public
#### Float

### Max Speed
#### Public
#### Float

### Min Speed
#### Public
#### Float

### Max Up Speed
#### Public
#### Float

### Max Bank Angle
#### Public
#### Float

### Max Collision Alpha
#### Public
#### Float

### Inverted Pitch
#### Public
#### Bool

### Throttle Percentage
#### Private
#### Float

### Strafe Percentage
#### Private
#### Float

### Up Percentage
#### Private
#### Float

### Current Forward Speed
#### Private
#### Float

### Current Up Speed
#### Private
#### Float

### Current Strafe Speed
#### Private
#### Float

### Current Pitch Speed
#### Private
#### Float

### Current Yaw Speed
#### Private
#### Float

### Current Roll Speed
#### Private
#### Float

### Current Forward Force
#### Private
#### Float

### Current Strafe Force
#### Private
#### Float

### Current Up Force
#### Private
#### Float

### Current Bank Angle
#### Private
#### Float

### Spring Arm Length
#### Private
#### Float

### Spring Arm Offset
#### Private
#### Vector

### Pitch Inversion Modifier
#### Private
#### Float

If Inverted Pitch is true, value will be -1.0. Otherwise, it will be 1.0.

### Active Flight Mode
#### Private
#### Enum

### Forward Throttle Is Pressed
#### Private
#### Bool

### Backward Throttle Is Pressed
#### Private
#### Bool

## Methods

1. [Accelerate](###accelerate)
2. [BoardShip](###board ship)
3. [AddForces](###addforces)
4. [Remove Forces](###removeforces)

### Accelerate

### BoardShip

### AddForces

### RemoveForces

