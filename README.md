## Practical 3 (Develop Snake Game using pygame)

```python
import pygame
import random

pygame.init()

WIDTH, HEIGHT = 500, 400
BLOCK_SIZE = 10
FPS = 10

BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

screen = pygame.display.set_mode((WIDTH, HEIGHT))

font = pygame.font.SysFont("Arial", 24)

snake = [(100, 50), (90, 50), (80, 50)]
food = (random.randrange(0, WIDTH, BLOCK_SIZE), random.randrange(0, HEIGHT, BLOCK_SIZE))
dx, dy = BLOCK_SIZE, 0
score = 0

running, clock = True, pygame.time.Clock()
game_over = False

while running:
    if not game_over:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP and dy == 0: dx, dy = 0, -BLOCK_SIZE
                if event.key == pygame.K_DOWN and dy == 0: dx, dy = 0, BLOCK_SIZE
                if event.key == pygame.K_LEFT and dx == 0: dx, dy = -BLOCK_SIZE, 0
                if event.key == pygame.K_RIGHT and dx == 0: dx, dy = BLOCK_SIZE, 0

        head = (snake[0][0] + dx, snake[0][1] + dy)
        snake = [head] + snake[:-1]

        if head == food:
            snake.append(snake[-1])
            score += 1
            food = (random.randrange(0, WIDTH, BLOCK_SIZE), random.randrange(0, HEIGHT, BLOCK_SIZE))
        if head[0] < 0 or head[0] >= WIDTH or head[1] < 0 or head[1] >= HEIGHT or head in snake[1:]:
            game_over = True

        screen.fill(BLACK)
        for segment in snake:
            pygame.draw.rect(screen, GREEN, (*segment, BLOCK_SIZE, BLOCK_SIZE))
        pygame.draw.rect(screen, RED, (*food, BLOCK_SIZE, BLOCK_SIZE))
        
        pygame.display.flip()
        clock.tick(FPS)

    else:
        screen.fill(BLACK)
        game_over_text = font.render(f"Game Over! Score: {score}", True, WHITE)
        screen.blit(game_over_text, (WIDTH // 4, HEIGHT // 2))
        pygame.display.flip()

        pygame.time.wait(2000)
        running = False

pygame.quit()
```

## Practical 4 (Shooting Game Pygame)

```
import pygame
import random

# Initialize
pygame.init()
width, height = 800, 600
screen = pygame.display.set_mode((width, height))
character = pygame.Rect(width // 2 - 25, height - 50, 50, 50)
bullets = []
enemies = []
score = 0
clock = pygame.time.Clock()

# Main game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN and event.key == pygame.K_SPACE:
            bullets.append(pygame.Rect(character.centerx - 5, character.top, 10, 10))

    for bullet in bullets[:]:
        bullet.y -= 10
        if bullet.top < 0:
            bullets.remove(bullet)

    if random.randint(1, 100) <= 2:
        enemy_x = random.randint(20, width - 20)
        enemies.append(pygame.Rect(enemy_x - 20, 0, 40, 40))

    for enemy in enemies[:]:
        enemy.y += 5
        if enemy.top > height:
            enemies.remove(enemy)

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        character.x -= 5
    if keys[pygame.K_RIGHT]:
        character.x += 5

    for bullet in bullets[:]:
        for enemy in enemies[:]:
            if bullet.colliderect(enemy):
                score += 1
                bullets.remove(bullet)
                enemies.remove(enemy)

    for enemy in enemies:
        if character.colliderect(enemy):
            running = False

    screen.fill((255, 255, 255))
    for bullet in bullets:
        pygame.draw.polygon(screen, (0, 0, 255), [(bullet.left, bullet.bottom), (bullet.centerx, bullet.top), (bullet.right, bullet.bottom)])
    pygame.draw.rect(screen, (255, 0, 0), character)
    for enemy in enemies:
        pygame.draw.circle(screen, (255, 0, 0), enemy.center, 20)

    font = pygame.font.Font(None, 36)
    score_text = font.render(f"Score: {score}", True, (255, 0, 0))
    screen.blit(score_text, (10, 10))

    pygame.display.flip()
    clock.tick(60)

pygame.quit()
```

