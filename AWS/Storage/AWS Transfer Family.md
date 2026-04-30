
AWS Transfer Family is a fully managed, highly available, and scalable service that enables you to **transfer files into and out of AWS storage services** over multiple protocols. It provides a secure and reliable way to exchange files with external partners, vendors, customers, or internal teams without the need to manage any underlying server infrastructure. 

Transfer Family supports a variety of protocols and can connect to multiple AWS storage services. 

Supported protocols
- **Secure File Transfer Protocol (SFTP):** A secure way to transfer files over an encrypted channel using SSH.
- **File Transfer Protocol Secure (FTPS):** Provides encryption for FTP using SSL/TLS.
- **File Transfer Protocol (FTP):** The standard, unencrypted file transfer protocol.
- **Applicability Statement 2 (AS2):** A standard for securely exchanging business-to-business (B2B) messages over HTTP/S.
- **Web apps:** A no-code, web browser-based interface for end-users to securely transfer files to and from Amazon S3. 

Supported storage services

- **Amazon S3:** Stores transferred files as objects in an S3 bucket.
- **Amazon EFS:** Stores transferred files in an Amazon Elastic File System (EFS) file system, providing a traditional file-based interface. 

#### Application
Migration between AWS storage and on-premise