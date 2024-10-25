---
title: "Flaws.cloud Common AWS Vulnerabilities"
date: 2024-09-16 16:16:00 +0300
categories: [aws security, cloud security, vulnerabilities, flaws.cloud]
tags: [aws security, cloud vulnerabilities, encryption, key management, security best practices, flaws.cloud]
image: /assets/img/Posts/Flaws/Flawshero.png
---





# Introduction


In the Flaws AWS series of levels, we will learn about common mistakes and pitfalls when using Amazon Web Services (AWS). By participating in this challenge, we will gain hands-on experience with AWS-specific issues and learn how to avoid them in our own projects. As we progress through the levels, we will confront various security vulnerabilities that commonly occur in AWS environments, where our task is to identify and exploit these vulnerabilities, gaining insights into how they can be mitigated. The goal is to provide us with a deeper understanding of AWS security best practices. Each level presents a scenario with a specific AWS security flaw, and we will learn how to discover and exploit these vulnerabilities in a controlled environment. A series of hints will guide us through each level, teaching us the commands and techniques needed to uncover the information required to solve the challenges.


Objective:
==========

S3 Bucket Permissions: Understand and correct overly permissive S3 bucket policies. Identify an S3 bucket that is publicly accessible and contains sensitive information. Learn how to use AWS Identity and Access Management (IAM) policies to restrict access to S3 buckets.  

AWS CLI Access : Use the AWS Command Line Interface (CLI) to retrieve and find sensitive data. Understand the importance of being aware of the scope of permissions granted, especially when using broad permissions like Any Authenticated AWS User  preventing unauthorized data retrieval.

IAM access keys: Identify an IAM role with excessive permissions and exploit it to access other AWS s3 bucket resources. See the importance of always revoking any AWS keys or any secrets that could have been leaked or were misplaced.

Protect Backups and Snapshots:Implement stringent access controls to secure backups and snapshots of EC2 instances and databases, as they can be exploited by attackers to gain unauthorized access.

Secure Metadata Service Access: Ensure the metadata service endpoint (169.254.169.254) is properly secured to prevent the exposure of sensitive information

Monitor Read-Only Permissions :Identify why it’s proactive to be cautious with read-only permissions, such as the SecurityAudit policy, as they can inadvertently aid attackers in identifying vulnerabilities. Regularly review and manage these permissions to maintain a secure environment.

Level 1 :
=========

The site flaws.cloud is hosted as an S3 bucket. This is a great way to host a static site, similar to hosting one via github pages. to confirm the hosting . You can determine the site is hosted as an S3 bucket by running a DNS lookup on the domain, such as:

$dig +nocmd flaws.cloud any +multiline +noall +answer

