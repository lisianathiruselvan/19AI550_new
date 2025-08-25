# Ex.No: 3  Basic movements in Unity 
### DATE:  12-08-25                                                                          
### REGISTER NUMBER : 212222240053
### AIM: 
 To learn the basic movements translation,scaling and rotation of game objects through code.
### Procedure:
1. Setup the Scene
2. Open Unity and create a 3D Scene.
3. Add three objects:Cube → Rename to Object1 (for movement),Sphere → Rename to Object2 (for rotation).Capsule → Rename to Object3 (for scaling).
4. Add the Script,Create a C# Script → Name it TransformOperations.cs.
5. Write the code for translation,scaling and rotation,save and close the script
6. Save the script
7. Select any empty GameObject (or create one: GameObject → Create Empty).
8. Attach the TransformOperations script to it.
9. In the Inspector, assign Object1 → Drag the Cube,Object2 → Drag the Sphere.Object3 → Drag the Capsule.
10. Run the Scene Press Play ▶️ in Unity
11. Stop the program.
### Program 
```
using UnityEngine;
public class TransformOperations : MonoBehaviour
{
    public Transform o1;
    public Transform o2;
    public Transform o3;
    void Start()
    {
    

    }

    // Update is called once per frame
    void Update()
    {
       o1.Translate(0.2f, 0, 0);
       o2.Rotate(0.2f, 0, 0);
       o3.localScale += new Vector3(0, 0.2f, 0);
    }
}
```
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour
{
    public Transform o1;
    public Transform o2;
    public Transform o3;
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        if (Input.GetKeyUp(KeyCode.X))
        {
            o1.Translate(0.2f, 0, 0);
        }
        if (Input.GetKeyUp(KeyCode.Z))
        {
            o3.localScale += new Vector3(0, 0.2f, 0);
        }
    }

}
```

### Output:

### Output 1: 
<img width="1920" height="1019" alt="Screenshot 2025-08-23 141920" src="https://github.com/user-attachments/assets/5edc81a0-32f0-4f03-a454-0dac3debee93" />


<img width="1920" height="1080" alt="Screenshot (364)" src="https://github.com/user-attachments/assets/931a9d6e-59ee-4c41-be02-a1a79c9e6b9c" />

### Output 2:

<img width="1919" height="1017" alt="Screenshot 2025-08-23 142300" src="https://github.com/user-attachments/assets/9099d547-2330-4183-9216-ba8dc899e30d" />

<img width="1916" height="1020" alt="Screenshot 2025-08-23 142316" src="https://github.com/user-attachments/assets/56944255-d082-478d-9a95-1b2a52cbf4d8" />


### Result:
Thus the basic movement is learned through scripting.
