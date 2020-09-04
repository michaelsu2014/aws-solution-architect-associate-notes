## What is Disaster Recovery?

Disaster recovery (DR) is about preparing for and recovering from a disaster

Need to define two terms:
- RPO: Recovery Point Objective
- RTO: Recovery Time Objective

## DR Strategies

- Backup and Restore
    - In most traditional environments, data is backed up to tape and sent off-site regularly. If you use this method, it can take a long time to restore your system in the event of a disruption or disaster. Amazon S3 is an ideal destination for backup data that might be needed quickly to perform a restore. 
- Pilot Light
    - A small version of the app is always running in the cloud
    - Useful for the **critical** core (pilot light)
    - Very similar to Backup and Restore
    - Faster than Backup and Restore as critical systems are already up
- Warm Standby
    - Full system is up and running, but at minimum size
        - **a scaled-down version** of a fully functional environment is always running in the cloud
    - Upon disaster, we can scale to production load
- Hot Site / Multi Site Approach
    - Very low RTO (minutes or seconds) – very expensive
    - Full Production Scale is running AWS and On Premise
    
https://d1.awsstatic.com/asset-repository/products/CloudEndure/CloudEndure_Affordable_Enterprise-Grade_Disaster_Recovery_Using_AWS.pdf