[![image32.png](https://i.postimg.cc/gkwpZkkS/image32.png)](https://postimg.cc/GH160RDG)

To know the region of the s3 bucket we do an nslookup on flaws.cloud and sub sequent nslookup on one of the returned IP address as shown below

[![image48.png](https://i.postimg.cc/MTGNJdnP/image48.png)](https://postimg.cc/Ff2Djbg3)

from the displayed information the s3 bucket is located in the “us-west-2” region.  

If we browse the bucket via aws s3 ls  s3://flaws.cloud/ --no-sign-request --region us-west-2

[![image9.png](https://i.postimg.cc/5tP1zHvr/image9.png)](https://postimg.cc/hJ7HWt28)

we can see there is a secret html page

Finally, we can also just visit [http://flaws.cloud.s3.amazonaws.com/](https://www.google.com/url?q=http://flaws.cloud.s3.amazonaws.com/&sa=D&source=editors&ust=1726489833087074&usg=AOvVaw12xYajvmNbmEEZVblCaeIr) which lists the files due to the permissions issues on this bucket. 

[![image23.png](https://i.postimg.cc/PrkGPb9x/image23.png)](https://postimg.cc/hhMZYmSW)

from here we can get the secret and access it from : [http://flaws.cloud.s3.amazonaws.com/secret-dd02c7c.html](https://www.google.com/url?q=http://flaws.cloud.s3.amazonaws.com/secret-dd02c7c.html&sa=D&source=editors&ust=1726489833087758&usg=AOvVaw1fID2GEDOC2s6_8CZfPuoK)

The page takes us to level 2.  

[![image25.png](https://i.postimg.cc/vH7J9gwv/image25.png)](https://postimg.cc/D8Zp3zZW)

**Lesson learned level 1**

On AWS you can set up S3 buckets with all sorts of permissions and functionality including using them to host static files. A number of people accidentally open them up with permissions that are too loose. Just like how you shouldn't allow directory listings of web servers, you shouldn't allow bucket listings.

*   if proprietary data is uploaded to the bucket then it would be exposed to attackers and everyone on the internet.

[![image15.png](https://i.postimg.cc/YCRwc4XS/image15.png)](https://postimg.cc/sMML72CF)

Level 2:  
==========

Similar to the first level, you can discover that this sub-domain is hosted as an S3 bucket with the name "level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud". Its permissions are too loose, but you need your own AWS account to see what's inside. Using my account I run:

aws s3 --profile hourglass ls s3://level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud

[![image4.png](https://i.postimg.cc/TYmRs3My/image4.png)](https://postimg.cc/QH8G9sMs)

from the terminal results we can access the flag page via

[http://level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud/secret-e4443fc.html](https://www.google.com/url?q=http://level2-c8b217a33fcf1f839f6f1f73a00a9ae7.flaws.cloud/secret-e4443fc.html&sa=D&source=editors&ust=1726489833090399&usg=AOvVaw2t3n3kLhExbhwnmG5KK9OI)

[![image40.png](https://i.postimg.cc/PJPgY2hp/image40.png)](https://postimg.cc/SYb1pLnk)

*   alternatively we can download all the files via sync and open the secret html page in a browser :

[![image5.png](https://i.postimg.cc/fW1z50Fn/image5.png)](https://postimg.cc/G9Jnm9dq)

file:///home/cyberchaosjedi/Music/Friday/secret-e4443fc.html

[![image16.png](https://i.postimg.cc/4Nzg52VV/image16.png)](https://postimg.cc/2V8MjwHS)

**Lesson learned level2**

Similar to opening permissions to "Everyone", people accidentally open permissions to "Any Authenticated AWS User". They might mistakenly think this will only be users of their account, when in fact it means anyone that has an AWS account.

[![image1.png](https://i.postimg.cc/HncNVn4b/image1.png)](https://postimg.cc/fSsCPwPb)

Level 3:  
==========

We download the level 3 files via sync :

aws s3 sync s3://level3-9afd3927f195e10225021a578e6f78df.flaws.cloud/ . --no-sign-request --region us-west-2

[![image46.png](https://i.postimg.cc/t4hL3105/image46.png)](https://postimg.cc/Jsz6mzzD)

to view all files including the hidden ones we can use : ls -la

[![image26.png](https://i.postimg.cc/nhH61FHf/image26.png)](https://postimg.cc/F7BWrQdT)

from the list we have a .git file.  

People often accidentally add secret things to git repos, and then try to remove them without revoking or rolling the secrets. You can look through the history of a git repo by running:

git log

[![image34.png](https://i.postimg.cc/90kjns1Y/image34.png)](https://postimg.cc/ctfjK9pv)

*   There are 2 versions of the commit.  

Then you can look at what a git repo looked like at the time of a commit by running:

git checkout f7cebc46b471ca9838a0bdd1074bb498a3f84c87

where \`f7cebc46b471ca9838a0bdd1074bb498a3f84c87\` would be the hash for the commit shown in \`git log\`.

*   git checkout f52ec03b227ea6094b04e43f475fb0126edb5a61  

[![image45.png](https://i.postimg.cc/hP7FGypN/image45.png)](https://postimg.cc/YhkDn3cx)

if we list we can find a sensitive key file that was left

[![image38.png](https://i.postimg.cc/XYd0sH59/image38.png)](https://postimg.cc/VJfT6FDN)

cat access\_key.txt

[![image35.png](https://i.postimg.cc/RCp9hn9J/image35.png)](https://postimg.cc/w3NnbM19)

we can configure a profile flawsaws with the credentials , following is the “who am I “ identity check using aws sts get-caller-identity – profile flawaws

[![image47.png](https://i.postimg.cc/Sx0BY6PR/image47.png)](https://postimg.cc/QHb4RThZ)

Then to list S3 buckets using that profile run:

aws --profile flawsaws s3 ls

[![image42.png](https://i.postimg.cc/bJCXZKXf/image42.png)](https://postimg.cc/phFc7cXs)

We have listed all the buckets available in the profile. the next level 4 : [http://level4-1156739cfb264ced6de514971a4bef68.flaws.cloud/](https://www.google.com/url?q=http://level4-1156739cfb264ced6de514971a4bef68.flaws.cloud/&sa=D&source=editors&ust=1726489833096840&usg=AOvVaw1wTyyDhRtXECtG3YJ5D7Nh)

**Lesson learned level 3**

People often leak AWS keys and then try to cover up their mistakes without revoking the keys. You should always revoke any AWS keys (or any secrets) that could have been leaked or were misplaced. Roll your secrets early and often.

Another interesting issue this level has exhibited, although not that worrisome, is that you can't restrict the ability to list only certain buckets in AWS, so if you want to give an employee the ability to list some buckets in an account, they will be able to list them all. The key you used to discover this bucket can see all the buckets in the account. You can't see what is in the buckets, but you'll know they exist. Similarly, be aware that buckets use a global namespace meaning that bucket names must be unique across all customers, so if you create a bucket named \`merger\_with\_company\_Y\` or something that is supposed to be secret, it's technically possible for someone to discover that bucket exists.

[![image50.png](https://i.postimg.cc/d1SXsrdq/image50.png)](https://postimg.cc/KkBQQKJV)

Level 4:
========

You can snapshot the disk volume of an EC2 as a backup. In this case, the snapshot was made public, but you'll need to find it.

To do this, first we need the account ID, which we can get using the AWS key from the previous level:

aws --profile flaws sts get-caller-identity

the account is 975426262029

The ARN shows the account is /backup  . The backups this account makes are snapshots of EC2s. Next, discover the snapshot: aws --profile flawsaws  ec2 describe-snapshots --owner-id 975426262029

[![image20.png](https://i.postimg.cc/C1W3CbKc/image20.png)](https://postimg.cc/ZBxs4BpN)

By default snapshots are private, and you can transfer them between accounts securely by specifying the account ID of the other account, but a number of people just make them public and forget about them it seems.

Now that you know the snapshot ID, you're going to want to mount it. You'll need to do this in your own AWS account.

First, create a volume using the snapshot:

aws --profile hourglass ec2 create-volume --availability-zone us-west-2a --region us-west-2  --snapshot-id  snap-0b49342abd1bdcb89

[![image28.png](https://i.postimg.cc/HxpHcwVX/image28.png)](https://postimg.cc/FfnwwJbH)

choose the volume you just created. create a new ec2 instance , an rsa pair key and SSH into the instance

to get the ssh link , click the instance and select Connect

[![image30.png](https://i.postimg.cc/Hnng3T0L/image30.png)](https://postimg.cc/Kk6w8SHX)

Grant read access chmod 400

then ssh -i "flaws-access-key.pem" [ec2-user@ec2-54-190-59-246.us-west-2.compute.amazonaws.com](mailto:ec2-user@ec2-54-190-59-246.us-west-2.compute.amazonaws.com)

[![image3.png](https://i.postimg.cc/ryDyS9fN/image3.png)](https://postimg.cc/fkhhsYnV)

We'll need to mount this extra volume by running:

lsblk

[![image44.png](https://i.postimg.cc/XNPS4Scw/image44.png)](https://postimg.cc/SjctfPPs)

sudo file -s /dev/xvdb1

[![image29.png](https://i.postimg.cc/7L1wZsYq/image29.png)](https://postimg.cc/DWztCcXN)

\# Next we mount it sudo mount /dev/xvdb1 /mnt  followed by verification 

[![image17.png](https://i.postimg.cc/7ZwyQ1nc/image17.png)](https://postimg.cc/hfywXdwb)

we navigate  to /mnt ,  home folder , ubuntu and check the content of the nginx configuration file

[![image24.png](https://i.postimg.cc/QN7G8Y6w/image24.png)](https://postimg.cc/vDGSXhwt)

here we get credentials to log into : [http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/](https://www.google.com/url?q=http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/&sa=D&source=editors&ust=1726489833101944&usg=AOvVaw1Una530o1z0OCefNvAgvT1)

[![image11.png](https://i.postimg.cc/bJWh6KmH/image11.png)](https://postimg.cc/N96SFPq5)

**Lesson learned level 4**

AWS allows you to make snapshots of EC2's and databases (RDS). The main purpose for that is to make backups, but people sometimes use snapshots to get access back to their own EC2's when they forget the passwords. This also allows attackers to get access to things. Snapshots are normally restricted to your own account, so a possible attack would be an attacker getting access to an AWS key that allows them to start/stop and do other things with EC2's and then uses that to snapshot an EC2 and spin up an EC2 with that volume in your environment to get access to it. Like all backups, you need to be cautious about protecting them.

Level 5:  
==========

This EC2 has a simple HTTP only proxy on it. Here are some examples of it's usage:

http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/flaws.cloud/

http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/summitroute.com/blog/feed.xml

http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/neverssl.com/

HTTP may be vulnerable to SSRF (Server-Side Request Forgery) . We can try accessing the metadata

via [http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/169.254.169.254/latest/meta-data](https://www.google.com/url?q=http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/169.254.169.254/latest/meta-data&sa=D&source=editors&ust=1726489833104169&usg=AOvVaw0aqZQ0edoKfSICXc7uMWsS)

169.254.169.254/latest/meta-data is a well known way of accessing the meta data of a service.

[![image27.png](https://i.postimg.cc/fRB12GLt/image27.png)](https://postimg.cc/k2RjDhWq)

Then :

[http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/169.254.169.254/latest/meta-data](https://www.google.com/url?q=http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/169.254.169.254/latest/meta-data&sa=D&source=editors&ust=1726489833104979&usg=AOvVaw04C1meET1WltQNyJY9vcHm)/iam

[![image6.png](https://i.postimg.cc/K82Zhhx0/image6.png)](https://postimg.cc/CzcybtZ8)

[http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/169.254.169.254/latest/meta-data](https://www.google.com/url?q=http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/169.254.169.254/latest/meta-data&sa=D&source=editors&ust=1726489833105811&usg=AOvVaw3jJ7ggdWvk71nw52YEUiOU)/iam/security-credentials/

[![image31.png](https://i.postimg.cc/wBY9H8zW/image31.png)](https://postimg.cc/CR73H2zD)

[http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/169.254.169.254/latest/meta-data](https://www.google.com/url?q=http://4d0cf09b9b2d761a7d87be99d17507bce8b86f3b.flaws.cloud/proxy/169.254.169.254/latest/meta-data&sa=D&source=editors&ust=1726489833106439&usg=AOvVaw0oW4Hy_REC573I6tCstjv2)/iam/security-credentials/flaws

[![image18.png](https://i.postimg.cc/BvvfJqyB/image18.png)](https://postimg.cc/pm36ZtPm)

We have obtained access keys credentials used to run the IAM access , we can create a profile with cli access 

[![image19.png](https://i.postimg.cc/5tkd0hWd/image19.png)](https://postimg.cc/qtnZDZ7j)

then nano ~/.aws/credentials and add the session token found with the IAM role credentials.

 We are to figure out how to list the contents of the level6 bucket at level6-cc4c404a8a8b876167f5e70a7d8c9880.flaws.cloud that has a hidden directory in it.

Using the created profile we list the contents

[![image14.png](https://i.postimg.cc/Fstv0Xdh/image14.png)](https://postimg.cc/MXYg84mL)

the directory is ddcc78ff and with a index.html

[http://level6-cc4c404a8a8b876167f5e70a7d8c9880.flaws.cloud/ddcc78ff/index.html](https://www.google.com/url?q=http://level6-cc4c404a8a8b876167f5e70a7d8c9880.flaws.cloud/ddcc78ff/index.html&sa=D&source=editors&ust=1726489833108120&usg=AOvVaw3W9CaTMaR4jafqImt2cU5F)

[![image49.png](https://i.postimg.cc/Pqy97B5v/image49.png)](https://postimg.cc/KKK0kpLm)

**Lesson learned level 5**

The IP address 169.254.169.254 is a magic IP in the cloud world. AWS, Azure, Google, DigitalOcean and others use this to allow cloud resources to find out metadata about themselves. Some, such as Google, have additional constraints on the requests, such as requiring it to use \`Metadata-Flavor: Google\` as an HTTP header and refusing requests with an \`X-Forwarded-For\` header. AWS has recently created a new IMDSv2 that requires special headers, a challenge and response, and other protections, but many AWS accounts may not have enforced it. If you can make any sort of HTTP request from an EC2 to that IP, you'll likely get back information the owner would prefer you not see.

A similar problem to getting access to the IAM profile's access keys is access to the EC2's user-data, which people sometimes use to pass secrets to the EC2 such as API keys or credentials.

[![image8.png](https://i.postimg.cc/kgv7nY5q/image8.png)](https://postimg.cc/dh3b6Bdx)

Level 6:  
==========

We set up a profile named flaws6 . using the provided IAM access key and secret key

[![image41.png](https://i.postimg.cc/Jn4V4R2Q/image41.png)](https://postimg.cc/jW9kMYKW)

The SecurityAudit group can get a high-level overview of the resources in an AWS account, but it's also useful for looking at IAM policies. First, find out who you are (assuming you named your profile "level6"): aws --profile flaws6 iam get-user

We find out the user is named level6

[![image13.png](https://i.postimg.cc/nV9ZshLd/image13.png)](https://postimg.cc/LhSGwRLf)

To find out what policies are attached to it:

aws --profile flaws6 iam list-attached-user-policies --user-name Level6

[![image43.png](https://i.postimg.cc/NFSW4mfN/image43.png)](https://postimg.cc/n9KRVsm7)

there are two policies attached

*   "SecurityAudit": This is an AWS managed policy that provides permissions necessary for security assessments. It is designed to allow users or roles to view (but not modify) a wide range of security-relevant resources and settings in an AWS account.
*   "list\_apigateways" a custom made policy.

Once you know the ARN for the policy you can get it's version id:

aws --profile flaws6 iam get-policy  --policy-arn arn:aws:iam::975426262029:policy/list\_apigateways

The policy id : "PolicyId": "ANPAIRLWTQMGKCSPGTAIO",

[![image7.png](https://i.postimg.cc/W4pTtmDr/image7.png)](https://postimg.cc/75RpW2nY)

Now that we have the ARN and the version id, we can see what the actual policy is:

 aws --profile flaws6 iam get-policy-version  --policy-arn arn:aws:iam::975426262029:policy/list\_apigateways --version-id v4

[![image39.png](https://i.postimg.cc/d09cYh41/image39.png)](https://postimg.cc/Y4jsNCcK)

This tells us using this policy we can call "apigateway:GET" on "arn:aws:apigateway:us-west-2::/restapis/\*"

The API gateway in this case is used to call a lambda function, but we need to figure out how to invoke it.

The SecurityAudit policy lets us see some things about lambdas:

aws --region us-west-2 --profile flaws6 lambda list-functions

[![image12.png](https://i.postimg.cc/WzZPxXKR/image12.png)](https://postimg.cc/Mfz4Qbqt)

That tells you there is a function named "Level6", and the SecurityAudit also lets you run:

aws --region us-west-2 --profile flaws6 lambda get-policy --function-name Level6

[![image37.png](https://i.postimg.cc/xdQVhvGh/image37.png)](https://postimg.cc/yDLGSSby)

This tells us about the ability to execute \`arn:aws:execute-api:us-west-2:975426262029:s33ppypa75/\*/GET/level6\\\` That "s33ppypa75" is a rest-api-id, which we can then use with the other attached policy:

aws --profile flaws6 --region us-west-2 apigateway get-stages --rest-api-id "s33ppypa75"

[![image22.png](https://i.postimg.cc/90ZvLzRC/image22.png)](https://postimg.cc/JH76nr52)

That tells us the stage name is "Prod". Lambda functions are called using the rest-api-id, stage name, region, and resource as [https://s33ppypa75.execute-api.us-west-2.amazonaws.com/Prod/level6](https://www.google.com/url?q=https://s33ppypa75.execute-api.us-west-2.amazonaws.com/Prod/level6&sa=D&source=editors&ust=1726489833114035&usg=AOvVaw1C0hVGvVwaz4o2EFGZeI1h)

[![image36.png](https://i.postimg.cc/sDtr9dMV/image36.png)](https://postimg.cc/QHkwhzgP)

"Go to [http://theend-797237e8ada164bf9f12cebf93b282cf.flaws.cloud/d730aa2b/](https://www.google.com/url?q=http://theend-797237e8ada164bf9f12cebf93b282cf.flaws.cloud/d730aa2b/&sa=D&source=editors&ust=1726489833114766&usg=AOvVaw2JQD_TTxrWveghgNyjH8qH)"

the link navigates to the page :

[![image2.png](https://i.postimg.cc/QC2y5D0w/image2.png)](https://postimg.cc/kRys30rv)
[![image33.png](https://i.postimg.cc/KzJbn1MV/image33.png)](https://postimg.cc/vggCyHXL)

**Lesson learned level 6**

It is common to give people and entities read-only permissions such as the SecurityAudit policy. The ability to read your own and other's IAM policies can really help an attacker figure out what exists in your environment and look for weaknesses and mistakes.

[![image10.png](https://i.postimg.cc/k4BCvnvg/image10.png)](https://postimg.cc/62JF9xPk)

Conclusion
==========

The flAWS.cloud challenge expounds the importance of rigorous security management in AWS environments. There is the necessity of carefully configuring permissions to avoid accidental data exposure, such as ensuring S3 buckets are not overly permissive. Understanding permission scopes is vital to prevent unauthorized access, especially when granting broad permissions that might be misconstrued, such as "Any Authenticated AWS User." Promptly revoking and rotating compromised AWS keys is crucial to maintaining security. Additionally, safeguarding backups and snapshots is essential, as these can be exploited by attackers to gain unauthorized access. Properly securing the metadata service endpoint (169.254.169.254) is also critical, as it can expose sensitive information if not protected by measures like IMDSv2. Finally, even read-only permissions like the SecurityAudit policy can help attackers identify vulnerabilities if not managed carefully. Overall, the challenge highlights the need for diligent permission management, proactive credential handling, and vigilant protection of sensitive data to ensure a secure AWS environment.