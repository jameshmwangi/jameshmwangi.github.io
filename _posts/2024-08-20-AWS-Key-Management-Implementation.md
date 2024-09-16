---
title: AWS Key Management Implementation
date: 2024-08-20 12:08:00 +0300
categories: [aws, cloud, encryption]
tags: [aws, encryption, keys, cloud, security]
---


 
[![awskmsblog1.png](https://i.postimg.cc/0NVxt0cx/awskmsblog1.png)](https://postimg.cc/PLvBJZgF)

AWS Key Management Implementation


Report by: James Mwangi  AKA Jamesh

[![image6.png](https://i.postimg.cc/FHTTrPrS/image6.png)](https://postimg.cc/gx6V4H6c)

Introduction
============

The AWS Key Management Service (KMS) provides secure storage and rotation of encryption keys with robust access control. For a cloud security engineer, it's crucial to have a comprehensive understanding of AWS KMS to ensure the security and integrity of data. Mastery of AWS KMS involves learning how to create, manage, and use cryptographic keys effectively within AWS services and applications. This entails understanding key policies, utilizing both customer-managed and AWS-managed keys, enabling automatic key rotation, and integrating KMS with other AWS services to ensure consistent enforcement of data encryption and access controls.
=================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================

Objective:
==========

Demonstrate the integration of AWS Key Management Service (KMS) with AWS services, such as Amazon S3 and Amazon EBS, to streamline the encryption and decryption process. This involves enabling data encryption within these services without requiring manual key management, showcasing how KMS simplifies secure data handling in cloud environments.
=========================================================================================================================================================================================================================================================================================================================================================

Lab implementation
==================

Login to your AWS Web console (root account) and create an IAM account.
=======================================================================

![](https://i.postimg.cc/RZZTDc7Z/image13.png)
=======================

I created an IAM user “kenya-one” and assigned the user admin privileges. Creating an IAM (Identity and Access Management) user account is recommended rather than using the root user account. An IAM user account provides a more secure way to access and manage AWS resources and services. By creating an IAM user account, you can specify the user’s permissions and restrict their access to only the necessary AWS resources and services.  All the activities will be done in the IAM account.

Click on Services. In the Search field, type KMS and then select Key Management Service. On the KMS Console page, click on Create Key.

[![image13.png](https://i.postimg.cc/RZZTDc7Z/image13.png)](https://postimg.cc/QHv5BKY2)

There are two types of encryption, asymmetric and symmetric

Symmetric encryption involves using a single key for both encryption and decryption of data. This key must be securely shared between the communicating parties. Symmetric encryption is faster and more computationally efficient compared to asymmetric encryption, making it suitable for bulk data encryption. Used for encrypting data at rest (eg, EBS volumes, S3 objects) and protecting data in transit (eg, between AWS services).

Asymmetric encryption, on the other hand, uses a pair of keys: a public key and a private key. The public key is used for encryption, while the private key is used for decryption. It enables secure communication and authentication without the need to share private keys, thus enhancing security in distributed systems and applications.  Used for Secure key exchange (eg, encrypting symmetric keys for secure transmission), digital signatures for data integrity and authentication, and SSL/TLS encryption for secure web traffic.

There are five steps to the creation of the KMS: In the Configure key (Step 1), retain the default settings. Click on Next to continue.

[![image23.png](https://i.postimg.cc/WbXGghBL/image23.png)](https://postimg.cc/hfQQqP4p)

Under Add Label (Step 2): Enter an alias of your choice as the name of your master key.

The description is optional. Click Next.

[![image15.png](https://i.postimg.cc/Hs22zS7N/image15.png)](https://postimg.cc/Xprd7kLK)

Define key administrative permissions (Step 3)

To allow users to perform encryption and decryption, I have to assign key permissions to them on the Define key administrative permissions page. Permit the user created in the previous lab to use the kenya-one-key by selecting the checkbox near the user. Here, we have selected the user kenya-one we created in the previous lab. Click on Next to continue.

[![image19.png](https://i.postimg.cc/QCDQk5Ns/image19.png)](https://postimg.cc/ftHSMV0g)

Define key usage permissions (Step 4) .                                                                                                  Here, you are trying to give the user key permission. You have the option to add another AWS user account. Select the user kenya-one and click on Next to continue.

[![image20.png](https://i.postimg.cc/Wz7M93tZ/image20.png)](https://postimg.cc/gLnXjGVc)

Review (Step 5)                                                                                                                                     Review all settings and the key policy, which is in JSON format, and then click FINISH to create the master key. Now we have successfully created an AWS KMS for the IAM user kenya-one

[![image1.png](https://i.postimg.cc/1z4JfpXj/image1.png)](https://postimg.cc/CZTj2njG)

Integrating the Created KMS to AWS services such as Amazon S3 and Amazon EBS

Let's start with Amazon S3 . In your AWS Management Console, type S3 in the search field to create a new bucket.  On your S3 bucket page. Click an S3 Create bucket. 
[![image16.png](https://i.postimg.cc/vm1tQbSR/image16.png)](https://postimg.cc/xk2z5D2g)

 The Create bucket popup appears. Under General Configuration, type the name of the bucket in the Bucket Name field -here, the bucket name is kenya-one-bucket.

[![image8.png](https://i.postimg.cc/26ZGxr4d/image8.png)](https://postimg.cc/jwRyxV3C)

Retain the default Object Ownership setting and block this bucket's public access settings.
===========================================================================================

[![image3.png](https://i.postimg.cc/Prz6cX02/image3.png)](https://postimg.cc/KRjr32GM)

Enable bucket versioning and  retain the default tags and default encryption settings. Then, click on Create Bucket.
 [![image7.png](https://i.postimg.cc/bvMLbs2N/image7.png)](https://postimg.cc/QBQpZNbR)

The ‘kenya-one-bucket’ is created successfully.

[![image9.png](https://i.postimg.cc/brfTCWT7/image9.png)](https://postimg.cc/KkQB49Cf)

Click on the newly created bucket to configure the encryption settings  Click on the bucket name ‘kenya-one-bucket’ to navigate the properties of the bucket. In the properties tab, ensure bucket versioning is enabled. Next, under default encryption, we select Server-side encryption with AWS Key Management Service keys (SSE-KMS).  

Retain the default setting (Enable) for Bucket Key and click on Save Changes.

[![image21.png](https://i.postimg.cc/NG87GNb0/image21.png)](https://postimg.cc/G92sqxW0)

Success notification

[![image12.png](https://i.postimg.cc/kX9sxrD8/image12.png)](https://postimg.cc/qgb2T5sM)

Scroll down and click on Edit under Server Access Logging. Enable Server access logging by selecting Enable, entering s3:// kenya-one-bucket or you can click on “Browse S3” to add the Target bucket, and click on Save changes.

[![image4.png](https://i.postimg.cc/JzrTWwLm/image4.png)](https://postimg.cc/ZCQPrMnD)

Success notification. We have configured the ‘kenya-one-bucket’  to encrypt data. The user can now add data

[![image5.png](https://i.postimg.cc/J4W6PDYs/image5.png)](https://postimg.cc/4nBQ3dgG)

Also, the ARN chosen in the Default Encryption section is “kenya-one-key”.

KMS to Amazon EBS
=================

Amazon EBS supports KMS. Its encryption provides data-at-rest security by encrypting data volumes, boot volumes, and snapshots using Amazon-managed keys or keys created and managed using AWS KMS.

To encrypt EBS volumes with the KMS master key, click on Services from the menu bar and search for EC2. From the search results, click on EC2 virtual servers in the cloud.

Once the EC2 Dashboard page opens, click on Volumes in the left pane under ELASTIC BLOCK STORE and click on Create Volume.

[![image22.png](https://i.postimg.cc/ncHKXvwG/image22.png)](https://postimg.cc/7bWJcT45)

In the KMS Key field, select the kenya-one-key that was created: and click Create Volume

[![image11.png](https://i.postimg.cc/v85Xg88b/image11.png)](https://postimg.cc/145DbQNj)

**Mini Task**

Now, encrypt Amazon Redshift using the same KMS master key as you encrypted the EBS volume. By following the above steps, you can now as a cloud security engineer implement AWS KMS. Search for redshift in the services.

Create a workgroup (here, I named it ‘kenya-one-wg’)

[![image14.png](https://i.postimg.cc/Sx1GFJd7/image14.png)](https://postimg.cc/mPM97Z0t)

Click next and create a namespace(here , I named it ‘kenya-one-ns’)

[![image17.png](https://i.postimg.cc/1zLcX2L8/image17.png)](https://postimg.cc/xk3bFxjQ)

Under the namespace creation page, head over to encryption and select add encryption and security.  Paste in the ARN for our kenya-one-key created earlier .

[![image18.png](https://i.postimg.cc/bJk0tLh3/image18.png)](https://postimg.cc/sQfZFY1h)

Click next, Review, and create.

[![image2.png](https://i.postimg.cc/VstZqBgy/image2.png)](https://postimg.cc/grYKdh14)

Conclusion
=========

Implementing **AWS Key Management Service (KMS)** enhances data security in the cloud by providing comprehensive key management capabilities, including creation, rotation, and access control, integrated seamlessly with other AWS services. It simplifies compliance with regulatory requirements through detailed logging and auditing, supports both symmetric and asymmetric keys for diverse use cases, and scales accordingly to meet growing organisational needs. By adopting AWS KMS, organizations can effectively protect sensitive data, streamline key management processes, reduce operational overhead, and ensure adherence to industry standards and best practices.