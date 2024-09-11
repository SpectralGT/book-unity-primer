# **Chapter 8: Input and Interaction**

## **8.1 Handling Input in Unity**

Handling input is a fundamental aspect of game development, as it dictates how players interact with your game. Unity provides two primary systems for input handling: the legacy Input Manager and the New Input System. The legacy Input Manager is simpler and widely used, but it has limitations when it comes to supporting multiple input devices or more advanced interaction schemes. The New Input System is more flexible and extensible, allowing developers to support complex input scenarios across various devices, including keyboards, game controllers, and touchscreens.

The legacy Input Manager is the older of the two systems and is suitable for simpler projects. It allows developers to define input axes (such as horizontal and vertical movement) and buttons, which can then be mapped to keyboard keys, mouse buttons, or controller inputs. The New Input System, on the other hand, introduces the concept of Input Actions, which decouple the input devices from the actions they perform in the game, making it easier to handle complex control schemes.

To get started with handling input in Unity, you must first decide which input system is appropriate for your project. If you're working on a simple game with standard input requirements, the legacy Input Manager might suffice. However, for more complex projects that need to support multiple input devices or advanced interaction techniques, the New Input System is a better choice.

---

## **8.2 Mouse, Keyboard, and Controller Support**

### **Mouse Input Handling**

Mouse input is crucial for many games, especially those on PC platforms. Unity provides built-in support for mouse input through its `Input` class. You can detect mouse button presses, track the mouse’s position on the screen, and even handle more complex interactions like drag-and-drop.

To detect mouse clicks, you can use the following code:

```csharp
if (Input.GetMouseButtonDown(0)) // Left-click
{
    // Handle left-click action
}
```

Tracking mouse movement is also straightforward. The `Input.mousePosition` property returns the current position of the mouse in screen coordinates, allowing you to determine where the player is pointing or clicking in the game world.

For drag-and-drop functionality, you can combine mouse button detection with tracking the mouse position over time. For example, you might check if the player is holding down the left mouse button (`Input.GetMouseButton(0)`) and use `Input.mousePosition` to move an object across the screen.

### **Keyboard Input Handling**

Keyboard input is another essential part of most games, particularly on PC. Unity allows developers to capture keyboard input through various methods provided by the `Input` class. You can detect specific key presses using `Input.GetKeyDown`, check if a key is being held down with `Input.GetKey`, or use `Input.GetAxis` for smoother, continuous input (e.g., for character movement).

For example, to detect if the player presses the "W" key to move forward:

```csharp
if (Input.GetKeyDown(KeyCode.W))
{
    // Move character forward
}
```

Using `Input.GetAxis`, you can capture movement input that works both with a keyboard and a controller’s joystick:

```csharp
float moveHorizontal = Input.GetAxis("Horizontal");
float moveVertical = Input.GetAxis("Vertical");

Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
transform.Translate(movement * speed * Time.deltaTime);
```

This method provides smooth and responsive character movement by reading input values continuously over time.

### **Controller Support**

Game controllers, such as Xbox and PlayStation controllers, are widely used across various gaming platforms. Unity supports controllers out of the box, allowing you to map controller input to predefined axes and buttons. For example, you can map the left joystick of a controller to the "Horizontal" and "Vertical" axes, enabling character movement with a joystick.

To detect controller input, you can use the same methods as with keyboard and mouse input, such as `Input.GetAxis` for joystick movement or `Input.GetButtonDown` for button presses. Unity also supports multiple controllers, allowing for local multiplayer setups where each player uses their own controller.

If you need to handle more advanced controller features, such as detecting the position of analog triggers or dealing with multiple controllers, the New Input System provides enhanced support for these scenarios.

---

## **8.3 Implementing Touch Controls for Mobile Devices**

With the rise of mobile gaming, touch controls have become a crucial part of game design. Unity provides extensive support for touch input through its `Input` class, allowing developers to capture touch events and implement intuitive controls for mobile devices.

### **Detecting Touch Input**

The `Input.touchCount` property returns the number of touches currently on the screen. This allows you to handle single-touch and multi-touch scenarios effectively. To get details about a specific touch, you can use `Input.GetTouch(index)`, which returns information about the touch at the specified index (e.g., position, phase, and delta movement).

For example, to detect a tap (single-touch) on the screen:

```csharp
if (Input.touchCount > 0)
{
    Touch touch = Input.GetTouch(0);

    if (touch.phase == TouchPhase.Began)
    {
        // Handle tap
    }
}
```

This code snippet checks if there's at least one touch on the screen and handles the tap when the touch begins.

### **Handling Swipe Gestures**

