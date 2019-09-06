---
layout: Meadow
title: Hello World
subtitle: Guides and documentation for Meadow
---

# Hello, World

Meadow applications are very similiar to familiar .NET Framework 4.7.2 console applications. To access the IO (GPIO, I2C, SPI, etc.) and Meadow-specific features, your project references the **Meadow.Core** library.

Let's create a new Meadow app!

## Prerequisites

You'll need Visual Studio: [available here](https://visualstudio.microsoft.com/downloads/)

You can use any edition including Enterprise, Professional, or the free Community edition.  

### Windows

Install Visual Studio 2017 or 2019.

You'll need to ensure the **.NET Framework 4.7.2 development tools** are installed. To verify, run the Visual Studio Installer and click **Modify**. Under **Installation details**, expand **.NET desktop development** and ensure that **.NET Framework 4.7.2 development tools** is checked.

![VS2017 Installer](vs2017_install.png){:standalone}

### macOS

Install the latest version of Visual Studio for Mac.

You'll also need to install the Meadow IDE Extension for Visual Studio for Mac.

1. On the menu go to *Visual Studio -> Extensions*
#. Click the **Gallery** tab
#. Search for **Meadow**
#. Select the Meadow IDE extension
#. Click **Install...** 

![Meadow extension for Visual Studio for macOS](meadow_extension.png){:standalone}

## Part 1: Creating a new Meadow Project

### Windows

 *Coming soon*

### macOS

 1. Open Visual Studio.
 * Create a new Project: *File -> New Solution..*.
 * In the **Meadow** section, select *Meadow Application* and press **Next**.
 * Choose an app name and location.
 * Press **Create**.

## Part 2: Hello, World

Now that your project is setup, we'll walk through the default application that controls the onboard RGB led and writes text to the console. 

## The App class

Wilderness Labs recommends placing your logic in an application class that's instantiated in the `Program` class when the app starts. This class is created automatically but you can create it manaually.

 1. Create a new `public` class named `MeadowApp`.
 * Add `using` statements to `Meadow`, `Meadow.Devices`, and `Meadow.Hardware`.
 * Change the class signature to derive from `App<F7Micro, App>`.
 * Add a `void` returning method named `InitializeHardware`.
 * Call `InitializeHardware` from the constructor:

  ```csharp
  using Meadow;
  using Meadow.Devices;
  using Meadow.Hardware;

  namespace HelloLED
  {
      public class MeadowApp : App<F7Micro, App>
      {
          public App()
          {
              InitializeHardware();
          }

          void InitializeHardware()
          {
          }
      }
  }
  ```

## Control the Onboard LED

Now we'll add fields to control the onboard LED and toggle its red, green, and blue components off and on periodically.

 1. Add three (3) fields of type `DigitalOutputPort` named `redLed`, `greenLed`, and `blueLed`.
 * In the `InitializeHardware method, instantiate each output port using `Device.Pins` to reference the onboard internal pins to control each color of the led:

  ```csharp
  IDigitalOutputPort redLed;
  IDigitalOutputPort greenLed;
  IDigitalOutputPort blueLed;
  ...

  void InitializeHardware()
  {
      redLed = Device.CreateDigitalOutputPort(Device.Pins.OnboardLedRed);
      blueLed = Device.CreateDigitalOutputPort(Device.Pins.OnboardLedBlue);
      greenLed = Device.CreateDigitalOutputPort(Device.Pins.OnboardLedGreen);
  }
  ```

Now we'll add a method to toggle the LEDs. We do this by controlling the `DigitalOutputPort`'s boolean `State` property. Within the while loop, we'll write the current state to the Console and toggle the LEDs in sequence.

We'll need two (2) additional `using` statements, add `System` and `System.Threading` if they're not there already.

Add the code below:

```csharp
using System;
using System.Threading;
...

public void ToggleLeds()
{
    bool state = false;
    int sleepTime = 150; //in ms

    while(true)
    {
        state = !state;

        Console.WriteLine($"State: {state}");

        blueLed.State = state;
        Thread.Sleep(sleepTime);
        redLed.State = state;
        Thread.Sleep(sleepTime);
        greenLed.State = state;
        Thread.Sleep(sleepTime);
    }
}
```

Finally, call `ToggleLeds` from the constructor after `InitializeHardware`:

```csharp
public App()
{
    InitializeHardware();
    ToggleLeds();
}
```

## Instantiate the App class

The last thing we need to do is create an instance of the `App` class when the application starts.

 1. Open Program.cs.
 * A `using` statement for `Meadow`.
 * Create a static field of type `IApp` named **app**.
 * In the constructor, instantiate an `App` instance and assign it to **app**:

  ```csharp
  using Meadow;

  namespace HelloLED
  {
      class Program
      {
          static IApp app;
          static void Main(string[] args)
          {
              app = new MeadowApp();
          }
      }
  }
  ```

## Compile and run the application

You're now ready to build and deploy your Meadow app.

### macOS

1. Connect your Meadow device to your development machine
#. Press the **Play** button in Visual Studio to compile and deploy your application
#. Wait 30-60 seconds for your application to start

### Windows

Coming soon

## [Next - Meadow Basics](/Meadow/Meadow_Basics/)