using UnityEngine;

public class TargetLookSystem : MonoBehaviour
{
    public enum AimZone { Body, Head }

    [Header("Target References")]
    public Transform bodyAnchor;
    public Transform headAnchor;

    [Header("Rotation Settings")]
    public AimZone currentZone = AimZone.Body;
    public float turnSpeedDegrees = 180f;

    void Update()
    {
        Transform target = (currentZone == AimZone.Head) ? headAnchor : bodyAnchor;
        if (target == null) return;

        Vector3 direction = target.position - transform.position;
        if (direction.sqrMagnitude < 0.0001f) return;

        Quaternion desiredRotation = Quaternion.LookRotation(direction.normalized);
        transform.rotation = Quaternion.RotateTowards(
            transform.rotation,
            desiredRotation,
            turnSpeedDegrees * Time.deltaTime
        );
    }
}