Swiping is a common interaction in mobile games, often used for movement or to trigger specific actions. Detecting a swipe gesture involves tracking the touch’s position and determining the direction of movement.

To implement a swipe gesture, you can monitor the touch's position during the `TouchPhase.Moved` phase and calculate the swipe direction:

```csharp
if (Input.touchCount > 0)
{
    Touch touch = Input.GetTouch(0);

    if (touch.phase == TouchPhase.Moved)
    {
        Vector2 swipeDirection = touch.deltaPosition.normalized;
        // Use swipeDirection to determine movement or actions
    }
}
```

By calculating the swipe direction, you can move characters, change camera angles, or trigger specific actions based on the player’s input.

### **Multi-Touch Gestures**

Multi-touch gestures, such as pinch-to-zoom, are commonly used in mobile games to enhance interaction. Unity supports multi-touch input, allowing you to implement gestures that involve multiple fingers.

For example, to implement pinch-to-zoom, you can calculate the distance between two touches and adjust the camera's zoom level based on the change in distance:

```csharp
if (Input.touchCount == 2)
{
    Touch touch1 = Input.GetTouch(0);
    Touch touch2 = Input.GetTouch(1);

    Vector2 touch1PrevPos = touch1.position - touch1.deltaPosition;
    Vector2 touch2PrevPos = touch2.position - touch2.deltaPosition;

    float prevTouchDeltaMag = (touch1PrevPos - touch2PrevPos).magnitude;
    float touchDeltaMag = (touch1.position - touch2.position).magnitude;

    float deltaMagnitudeDiff = prevTouchDeltaMag - touchDeltaMag;

    // Adjust camera zoom based on deltaMagnitudeDiff
}
```

This code tracks two touches and adjusts the camera’s zoom level based on the change in distance between the touches, providing an intuitive pinch-to-zoom functionality.

---

## **8.4 Creating Interactive Game Mechanics**

Interactive game mechanics rely heavily on responsive input handling. Whether you're building a puzzle game, an action-packed shooter, or an immersive RPG, ensuring that your game's controls feel intuitive and responsive is key to creating a compelling experience.

### **Designing for Responsiveness**

Responsiveness in input handling refers to how quickly and accurately the game responds to player actions. In a fast-paced game, even a slight delay in input processing can make the game feel sluggish. To ensure responsiveness, input handling code should be optimized to minimize latency, and input actions should be mapped as directly as possible to in-game events.

For example, in a platformer game, when a player presses the jump button, the character should jump immediately. Any delay in this action can frustrate players and break immersion.

### **Interactive Elements and Feedback**

In addition to responsiveness, providing feedback for player actions is essential for creating interactive game mechanics. Feedback can be visual, auditory, or haptic (vibration) and helps reinforce the connection between player input and game events.

For example, in a shooting game, when a player presses the fire button, the game should provide immediate feedback through sound effects, visual muzzle flashes, and possibly a vibration if the game is played on a controller. This feedback makes the action feel more impactful and engaging.

### **Input Customization**

Allowing players to customize their controls can enhance accessibility and player comfort. Both the legacy Input Manager and the New Input System in Unity allow for input remapping, although the New Input System provides more robust support for runtime input rebinding.

By enabling players to customize their controls, you can accommodate different playstyles and preferences, making your game more accessible to a broader audience.

---

## **8.5 Gesture Recognition and Advanced Input Techniques**

Advanced input techniques, such as gesture recognition, can add depth and immersion to your game. Gesture recognition involves detecting and interpreting complex input patterns, such as specific movements or combinations of inputs, to trigger in-game actions.

### **Gesture Recognition**

Unity’s New Input System supports gesture recognition out of the box, allowing you to define custom gestures and map them to specific actions. For example, in a mobile game, you might define a circular swipe gesture to perform a special attack.

To implement gesture recognition, you can use Unity's `InputSystem` library to detect patterns in touch or mouse input. You might compare

 the player’s input against predefined gesture templates or use algorithms like dynamic time warping (DTW) to recognize gestures in real-time.

### **Advanced Input Techniques**

In addition to gesture recognition, advanced input techniques can include things like voice commands, accelerometer-based controls, or even custom hardware inputs. Unity's extensible input system allows for the integration of these advanced inputs, giving you the flexibility to create unique and innovative gameplay experiences.

For example, in a mobile game, you might use the device’s accelerometer to control a character’s movement by tilting the device. Similarly, in a VR game, you might use hand-tracking data to allow players to interact with objects naturally.

Unity's New Input System is particularly well-suited for these advanced input techniques, as it allows for more complex input handling and the integration of custom devices and input methods.