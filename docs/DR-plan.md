# Disaster Recovery plan

There are a few steps and instructions to follow to be able to recover successfully

## Backups overview

The most important part to backup is the data on your NAS, you can also backup the cluster's ETCD and PV/PVCs states.
So please maintain the following backups for a doomsday:

* NAS data backup - a must! at lease one of:
  * RAID
  * Copies to BU HDD
  * Copy to S3 Bucket
* Longhorn PV/PVCs backups - recommended:
  * Snapshots for a quick recovery in case of local failure
  * Backups for a bull recovery in case of general cluster failure
    * you can set the above both in LH's UI or in defining annotations for recurring jobs
* ETCD state backup - can be stored in S3

### Backups flow
<div align="center">
<img src="https://github.com/ilanKushnir/kws-cluster/blob/main/docs/diagrams/backups-flow.png?raw=true" align="center" width="600"/>
</div>
<br/>

<br/>

## 📝 TODOs

* Copy NAS data to S3 Bcuket / Glacier
V Define three BU groups for LH (`low`,`medium`,`high` )
V Define recurring jobs for each group `snapshots` & `backups`
V Plan an automatic flow
* add current backup layers flow to readme (draw.io diagram)

## 🚶 Recovery steps - General cluster failure
1. Reinstall OS on SSDs (`Ubuntu 22.04`)
2. Follow instructions.md
   * Prepare Ubuntu with ansible
   * Prepare K3S with ansible
3. Git repo
   * Set a backup branch on the current repo state (last commit)
   * Reset main branch to Longhorn deployment step
4. Continue instructions.md
   * GitOps with Flux
5. Run Longhorn UI
   * Mount disks
   * Make them schedulable
   * Make sure you can access the S3 backups
6. Bug fix: blacklist `multipathd` (only if/after deploying Longhorn) [source](https://longhorn.io/kb/troubleshooting-volume-with-multipath/)
   * ssh into every node and do the following:
     * `sudo nano /etc/multipath.conf`
     * add: `blacklist { devnode "^sd[a-z0-9]+" }`
     * Restart multipath service: `systemctl restart multipathd.service`
     * Verify that configuration is applied: `multipath -t`
7. Git repo - Reset main branch to current state
   * if it fails, reset step by step
8. Recover PV/PVCs, same steps for each deployment
   * Deploy and make sure its running properly
   * Scale to `0`
   * Delete `PVC`
   * Restore `PVC` from backup with original name
   * When `PVC` restored and detached -> click `create PV claim` -> `OK`
   * Scale back up and make sure the `PVC` is `Attached` & `Bound`

<br />

> In minor cluster failures you can always try to recover from snapshots without restarting the cluster
