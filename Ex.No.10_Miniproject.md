# Ex.No: 10  Implementation of 2D/3D game 
### DATE:                                                                            
### REGISTER NUMBER : 212222240053
### AIM: 
To develop a 2D Fruit Catcher Game using Unity Engine. 
### Algorithm:
```
1. Start Unity Hub and create a new project using the 2D Template.

Design the scene by adding:

2. A basket (player object) with collider for catching fruits.

3. Falling fruit prefabs with Rigidbody2D and collider components.

4. A UI canvas for score, lives, and game-over text.

Create and attach scripts to handle:

5. Basket movement using arrow keys (PlayerController.cs).

6. Fruit falling behavior and collision detection (Fruit.cs).

7. Game management, score, and lives (GameManager.cs).

8. Spawning fruits at random positions (FruitSpawner.cs).

Set up prefabs and colliders:

9. Assign box collider to basket.

10. Assign circle collider and Rigidbody2D to fruits.

11. Adjust collider sizes and gravity to control speed.

Implement UI Logic:

12. Update score when a fruit is caught.

13. Decrease life when a fruit hits the ground.

14. Display “Game Over” panel when lives = 0.

Test and Debug:

15. Ensure fruits fall at consistent speed.

16. Confirm UI updates correctly.

17. Test restart button functionality.

Build the Game:

18. Export the final game for PC or Android.
```  
### Program:

### PlayerController.cs
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float speed = 7f;
    public float xLimit = 7.5f; // set to fit your camera view

    void Update()
    {
        float move = Input.GetAxis("Horizontal"); // A/D or Left/Right
        Vector3 pos = transform.position;
        pos.x += move * speed * Time.deltaTime;
        pos.x = Mathf.Clamp(pos.x, -xLimit, xLimit);
        transform.position = pos;
    }
}

```
### Fruit.cs
```
using UnityEngine;

public class Fruit : MonoBehaviour
{
    void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Player"))
        {
            GameManager.instance.AddScore(1);
            Destroy(gameObject);
        }
        else if (other.CompareTag("Ground"))
        {
            GameManager.instance.LoseLife(1);
            Destroy(gameObject);
        }
    }
}

```
### GameManager.cs
```
using UnityEngine;
using UnityEngine.SceneManagement;
using TMPro;
using UnityEngine.UI;

public class GameManager : MonoBehaviour
{
    public static GameManager instance;
    public TMP_Text scoreText;
    public TMP_Text livesText;
    public GameObject gameOverPanel;
    public Button restartButton;
    public int lives = 3;
    private int score = 0;

    void Awake()
    {
        if (instance == null) instance = this;
        else Destroy(gameObject);
    }

    void Start()
    {
        UpdateUI();
        Time.timeScale = 1f;
    }

    public void AddScore(int amount)
    {
        score += amount;
        UpdateUI();
    }

    public void LoseLife(int amount)
    {
        lives -= amount;
        UpdateUI();
        if (lives <= 0) GameOver();
    }

    void UpdateUI()
    {
        if (scoreText != null) scoreText.text = "Score: " + score;
        if (livesText != null) livesText.text = "Lives: " + lives;
    }

    void GameOver()
    {
        Time.timeScale = 0f; // pause game
        if (gameOverPanel != null) gameOverPanel.SetActive(true);
    }

    // Hook this to Restart button
    public void RestartGame()
    {
        Time.timeScale = 1f;
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }
}

```
### FruitSpawner.cs
```
using UnityEngine;

public class FruitSpawner : MonoBehaviour
{
    public GameObject[] fruitPrefabs;
    public float spawnInterval = 1.2f;
    public float xRange = 7f;
    private float timer;

    void Update()
    {
        timer += Time.deltaTime;
        if (timer >= spawnInterval)
        {
            float x = Random.Range(-xRange, xRange);
            Vector2 spawnPos = new Vector2(x, 6f);
            int randomIndex = Random.Range(0, fruitPrefabs.Length);
            Instantiate(fruitPrefabs[randomIndex], spawnPos, Quaternion.identity);
            timer = 0f;
        }
    }
}

```
### Output:

<img width="1915" height="1018" alt="image" src="https://github.com/user-attachments/assets/53130d4b-8f80-44ea-8173-61538a287466" />

<img width="1920" height="1080" alt="Screenshot (384)" src="https://github.com/user-attachments/assets/7ad621b4-d54b-43c6-a5e8-2e73ebfbcbcb" />

<img width="1919" height="1020" alt="Screenshot 2025-11-08 195635" src="https://github.com/user-attachments/assets/9642f440-e342-494a-8f69-460429c50fe1" />


<img width="1919" height="1022" alt="image" src="https://github.com/user-attachments/assets/cf8766c9-e54b-418a-9923-c885a5d5c9fd" />


### Result:
Thus, the 2D Fruit Catcher Game was successfully developed using Unity Engine, and it adopted basic Artificial Intelligence concepts such as object collision detection and game state management.
