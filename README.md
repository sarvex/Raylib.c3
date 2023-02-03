<div align="center">
<p>

<img width="200" src="./assets/logo.png"></img>

# Raylib.c3 - [Raylib](https://www.raylib.com/) binding for the [C3 Programming Language](https://c3-lang.org/)

</p>

| [📀 Installation](./INSTALL.md) | [🧑‍💻 Development](./DEVELOPMENT.md) | [🤝 Contributing](./CONTRIBUTING.md) | [📜 License](./LICENSE) |

</div>

**Raylib.c3** is a [Raylib](https://www.raylib.com/) binding for [Raylib 4.5-dev](http://www.raylib.com/) a simple and easy-to-use library to learn videogames programming.

Raylib.c3 is the perfect solution for any Raylib game developer who wants to get the job done without sacrificing their sanity. After all, do you really want to be the one who drained their brain cells just trying fight with C's footguns?

### Code Example

<details>
<summary><b>Basic Window</b></summary>

```c
import rl;

const int SCREEN_WIDTH = 800;
const int SCREEN_HEIGHT = 450;

fn void main()
{
    rl::initWindow(SCREEN_WIDTH, SCREEN_HEIGHT, "raylib [core] example - basic window");
    defer rl::closeWindow();
    
    rl::setTargetFPS(60);

    while(!rl::windowShouldClose())
    {
        rl::@drawing() {
            rl::clearBackground(rl::RAYWHITE);
            rl::drawText("Congrats! You created your first window!", 190, 200, 20, rl::LIGHTGRAY);
        };
    }
}
```

</details>

<details>
<summary><b>Basic Keyboard Input</b></summary>

```c
import rl;

const int SCREEN_WIDTH = 800;
const int SCREEN_HEIGHT = 450;

fn void main()
{
    // Initialization
    //--------------------------------------------------------------------------------------
    rl::setConfigFlags(rl::config::VSYNC_HINT);
    rl::initWindow(SCREEN_WIDTH, SCREEN_HEIGHT, "raylib [core] example - keyboard input");
    defer rl::closeWindow();

    Vector2 ballPosition = { (float)(SCREEN_WIDTH/2), (float)(SCREEN_HEIGHT/2) };

    while(!rl::windowShouldClose())
    {
        // Update
        //----------------------------------------------------------------------------------
        if (rl::isKeyDown(rl::keyboard::RIGHT)) ballPosition.x += 2.0f;
        if (rl::isKeyDown(rl::keyboard::LEFT)) ballPosition.x -= 2.0f;
        if (rl::isKeyDown(rl::keyboard::UP)) ballPosition.y -= 2.0f;
        if (rl::isKeyDown(rl::keyboard::DOWN)) ballPosition.y += 2.0f;
        //----------------------------------------------------------------------------------

        // Update
        //----------------------------------------------------------------------------------
        rl::@drawing() {
            rl::clearBackground(rl::RAYWHITE);

            rl::drawText("move the ballwith arrow keys", 10, 10, 20, rl::DARKGRAY);

            rl::drawCircleV(ballPosition, 50, rl::MAROON);
        };
        //----------------------------------------------------------------------------------

    }
}
```

</details>

<details>
<summary><b>Texture Loading and Drawing</b></summary>

```c
import rl;

const int SCREEN_WIDTH = 800;
const int SCREEN_HEIGHT = 450;

fn void main()
{
    rl::initWindow(SCREEN_WIDTH, SCREEN_HEIGHT, "raylib [textures] example - texture loading and drawing");
    defer rl::closeWindow();
    
    rl::setTargetFPS(60);

	rl::Texture2D texture = rl::loadTexture("resources/logo.png");
	defer rl::unloadTexture(texture);

    while(!rl::windowShouldClose())
    {
        rl::@drawing() {
            rl::clearBackground(rl::RAYWHITE);
			rl::drawTexture(texture, SCREEN_WIDTH/2 - texture.width/2, SCREEN_HEIGHT/2 - texture.height/2, rl::WHITE);
            rl::drawText("this IS a texture!", 360, 370, 10, rl::GRAY);
        };
    }
}
```

</details>

<details>
<summary><b>3D Free Camera</b></summary>

```c
import rl;

// Constants
//--------------------------------------------------------------------------------------
const int SCREEN_WIDTH = 800;
const int SCREEN_HEIGHT = 450;

//------------------------------------------------------------------------------------
// Program main entry point
fn void main() {
// Initialization
//--------------------------------------------------------------------------------------
    rl::initWindow(SCREEN_WIDTH, SCREEN_HEIGHT, "raylib [core] example - 3d camera free");
    defer rl::closeWindow();

	rl::Camera3D camera = { .position = Vector3{ 10.0f, 10.0f, 10.0f }, .target = Vector3{ 0.0f, 0.0f, 0.0f }, .up = Vector3{ 0.0f, 1.0f, 0.0f }, .fovy = 45.0f, .projection = rl::CameraProjection.PERSPECTIVE }; // named parameters are optional and not required.
	rl::Vector3 cubePosition = { .x = 0f, .y = 0f, .z = 0f }; // named parameters are optional and not required.
    
	rl::setCameraMode(camera, rl::CameraMode.FREE); // Set a free camera mode
    rl::setTargetFPS(60); // Set our game to run at 60 frames-per-second
    //--------------------------------------------------------------------------------------

	// Main game loop
    while(!rl::windowShouldClose()) { // Detect window close button or ESC key 
	    
		// Update
        //----------------------------------------------------------------------------------
        rl::updateCamera(&camera);

        if (rl::isKeyDown(rl::keyboard::Z)) camera.target = Vector3{ 0.0f, 0.0f, 0.0f };
        //----------------------------------------------------------------------------------
 		
		// Draw
        //----------------------------------------------------------------------------------
        rl::@drawing() {
            rl::clearBackground(rl::RAYWHITE);

			rl::@mode3D(camera) {
				rl::drawCube(cubePosition, 2.0f, 2.0f, 2.0f, rl::RED);
                rl::drawCubeWires(cubePosition, 2.0f, 2.0f, 2.0f, rl::MAROON);

                rl::drawGrid(10, 1.0f);
			};

			rl::drawRectangle( 10, 10, 320, 133, rl::fade(rl::SKYBLUE, 0.5f));
            rl::drawRectangleLines( 10, 10, 320, 133, rl::BLUE);

            rl::drawText("Free camera default controls:", 20, 20, 10, rl::BLACK);
            rl::drawText("- Mouse Wheel to Zoom in-out", 40, 40, 10, rl::DARKGRAY);
            rl::drawText("- Mouse Wheel Pressed to Pan", 40, 60, 10, rl::DARKGRAY);
            rl::drawText("- Alt + Mouse Wheel Pressed to Rotate", 40, 80, 10, rl::DARKGRAY);
            rl::drawText("- Alt + Ctrl + Mouse Wheel Pressed for Smooth Zoom", 40, 100, 10, rl::DARKGRAY);
            rl::drawText("- Z to zoom to (0, 0, 0)", 40, 120, 10, rl::DARKGRAY);
        };
    }
}
```

</details>

<p>

More examples can be found [here.](https://github.com/Its-Kenta/Raylib.c3/tree/main/examples)

</p>