using UnityEngine;

public class GameController : MonoBehaviour
{
    [Header("PLAYER SETTINGS")]
    public float moveSpeed = 5f;
    public float mouseSensitivity = 2f;

    [Header("FLASHLIGHT")]
    public Light flashlight;
    public float battery = 100f;
    public float batteryDrain = 5f;

    [Header("WEAPON")]
    public float shootRange = 50f;
    public int damage = 50;

    float xRotation = 0f;

    void Update()
    {
        HandleMovement();
        HandleMouseLook();
        HandleFlashlight();
        HandleShooting();
    }

    void HandleMovement()
    {
        float moveX = Input.GetAxis("Horizontal");
        float moveZ = Input.GetAxis("Vertical");

        transform.Translate(new Vector3(moveX, 0, moveZ) * moveSpeed * Time.deltaTime);
    }

    void HandleMouseLook()
    {
        float mouseX = Input.GetAxis("Mouse X") * mouseSensitivity;
        float mouseY = Input.GetAxis("Mouse Y") * mouseSensitivity;

        xRotation -= mouseY;
        xRotation = Mathf.Clamp(xRotation, -80f, 80f);

        Camera.main.transform.localRotation = Quaternion.Euler(xRotation, 0f, 0f);
        transform.Rotate(Vector3.up * mouseX);
    }

    void HandleFlashlight()
    {
        if (battery > 0)
        {
            battery -= batteryDrain * Time.deltaTime;
            flashlight.intensity = 1;
        }
        else
        {
            flashlight.intensity = 0;
        }
    }

    void HandleShooting()
    {
        if (Input.GetButtonDown("Fire1"))
        {
            RaycastHit hit;

            if (Physics.Raycast(Camera.main.transform.position,
                                Camera.main.transform.forward,
                                out hit, shootRange))
            {
                ZombieAI zombie = hit.transform.GetComponent<ZombieAI>();

                if (zombie != null)
                {
                    zombie.TakeDamage(damage);
                }
            }
        }
    }
}

/////////////////////////////////////////////////////////////

public class ZombieAI : MonoBehaviour
{
    public Transform player;
    public float speed = 2f;
    public int health = 100;

    void Update()
    {
        if (player == null) return;

        transform.LookAt(player);

        transform.position = Vector3.MoveTowards(
            transform.position,
            player.position,
            speed * Time.deltaTime
        );
    }

    public void TakeDamage(int damage)
    {
        health -= damage;

        if (health <= 0)
        {
            Destroy(gameObject);
        }
    }
}
