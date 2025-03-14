1️⃣ Create an Automated etcd Backup Script
Create a backup script and save it as /opt/etcd-backup.sh:

#!/bin/bash
# 📌 Set backup directory and filename with timestamp
BACKUP_DIR="/backup"
TIMESTAMP=$(date +%Y%m%d-%H%M%S)  # Generates a timestamp (YYYYMMDD-HHMMSS)
SNAPSHOT_FILE="$BACKUP_DIR/etcd-snapshot-$TIMESTAMP.db"  # Full path for the snapshot file

# 📌 Create backup directory if it doesn't exist
mkdir -p $BACKUP_DIR

# 📌 Take etcd snapshot and store it in the specified backup directory
ETCDCTL_API=3 etcdctl snapshot save $SNAPSHOT_FILE \
  --endpoints=https://127.0.0.1:2379 \  # Connects to the etcd server running locally
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \  # Uses CA certificate for secure communication
  --cert=/etc/kubernetes/pki/etcd/server.crt \  # Uses etcd server certificate
  --key=/etc/kubernetes/pki/etcd/server.key  # Uses etcd server private key

# 📌 Delete old backups (keep only the last 7 backups)
find $BACKUP_DIR -type f -name "etcd-snapshot-*.db" -mtime +7 -exec rm {} \;  # Deletes files older than 7 days

# 📌 Upload backup to AWS S3 for remote storage
aws s3 cp $SNAPSHOT_FILE s3://my-etcd-backups/  # Replace 'my-etcd-backups' with your actual S3 bucket name

# 📌 Log the backup completion status
echo "Backup completed: $SNAPSHOT_FILE" >> /var/log/etcd-backup.log  # Logs the backup in /var/log


🔹 What this script does?

📌 Creates a timestamped etcd snapshot.
📌 Stores it in /backup directory.
📌 Deletes backups older than 7 days.
📌 Logs the backup status.

,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,

2️⃣ Schedule the Script Using Cron Job
To run this script daily at midnight, create a cron job:

crontab -e
Then, add this line at the bottom:

0 0 * * * /bin/bash /opt/etcd-backup.sh
✅ This will run the script every day at midnight (00:00 UTC).

,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,,

3️⃣: Restore the etcd Snapshot

Step 1️⃣: Download the Latest Backup from S3
Since your backups are stored in AWS S3, first, retrieve the most recent backup:

bash
Copy
Edit
aws s3 cp s3://my-etcd-backups/etcd-snapshot-YYYYMMDD-HHMMSS.db /backup/
📌 Replace YYYYMMDD-HHMMSS with the latest available timestamp.

Step 2️⃣: Stop etcd Service
Before restoring, stop the running etcd process to prevent conflicts:

bash
Copy
Edit
systemctl stop etcd
or if running inside Kubernetes as a static pod:

bash
Copy
Edit
kubectl delete pod -n kube-system -l component=etcd
Kubernetes will restart etcd automatically later.

Step 3️⃣: Restore the etcd Snapshot
Run the following command to restore the etcd database:

bash
Copy
Edit
ETCDCTL_API=3 etcdctl snapshot restore /backup/etcd-snapshot-YYYYMMDD-HHMMSS.db \
  --data-dir /var/lib/etcd-new

,,,,,,,,,,,,,,,,,,,,,,,,,,,,,
,,,,,,,,,,,,,,,,,,,,,,,,,,,,,

🔥 Summary: Fully Automated etcd Backups
✔ Backup script (/opt/etcd-backup.sh) ✅
✔ Cron job (Runs daily at midnight) ✅
✔ Old backups deleted automatically ✅
✔ Optionally upload to S3/GCS ✅