## Practical 5 (Infinite Scrolling Effect)

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour
{
    public float SPEED = 0.5F;

    // Start is called before the first frame update
    void Start()
    {
    }

    // Update is called once per frame
    void Update()
    {
        Vector2 OFFSET = new Vector2(Time.time * SPEED, 0);
        GetComponent<Renderer>().material.mainTextureOffset = OFFSET;
    }
}
```

### Steps:

1. **Create a Quad**: In Unity, go to `GameObject > 3D Object > Quad` to create a quad in the scene.
2. **Import Background Image**: In the Assets panel, right-click and import your background image. Select the image, then in the Inspector, set `Texture Type` to `Default`.
3. **Apply Script**: Drag and drop the camera shake script onto the Quad.

## Practical 6 (Camera Shake Effect)

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraShakeScript : MonoBehaviour
{
    public Transform cameraTransform = default;
    private Vector3 _originalPosOfCam = default;
    public float shakeFrequency = 0.1f;
    public GameObject quad;

    void Start()
    {
        if (cameraTransform == null)
        {
            cameraTransform = Camera.main.transform;
        }

        _originalPosOfCam = cameraTransform.position;
    }

    void Update()
    {
        if (Input.GetKey(KeyCode.S))
        {
            CameraShake();
        }
        else if (Input.GetKeyUp(KeyCode.S))
        {
            StopShake();
        }
    }

    private void CameraShake()
    {
        cameraTransform.position = _originalPosOfCam + UnityEngine.Random.insideUnitSphere * shakeFrequency;
    }

    private void StopShake()
    {
        cameraTransform.position = _originalPosOfCam;
    }
}
```

### Steps:

1. **Create a Quad**: In Unity, go to `GameObject > 3D Object > Quad`, and assign a background image to the quad as a texture.
2. **Add Box Collider 2D**: In the inspector, add `Physics 2D > Box Collider 2D` to the Quad.
3. **Attach Script**: Drag and drop the camera shake script onto the Quad, set `cameraTransform` to **Main Camera**, and set `shakeFrequency` to `5`.

## Practical 7, 10 (Smart Enemy)

```csharp
using System.Collections;
using System.Collections.Generic;
using System.Collections.Specialized;
using System.Threading;
using UnityEngine;

public class EnemyTransform : MonoBehaviour
{
    public Transform player;
    public float playerDistance;
    public float rotationDamping;
    public float moveSpeed;

    // Start is called before the first frame update
    void Start()
    {

    }

    // Update is called once per frame
    void Update()
    {
        playerDistance = Vector3.Distance(player.position, transform.position);

        if (playerDistance < 15f)
        {
            LookAtPlayer();
        }
        if (playerDistance > 12f)
        {
            Chase();
        }
    }

    void LookAtPlayer()
    {
        Quaternion rotation = Quaternion.LookRotation(player.position - transform.position);
        transform.rotation = Quaternion.Slerp(transform.rotation, rotation, Time.deltaTime * rotationDamping);
    }
    void Chase()
    {
        transform.Translate(Vector3.forward * moveSpeed * Time.deltaTime);
    }
}
```

## Practical 8 (SnowFall)

### Snowfall Particle Effect in Unity: Quick Steps

1. **Create a Particle System:**
    - In Unity Editor, select GameObject > Effects > Particle System to add a particle system for snowfall.
