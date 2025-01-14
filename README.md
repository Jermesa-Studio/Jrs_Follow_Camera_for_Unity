# Free Smooth Third-Person Vehicle Follow Camera script for Unity

Welcome to this beginner-friendly guide on using the **JrsFollowCamera.cs** script in Unity! This open-source, free-to-use script is perfect for creating a smooth third-person vehicle camera in your Unity games. Whether you're making a racing game, a driving simulator, or any other vehicle-based game, this script will help you achieve professional-quality camera movement.

✅ **Want to see how the camera works?** Check out this full project with drivable 🚘 vehicle on [this link](https://jermesa.com/vehicle-physics-car-controller-for-unity-with-free-and-open-source-script/).

> **✨ Open-Source & Free for Commercial Use:** This script is licensed under the MIT License, meaning you can use it freely in your personal or commercial projects. Check out the [GitHub repository](https://github.com/Jermesa-Studio/Jrs_Follow_Camera_with_smooth_feature_for_Unity) for the source code.

> **📥 Download the Complete Project:** Want to see the camera in action? Download the complete Unity project from [this link](https://github.com/Jermesa-Studio/JRS_Vehicle_Physics_Controller).

## 📑 Table of Contents

- [What is JrsFollowCamera.cs?](#what-is-jrsfollowcamera)
- [Why Use This Script?](#why-use-it)
- [Step-by-Step Guide to Using the Script](#step-by-step-guide)
- [Features of the Script](#features)
- [Tips for Getting the Best Results](#tips)
- [Other Uses of the Script](#other-uses)
- [Full Script Code](#code)
- [Attribution and Licenses](#attribution)

## Feature Cards

| Feature | Description |
| --- | --- |
| **Smooth Movement** | Uses a spring-damper system for smooth camera transitions. |
| **Vehicle-Friendly** | Perfect for third-person vehicle cameras in racing or driving games. |
| **Open-Source** | Free to use and modify under the MIT License. |
| **Easy to Integrate** | Simple setup with customizable parameters. |

## What is JrsFollowCamera.cs?

The **JrsFollowCamera.cs** script is a Unity C# script designed to create a smooth, third-person camera that follows a target object (like a vehicle). It uses a spring-damper system to ensure the camera moves smoothly, even when the target is moving or turning quickly. This makes it ideal for vehicle-based games where the camera needs to follow the player's car or other vehicles.

## Why Use This Script?

Here are some reasons why **JrsFollowCamera.cs** is a great choice for your Unity projects:

- **Smooth Camera Movement:** The spring-damper system ensures the camera doesn't jerk or snap to the target's position.
- **Customizable Offset:** You can easily adjust the camera's position relative to the target.
- **Automatic Rotation:** The camera always faces the target, keeping it in view at all times.
- **Free and Open-Source:** You can use and modify the script for free, even in commercial projects.

## Step-by-Step Guide to Using the Script

Follow these steps to integrate the **JrsFollowCamera.cs** script into your Unity project:

1. **Download the Script:** Copy the script from the [GitHub repository](https://github.com/Jermesa-Studio/Jrs_Follow_Camera_with_smooth_feature_for_Unity) or use the code provided at the end of this article.
2. **Create a New Script:** In Unity, create a new C# script and name it `JrsFollowCamera.cs`.
3. **Paste the Code:** Replace the default code in the script with the code provided below.
4. **Attach the Script:** Attach the script to your main camera in the Unity scene.
5. **Set the Target:** Assign the `target` variable in the Inspector to the transform of the vehicle or object you want the camera to follow.
6. **Adjust the Offset:** Set the `offset` variable to control the camera's position relative to the target.
7. **Tune the Parameters:** Adjust the `horizontalSpringConstant` and `horizontalDampingConstant` to achieve the desired smoothness.

## Features of the Script

Here are the key features of the **JrsFollowCamera.cs** script:

- **Spring-Damper System:** Ensures smooth horizontal movement of the camera.
- **Fixed Vertical Offset:** Maintains a consistent height relative to the target.
- **Automatic Rotation:** The camera always faces the target.
- **Physics-Based Updates:** Uses `FixedUpdate` for consistent and smooth movement.

## Tips for Getting the Best Results

> **Tip:** Use a higher `horizontalSpringConstant` for faster camera movement and a higher `horizontalDampingConstant` to reduce oscillations.

## Other Uses of the Script

While this script is perfect for vehicle cameras, it can also be used in other scenarios, such as:

- **Third-Person Character Cameras:** Follow a character in a third-person game.
- **Drone or Flying Object Cameras:** Smoothly follow a drone or flying object.
- **Cinematic Cameras:** Create smooth camera movements for cutscenes.

## Full Script Code

Here is the complete code for the **JrsFollowCamera.cs** script:

```csharp
// MIT License
//
// Copyright (c) 2023 Samborlang Pyrtuh
//
// Permission is hereby granted, free of charge, to any person obtaining a copy
// of this software and associated documentation files (the "Software"), to deal
// in the Software without restriction, including without limitation the rights
// to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
// copies of the Software, and to permit persons to whom the Software is
// furnished to do so, subject to the following conditions:
//
// The above copyright notice and this permission notice shall be included in all
// copies or substantial portions of the Software.
//
// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
// IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
// AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
// LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
// OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
// SOFTWARE.

using UnityEngine;

public class JrsFollowCamera : MonoBehaviour
{
    public Transform target; // The vehicle to follow
    public Vector3 offset; // The offset from the vehicle
    public float horizontalSpringConstant = 0.5f; // The spring constant for horizontal movement
    public float horizontalDampingConstant = 0.3f; // The damping constant for horizontal movement
    private Vector3 velocity; // The velocity of the camera

    void FixedUpdate()
    {
        Vector3 desiredPosition = target.TransformPoint(offset);
        Vector3 horizontalDisplacement = new Vector3(desiredPosition.x - transform.position.x, 0, desiredPosition.z - transform.position.z);
        Vector3 horizontalSpringForce = horizontalSpringConstant * horizontalDisplacement;
        Vector3 horizontalDampingForce = -horizontalDampingConstant * new Vector3(velocity.x, 0, velocity.z);
        Vector3 force = horizontalSpringForce + horizontalDampingForce;

        velocity += force * Time.fixedDeltaTime;

        transform.position += new Vector3(velocity.x, 0, velocity.z) * Time.fixedDeltaTime;

        // Calculate the desired camera height based on the target's position and offset
        float desiredCameraHeight = target.position.y + offset.y;

        // Set the camera's position with the desired height
        transform.position = new Vector3(transform.position.x, desiredCameraHeight, transform.position.z);

        Vector3 lookDirection = target.position - transform.position;
        Quaternion rotation = Quaternion.LookRotation(new Vector3(lookDirection.x, lookDirection.y, lookDirection.z));
        transform.rotation = rotation;
    }
}
