# Ex.No: 5  Implementation of Steering behaviour-Pursue and Evade in Unity
### DATE:                                                                            
### REGISTER NUMBER : 212222240053
### AIM: 
To write a program to simulate the process of Pursue and Evade behavior in Unity using NavigationMeshAgent. 
### Algorithm:
1. Create a New Unity Project by Open the  Unity Hub and create a new 3D Project.
2. Name the project "SteeringBehaviors" and select a location. Click Create.
3.Open Unity Scene (default is SampleScene).
  In the Hierarchy, create a Plane:
  Right-click → 3D Object → Plane (this will be the ground).
  Set its Scale to (10, 1, 10) for a larger surface.
  Create three Capsule for the Player, Pursuer, and Evader:
  Rename them to "Player", "Pursuer", and "Evader".
  Set their Y Position to 0.5 (so they sit on the ground).
  Change their Material for better distinction (optional).
3. Add NavMesh and Bake
   Window → AI → Navigation (opens the Navigation tab).
   Select the Plane, go to the Navigation tab, and mark it as Navigation Static.
   Go to the Bake tab and click Bake.
   or
   Add navMeshSurface to plane and bake 
4. Add NavMeshAgent Component
    Select Pursuer, and Evader.
    Click Add Component → Search for NavMeshAgent and add it.
    Adjust NavMeshAgent Settings:
    Player: Set Speed = 5.
    Pursuer: Set Speed = 4.
    Evader: Set Speed = 6.
5. Write a script for  Player_movement behavior and save it

