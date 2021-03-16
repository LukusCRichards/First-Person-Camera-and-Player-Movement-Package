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

**Make sure you remember to reference the script in an Awake function**, because if you don't, you will not be able to reference the JumpController script.

Now go to the Update void method and write the following:

    void Update()
    {
        PlayerMovement();
    }

The Player Movement method is what will contain the character's movement and what will help organise the script.

Under the Update method, create a private method called Player Movement and write the following:

    void PlayerMovement()
    {
        isGrounded = Physics.CheckSphere(groundCheck.position, groundDistance, groundMask);

        if (isGrounded && velocity.y < 0)
        {
            velocity.y = -2f;
        }

        float horoMove = Input.GetAxis("Horizontal") * speed;
        float vertMove = Input.GetAxis("Vertical") * speed;

        Vector3 move = transform.right * horoMove + transform.forward * vertMove;

        controller.Move(move * speed * Time.deltaTime);

        if(Input.GetKeyDown(KeyCode.Space) && isGrounded)
        {
            Jump();
        }

        velocity.y += gravity * Time.deltaTime;

        controller.Move(velocity * Time.deltaTime);
    }

The isGrounded bool may look different and might not look like the kind you're familiar with, but this is what makes sure that whenever the groundChack GameObject, that you will make later, is touching another GameObject with the layer Ground in the groundCheck layerMask, it will allow you to jump.

The if statement makes sure that it cannot teleport you to the ground when the gravity in the velocity variable gets to high, because if it gets too high, it will look like you are heavier and you will move downward a lot faster as it never stops growing.

The 2 float variables are what will be used to make the character move back and forth in every direction. The Vector3 variable is what sets the movement of the character and Character Controller variable is what makes the character move through the Character Controller component.

The if statement will be made later, but this is what makes the character jump whenever the space button is pressed and isGrounded is true.

The last two are what makes sure the character is able to fall to the ground after jumping or falling off a ledge. it utilizes both the gravity variable and Character Controller component.

If you're written these correctly and you did not encounter any errors (save for the referenced Jump method, which will be made later), the character should now fall to the ground and move around.

### First Person Camera Controller



#### Jump Controller

For this script, you don't need to add much to it as this will only be used to calculate how high the character can jump.

## Putting Everything Together



## Things to Note

### Camera Controller

This script is capable of making the character rotate with the camera and make the character move in the direction of where it faces.

This script it **not** capable of adjusting its axis when something blocks its view as this is a basic version of a camera movement script.

### IMPORTANT

