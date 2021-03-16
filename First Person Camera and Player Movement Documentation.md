# First Person Camera and Player Movement Documentation

This documentation will tell you how to make the script and how to implement it with the rest of the required components.

## Required Components

3 Scripts

1 Camera component

1 Character Controller

## Writing the Script

### First Person Character Controller

After creating the the Character Controller script, create 6 **public** variables and 3 private variables as written below:

    public CharacterController controller;

    public float speed = 5f;
    public float gravity = -9f;

    public Transform groundCheck;
    public float groundDistance = 0.4f;
    public LayerMask groundMask;

    bool isGrounded;
    Vector3 velocity;

    JumpController jumpController;

The speed and gravity variables are self explanitory, but the Character Controller component is what will replace the collider for the game object that you will be using for yout character. The last 3 public variables, along with the **isGrounded** bool, are what will be used to make sure that when you fall, you **won't** be able to jump again until you reach the floor.

The velicity variable is what will be used to make sure that the character can fall without using a rigidbody to do it for you, and the **jumpController** variable is what this script will reference so the character will be able to jump.

The next thing to do is to reference the Jump Controller script by creating an Awake method and writing the following:

    void Awake()
    {
        jumpController = GetComponent<JumpController>();
    }

### First Person Camera Controller



### Jump Controller

For this script, you don't need to add much to it as this will only be used to calculate how high the character can jump.

## Putting Everything Together



## Things to Note

### Camera Controller

This script is capable of making the character rotate with the camera and make the character move in the direction of where it faces.

This script it **not** capable of adjusting its axis when something blocks its view as this is a basic version of a camera movement script.

### IMPORTANT

