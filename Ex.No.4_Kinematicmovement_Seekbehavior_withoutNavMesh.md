# Ex.No: 4  Implementation of Kinematic movement -seek behavior in Unity
### DATE:                                                                            
### REGISTER NUMBER : 212222240053
### AIM: 
To write a program to simulate the process of seek behavior in Unity without NavigationMeshAgent. 
### Algorithm:
1. Create a New Unity Project by Open the  Unity Hub and create a new 3D Project,Name the project (e.g., SeekBehaviorDemo).
2. Create the Moving Object
   In the Hierarchy, right-click → 3D Object → Cube (or Sphere).
   Rename it to Seeker and Reset its position:Select the Seeker, go to Inspector → Transform → Set Position to (0,0,0).
3. Create the Target Object
   Right-click in the Hierarchy → 3D Object → Sphere (or any other shape).
   Rename it to Target. Move it away from Seeker, e.g., set Position to (5, 0, 5).
   Select the Target, add a Material, and change the color. (if needed) 
4. Adding the Seek Behavior Script
   Create the Script-In the Project Window, go to the Assets folder.
   Right-click → Create → C# Script.
5. Write a script for seek behavior and save it
6. Attach the Script
   Select Seeker in the Hierarchy - Drag & Drop the SeekBehavior script onto the Inspector Panel.
   Drag & Drop the Target from the Hierarchy into the "Target" field in the script component.
12. Run the game 
13. Stop the program
    
### Program:

### Seek
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class seek : MonoBehaviour
{
    // Start is called before the first frame update
    public Transform target;
    public float speed;
    void Start()
    {
    
    }

    // Update is called once per frame
    void Update()
    {
        Vector3 direction = (target.position - transform.position).normalized;
        transform.position += direction * speed * Time.deltaTime;
    }
}

```
### Target
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class flee_behavior: MonoBehaviour
{
    public Transform t;
    public float speed=0.3f;
    void Start()
    {
    
    }
    // Update is called once per frame
    void Update()
    {
        Vector3 direction = (transform.position - t.position).normalized;
        transform.position += direction * speed * Time.deltaTime;
    }
}

```
### Output:

<img width="1920" height="1080" alt="Screenshot (341)" src="https://github.com/user-attachments/assets/51730e09-85f6-488a-9a15-b9d71c993c79" />

<img width="1920" height="1080" alt="Screenshot (342)" src="https://github.com/user-attachments/assets/eaf9bb46-57f5-4e5f-a514-71238a91e1b8" />

<img width="1920" height="1080" alt="Screenshot (344)" src="https://github.com/user-attachments/assets/3a90383e-cbd0-43ce-b725-f2ff49802159" />



### Result:
Thus the simple seek behavior was implemented successfully.
