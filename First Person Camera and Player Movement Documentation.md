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

In this script, create 1 public float variable 1 public Transform variable and 1 private float variable with a value of 0 which should look like this:

    public float rotationSpeed = 75f;

    public Transform character;

    float xRotation = 0f;

These variables are what the camera will use to make the camera rotate with the mouse and the transform variable is what makes sure that the character rotates along with it.

In the Update method, write a reference to the method called **CamControl** and underneath it, create the CamControll method which should look like the following:
    
    void Update()
    {
        CamControl();        
    }

    void CamControl()
    {

        float mouseX = Input.GetAxis("Mouse X") * rotationSpeed * Time.deltaTime;
        float mouseY = Input.GetAxis("Mouse Y") * rotationSpeed * Time.deltaTime;
        mouseY = Mathf.Clamp(mouseY, -35, 60);

        xRotation -= mouseY;
        xRotation = Mathf.Clamp(xRotation, -50f, 90f);

        transform.localRotation = Quaternion.Euler(xRotation, 0f, 0f);
        character.Rotate(Vector3.up * mouseX);
    }

The referenced method in the Update method is what makes sure that the camera can rotate as the Update methof checks everyframe, as what should happen for this method.

Same as the float variables used for the Character Controller script, the 2 float variables in this method will be used to make the camera along with the mouse's movement in all directions. If you encounter any inverted camera movement, you might have wrote negative (-) next to one of the Inputs for the axis and all you need to do is remove it.

In this method, you might see something you're not familiar with using the Mathf: **.Clamp**. .Clamp (or **Clamping**) is used to make sure that a rotation can't go past its set rotation axis, which is useful especially for a camera.

#### Jump Controller

For this script, you don't need to add much to it as this will only be used to calculate how high the character can jump.

You just need to write a public float variable called jumpForce and give it any value you want.

    public float jumpHeight = 3f;

This is what the Character Controller script will reference in order to make the character jump, as the value of this variable is what will be used when the jump method is called from the Character Controller script.

## Putting Everything Together

After you've written the scripts, it's time to make the other assets for the character.

First, create an empty GameObject and name it First Person Player, then give it the Character Controller component and edit its **raduis** to **0.6** and its **height** to **3.8** as this changes the size of its collider. Now reset its transform then add a 3D Object of your choice into it as a child and remove its collider component as it is not needed anymore. You don't add the Character Controller script to the child.

Next thing to do is to create a Camera GameObject, or use the Main Camera, parent it to the First Person Player GameObject and reset it transform. Make sure to raise it up a bit so it reaches the head, but **do not** move it in any other direction as this will mess with the character's rotation through the camera.

Now create one more empty GameObject and name it as **Ground Check** and move it to the very bottom of the character as this is what the Character Controller script will use in order to check whether or not the character's isGrounded value is true.

Finally, add the First Person Character Controller and Jump Controller scripts to the First Person Player GameObject and assign the corresponding assets into the First Person Character Controller script's slots. Then add the First Person Camera Controller script into the Camera GameObject and asign the First Person Player GameObject into the Character slot of that very script.

If you've done everything correctly, your character should not be moving in the direction you're facing and jump whenever you press the space button, but not jumping agani while you're in the air. If you feel like the jumping is too high or too low, simply change its value.

## Things to Note

### Camera Controller

This script is capable of making the character rotate with the camera and make the character move in the direction of where it faces.

This script it **not** capable of adjusting its axis when something blocks its view, but this should not be a problem as it is contained in a collider through the Character Controller.

### IMPORTANT

Make sure that any scripts you reference in other scripts are in an Awake method, because if you do not do this, the other scripts will not be referenced so that they work in the script that referenced it.
