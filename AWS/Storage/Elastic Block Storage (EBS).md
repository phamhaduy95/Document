EBS volumes can be used only for a single instance at a time

Lưu ý: EBS 

EBS volumes are automatically replicated _within_ a single Availability Zone to protect against hardware failure
By default, an EBS volume can only be attached to an EC2 instance in the same Availability Zone.
==**EBS volumes are private**==. EBS volumes are designed to be used as private, network-attached block storage for a single Amazon EC2 instance within the same Availability Zone. They are not directly accessible over the public internet.

#### Resiliency
- EBS volumes copied across regions (AWS DataSync)  
- Snapshots copied across regions (Amazon Data Lifecycle Manager)  
- Multi-attach EBS volumes (io1)

You can take a snapshot of an EBS volume while the instance is running and it does not cause any outage of the volume so it can continue to be used as normal. However, the advice is that to take consistent snapshots writes to the volume should be stopped. For non-root EBS volumes this can entail taking the volume offline (detaching the volume with the instance still running), and for root 

The possible values are ok, impaired, warning, or insufficient-data. If all checks pass, the overall status of the volume is ok. If the check fails, the overall status is impaired. If the status is insufficient-data, then the checks may still be taking place on your volume at the time

Snapshots capture a point-in-time state of an instance and are stored on S3. To take a consistent snapshot writes must be stopped (paused) until the snapshot is complete – if not possible the volume needs to be detached, or if it’s an EBS root volume the instance must be stopped


#### Instance Volume vs EBS Volume

- Instance store volumes are sometimes called Ephemeral storage (non-persistent)
- Instance store volumes cannot be stopped. If the underlying host fails the data will be lost
- Instance store volume root devices are created from AMI templates stored on S3
- Instance store volumes cannot be detached/reattached


#### Attach and Detach EBS Volume

user có thể attach nhiều EBS volume vào trong 1 EC2. Thông thường ta sẽ cho 1 volume làm root volume chứa các file và data quan trong của OS. Các volume còn lại làm non-root volume.

Detach EBS ta cần quan tâm đến việc volume đó có phải root không

- Với root volume ta không thể detach EBS khi EC2 instance đang chạy. Ta chỉ có thể detach khi EC2 instance được stop hoán toàn.
- Với non-root volume ta có thể detach volume đó ngay cả khi đang chạy. Lưu ý, cần dừng các hoạt động đọc ghi dữ liệu để tránh hiện tượng data corruption