### Bot Script
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class Bot : MonoBehaviour {

    public GameObject target;
    public GameObject sphere;

    GameObject jitter;

    NavMeshAgent agent;
    Drive ds;
    Vector3 wanderTarget = Vector3.zero;

    float q = 0.0f;

    bool coolDown = false;


    void Start() {

        agent = GetComponent<NavMeshAgent>();
        ds = target.GetComponent<Drive>();
        jitter = Instantiate(sphere);
    }

    void Seek(Vector3 location) {

        agent.SetDestination(location);
    }

    void Flee(Vector3 location) {

        Vector3 fleeVector = location - transform.position;
        agent.SetDestination(transform.position - fleeVector);
    }
    void Pursue() {

        Vector3 targetDir = target.transform.position - transform.position;
        float relativeHeading = Vector3.Angle(transform.forward, transform.TransformVector(target.transform.forward));
        float toTarget = Vector3.Angle(transform.forward, transform.TransformVector(targetDir));


        if ((toTarget > 90.0f && relativeHeading < 20.0f) || ds.currentSpeed < 0.01f) {

            // Debug.Log("SEEKING");
            Seek(target.transform.position);
            return;
        }

        // Debug.Log("LOOKING AHEAD");
        float lookAhead = targetDir.magnitude / (agent.speed + ds.currentSpeed);
        Seek(target.transform.position + target.transform.forward * lookAhead);
    }

    void Evade() {

        Vector3 targetDir = target.transform.position - transform.position;
        float lookAhead = targetDir.magnitude / (agent.speed + ds.currentSpeed);
        Flee(target.transform.position + target.transform.forward * lookAhead);
    }

    void Wander() {

        float wanderRadius = 10.0f;
        float wanderDistance = 20.0f;
        float wanderJitter = 1.0f;

        wanderTarget += new Vector3(
            Random.Range(-1.0f, 1.0f) * wanderJitter,
            0.0f,
            Random.Range(-1.0f, 1.0f));
            wanderTarget.Normalize();
            wanderTarget *= wanderRadius;

        Vector3 targetLocal = wanderTarget + new Vector3(0.0f, 0.0f, wanderDistance);
        Vector3 targetWorld = gameObject.transform.InverseTransformVector(targetLocal);

        Debug.DrawLine(transform.position, targetWorld, Color.red);
        jitter.transform.position = targetWorld;
        Seek(targetWorld);
    }

    void Hide() {

        float dist = Mathf.Infinity;
        Vector3 chosenSpot = Vector3.zero;

        for (int i = 0; i < World.Instance.GetHidingSpots().Length; ++i) {

            Vector3 hideDir = World.Instance.GetHidingSpots()[i].transform.position - target.transform.position;
            Vector3 hidePos = World.Instance.GetHidingSpots()[i].transform.position + hideDir.normalized * 10.0f;

            if (Vector3.Distance(transform.position, hidePos) < dist) {

                chosenSpot = hidePos;
                dist = Vector3.Distance(transform.position, hidePos);
            }
        }

        Seek(chosenSpot);
    }

    void CleverHide() {

        float dist = Mathf.Infinity;
        Vector3 chosenSpot = Vector3.zero;
        Vector3 chosenDir = Vector3.zero;
        GameObject chosenGO = World.Instance.GetHidingSpots()[0];

        for (int i = 0; i < World.Instance.GetHidingSpots().Length; ++i) {

            Vector3 hideDir = World.Instance.GetHidingSpots()[i].transform.position - target.transform.position;
            Vector3 hidePos = World.Instance.GetHidingSpots()[i].transform.position + hideDir.normalized * 10.0f;

            if (Vector3.Distance(transform.position, hidePos) < dist) {

                chosenSpot = hidePos;
                chosenDir = hideDir;
                chosenGO = World.Instance.GetHidingSpots()[i];
                dist = Vector3.Distance(transform.position, hidePos);
            }
        }

        Collider hideCol = chosenGO.GetComponent<Collider>();
        Ray backRay = new Ray(chosenSpot, -chosenDir.normalized);
        RaycastHit info;
        float distance = 100.0f;
        hideCol.Raycast(backRay, out info, distance);

        Seek(info.point + chosenDir.normalized * 2.0f);
    }

    bool CanSeeTarget() {

        RaycastHit raycastInfo;
        Vector3 rayToTarget = target.transform.position - transform.position;
        float lookAngle = Vector3.Angle(transform.forward, rayToTarget);
        if (lookAngle < 60.0f && Physics.Raycast(transform.position, rayToTarget, out raycastInfo)) {

            if (raycastInfo.transform.gameObject.tag == "cop") return true;
        }
        return false;
    }

    bool CanSeeMe() {

        Vector3 rayToTarget = transform.position - target.transform.position;
        float lookAngle = Vector3.Angle(target.transform.forward, rayToTarget);

        if (lookAngle < 60.0f) return true;
        return false;
    }

    void BehaviourCoolDown() {

        coolDown = false;
    }

    void Update() {

        // Seek(target.transform.position);
        // Flee(target.transform.position);
        // Pursue();
        // Evade();
        // Wander();
        // Hide();
        if (!coolDown) {

            if (CanSeeTarget() && CanSeeMe()) {

                CleverHide();
                coolDown = true;
                Invoke("BehaviourCoolDown", 5.0f);
            } else Pursue();
        }
    }
}
```
### Drive Script
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Drive : MonoBehaviour
{
    public float speed = 10.0f;
    public float rotationSpeed = 100.0f;
    public float currentSpeed = 0;

    void Update()
    {
        // Get the horizontal and vertical axis.
        // By default they are mapped to the arrow keys.
        // The value is in the range -1 to 1
        float translation = Input.GetAxis("Vertical") * speed;
        float rotation = Input.GetAxis("Horizontal") * rotationSpeed;

        // Make it move 10 meters per second instead of 10 meters per frame...
        translation *= Time.deltaTime;
        rotation *= Time.deltaTime;

        // Move translation along the object's z-axis
        transform.Translate(0, 0, translation);
        currentSpeed = translation;

        // Rotate around our y-axis
        transform.Rotate(0, rotation, 0);
    }
}
```
### World Script
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public sealed class World {

    static readonly World instance = new World();
    static GameObject[] hidingSpots;

    static World() {

        hidingSpots = GameObject.FindGameObjectsWithTag("hide");
    }

    World() { }


    public static World Instance { get { return instance; } }
    public GameObject[] GetHidingSpots() { return hidingSpots; }
}
```

### Output:

<img width="1920" height="1019" alt="{4768CEFD-6F5E-45DB-8E50-33690F76FCDD}" src="https://github.com/user-attachments/assets/51cb5333-3dd0-4ac2-9c2e-f1601a229ca6" />

<img width="1920" height="1080" alt="Screenshot (369)" src="https://github.com/user-attachments/assets/e5333a95-3ecb-44ff-85ab-814695e8a152" />

### Result:
Thus the simple pursue and evade behavior was implemented successfully.
