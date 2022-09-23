# Delete snapshots older than retention period
import boto3
from botocore.exceptions import ClientError
from datetime import datetime,timedelta
def delete_snapshot(snapshot_id):
    print(f"Deleting snapshot {snapshot_id} ")
try:
        ec2resource = boto3.resource('ec2')
        snapshot = ec2resource.Snapshot(snapshot_id)
        snapshot.delete()
    except ClientError as e:
        print(f"Caught exception: {e}")
return
def lambda_handler(event, context):
# Get current timestamp in UTC
    now = datetime.now()    
    
    # AWS Account ID
    account_id = '111111111111'
# Define retention period in days
    retention_days = 10
# Create EC2 client
    ec2 = boto3.client('ec2')
# Filtering by snapshot timestamp comparison is not supported
    # So we grab all snapshot id's
    result = ec2.describe_snapshots( OwnerIds=[account_id] )
for snapshot in result['Snapshots']:
        print(f"Checking snapshot {snapshot['SnapshotId']} which was created on {snapshot['StartTime']}")
# Remove timezone info from snapshot in order for comparison to work below
        snapshot_time = snapshot['StartTime'].replace(tzinfo=None)
# Subtract snapshot time from now returns a timedelta 
        # Check if the timedelta is greater than retention days
        if (now - snapshot_time) > timedelta(retention_days):
            print(f"Snapshot is older than configured retention of {retention_days} days")
            delete_snapshot(snapshot['SnapshotId'])
        else:
            print(f"Snapshot is newer than configured retention of {retention_days} days, so we keep it")
