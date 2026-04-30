atomic : ensure all operations within the scope of transaction to be successful. If one if them fail then database will perform rollback
consistency: ensure data integrity by complying to any constraint such as check or foreign key
Isolation: Isolation ensures that transactions do not interfere with each other when executed concurrently. This will prevent dirty read (accessing uncommitted data) or race condition 
durability: make sure data persisted durably and permanently when transaction is committed