2. **Configure Particle System Settings:**
    - **Duration:** Set high or loop for continuous snowfall.
    - **Start Lifetime:** Adjust for snowflake longevity (e.g., 5 seconds).
    - **Start Speed:** Set low speed (1-5) for gentle fall.
    - **Start Size:** Small snowflakes (0.05 to 0.2).
    - **Start Color:** Use white or light blue.
    - **Emission Rate:** Set around 100 particles per second.
    - **Shape:** Choose "Cone" and adjust angle/radius for snow spawn area.
    - **Gravity Modifier:** Use a small negative value (-0.1) for a slow fall.
3. **Apply Snowflake Texture:**
    - In Renderer, assign a particle material with a snowflake texture (with transparent background).
4. **Enhance with Lifetime Settings:**
    - Use "Color Over Lifetime", "Size Over Lifetime", and "Rotation Over Lifetime" for variation.
5. **Loop the Effect (Optional):**
    - Enable "Looping" in Particle System settings for endless snowfall.
6. **Play & Optimize:**
    - Press Play to test. Adjust particle count if performance drops.

---

## Practical 9 (SnowFall)

We can create a basic game like a “Roll-a-Ball” where a player controls a ball that rolls around and collects items. It’s one of the classic starter projects for Unity beginners. Here’s a step-by-step guide to get you started:

### **Step 1: Set Up Your Environment**

1. **Install Unity:** Download and install Unity Hub from [**Unity’s website**](https://unity.com/?form=MG0AV3). Use Unity Hub to install the latest version of Unity Editor.
2. **Install Visual Studio:** Install Visual Studio for writing C# scripts. Make sure to add the Game Development with Unity workload during installation.

### **Step 2: Create a New Project**

1. **Open Unity Hub:** Click on the “New” button to create a new project.
2. **Select 3D Template:** Choose the 3D template and name your project “Roll-a-Ball.”
3. **Create the Project:** Click “Create” to open your new project.

### **Step 3: Create the Game Objects**

1. **Create the Ground:** In the Hierarchy window, right-click and select `3D Object > Plane`. Rename it to “Ground” in the Inspector window.
2. **Create the Player:** In the Hierarchy window, right-click and select `3D Object > Sphere`. Rename it to “Player”.
3. **Adjust the Player:** Set the position of the Player to `(0, 0.5, 0)` in the Inspector window.

### **Step 4: Add Materials to Objects**

1. **Create Materials:** In the Assets folder, right-click and create a new folder named “Materials.” Inside this folder, right-click and create two new materials, naming them “GroundMat” and “PlayerMat.”
2. **Assign Colors:** Choose different colors for each material and drag the materials onto their respective objects.

### **Step 5: Create Player Movement Script**

1. **Create Script:** In the Assets folder, right-click and create a new folder named “Scripts.” Inside this folder, create a new C# script and name it “PlayerController.”
2. **Edit Script:** Double-click the script to open it in Visual Studio and replace the existing code with the following:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float speed = 10f;

    void FixedUpdate()
    {
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");

        Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);

        GetComponent<Rigidbody>().AddForce(movement * speed);
    }
}
```

1. **Attach Script:** Drag the script onto the Player object in the Hierarchy window. Also, add a Rigidbody component to the Player object via the Inspector window.

### **Step 6: Create Collectible Items**

1. **Create Items:** In the Hierarchy window, right-click and select `3D Object > Cube`. Rename it to “Collectible.”
2. **Adjust the Item:** Scale and position the cube as needed. Duplicate it to create multiple items around the ground.
3. **Add a Trigger Collider:** Ensure the Collectible items have a Box Collider component set as a trigger in the Inspector window.

### **Step 7: Detect Item Collection**

1. **Create Script:** In the Scripts folder, create a new C# script and name it “CollectibleController.”
2. **Edit Script:** Double-click the script to open it in Visual Studio and replace the existing code with the following:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CollectibleController : MonoBehaviour
{
    void OnTriggerEnter(Collider other)
    {
        if (other.gameObject.CompareTag("Player"))
        {
            gameObject.SetActive(false);
        }
    }
}
```

1. **Attach Script:** Drag the script onto each Collectible item in the Hierarchy window. Also, set the tag of the Player object to “Player.”

