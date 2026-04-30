A data warehouse is a relational OLAP database

Redshift is Amazon’s managed data warehouse service. Although it’s based on PostgreSQL, it’s not part of RDS
column-base storage 
compression


node type of Redshift

Amazon Redshift Spectrum resides on dedicated Amazon Redshift servers that are independent of your cluster.

A Redshift cluster contains one or more compute nodes that are divided into two categories.  
Dense compute nodes can store up to 326 TB of data on fast SSDs. Dense storage nodes can  
store up to 2 PB of data on magnetic hard disk drives. A faster alternative to dense storage  
nodes are the RA3 nodes, which offer up to 16,384 TB of storage on fast SSDs.  
If your cluster contains more than one compute node, Redshift also includes a leader node  
to coordinate communication among the compute nodes, as well as to communicate with  
clients. A leader node doesn’t incur any additional charges.



 RedShift can also improve performance for repeat queries by caching the result and returning the cached result when queries are re-run. Dashboard, visualization, and business intelligence (BI) tools that execute repeat queries see a significant boost in performance due to result caching

- > Amazon RedShift Enhanced VPC routing forces all COPY and UNLOAD traffic between clusters and data repositories through a VPC