### **Step 8: Play the Game**

1. **Save the Scene:** Save your scene with `File > Save Scene` and name it “Main.”
2. **Play the Game:** Click the Play button in Unity to test your game. Use the arrow keys or WASD keys to move the player and collect items.

## *Note for Android:*

Let’s focus on making your game motion-controlled and adding the restart functionality. Here’s the revised approach:

### **Step 1: Modify the Player Movement for Motion Control**

1. **Open PlayerController Script:** Edit your `PlayerController` script to utilize accelerometer input for Android motion controls. Here’s the updated code:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float speed = 10f;

    void FixedUpdate()
    {
        Vector3 movement = new Vector3(Input.acceleration.x, 0.0f, Input.acceleration.y);
        GetComponent<Rigidbody>().AddForce(movement * speed);
    }
}
```

### **Step 2: Implement Restart Mechanism**

1. **Open GameController Script:** Modify your `GameController` script to handle the restart functionality. Here’s the updated code:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameController : MonoBehaviour
{
    private bool isPlaying;
    private float fallTime;
    private bool ballFell;

    void Start()
    {
        isPlaying = true;
        ballFell = false;
    }

    void Update()
    {
        if (ballFell && Time.time - fallTime > 2)
        {
            // Show Restart UI
            // Implement the code to show a pop-up for restarting
            ShowRestartUI();
        }
    }

    public void BallFell()
    {
        isPlaying = false;
        fallTime = Time.time;
        ballFell = true;
    }

    public void RestartGame()
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }

    private void ShowRestartUI()
    {
        // Implement the code to show a pop-up for restarting
        Debug.Log("Restart UI Shown");
    }
}
```

1. **Modify PlayerController Script:** Update the `PlayerController` script to detect when the ball falls and inform the `GameController`.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float speed = 10f;
    private GameController gameController;

    void Start()
    {
        gameController = GameObject.Find("GameController").GetComponent<GameController>();
    }

    void FixedUpdate()
    {
        Vector3 movement = new Vector3(Input.acceleration.x, 0.0f, Input.acceleration.y);
        GetComponent<Rigidbody>().AddForce(movement * speed);

        if (transform.position.y < -10) // Arbitrary height for falling
        {
            gameController.BallFell();
        }
    }
}
```

### **Step 4: Build and Deploy to Android**

### **Setting Up Unity for Android**

1. **Install Android Build Support:**
    - Open Unity Hub.
    - Go to Installs.
    - Find your Unity version and click on the three dots next to it.
    - Select `Add Modules`.
    - Check `Android Build Support` and click `Install`.

### **Configuring Project for Android**

1. **Switch Platform:**
    - Open your project in Unity.
    - Go to `File > Build Settings`.
    - Select `Android` and click `Switch Platform`.
2. **Player Settings:**
    - In the Build Settings window, click `Player Settings`.
    - Configure settings like `Company Name`, `Product Name`, `Version`, and `Package Name` (e.g., com.example.mygame).
    - Under `Other Settings`, set the Minimum API Level and Target API Level.

### **Building the APK**

1. **Build the Game:**
    - In the Build Settings window, click `Build`.
    - Choose a location to save your APK file and click `Save`.
    - Unity will build the APK file. This might take a few minutes.

### **Deploying to Android Device**

1. **Enable Developer Mode on Android:**
    - Go to `Settings > About Phone`.
    - Tap `Build Number` 7 times to enable Developer Mode.
    - Go to `Settings > Developer Options` and enable `USB Debugging`.
2. **Transfer APK to Android:**
    - Connect your Android device to your computer via USB.
    - Transfer the APK file to your device.
3. **Install APK:**
    - On your Android device, open a File Manager and navigate to the APK file.
    - Tap the APK file to install it.
4. **Run the Game:**
    - Once installed, find the app icon on your Android device and launch it.
    - Enjoy your motion-controlled game!
