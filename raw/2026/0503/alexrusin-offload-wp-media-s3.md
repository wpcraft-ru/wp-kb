How to Offload WordPress Media to AWS S3 | Alex Rusin Blog
[Skip to content](#main)
[Alex Rusin Blog](https://blog.alexrusin.com/)
* Blog
+ [AWS](https://blog.alexrusin.com/category/aws/)
+ [Javascript Development](https://blog.alexrusin.com/category/javascript/)
+ [Remix](https://blog.alexrusin.com/category/remix/)
+ [PHP MySQL Development](https://blog.alexrusin.com/category/php-mysql-development/)
+ [Laravel](https://blog.alexrusin.com/category/laravel/)
+ [System Administration](https://blog.alexrusin.com/category/system-administration/)
+ [WordPress Development](https://blog.alexrusin.com/category/wordpress-development/)
+ [Magento](https://blog.alexrusin.com/category/magento/)
+ [Uncategorized](https://blog.alexrusin.com/category/uncategorized/)
* [My Projects](https://blog.alexrusin.com/category/my-projects/)
* Search for:Search Button
[Alex Rusin Blog](https://blog.alexrusin.com/)
[AWS](https://blog.alexrusin.com/category/aws/) | [WordPress Development](https://blog.alexrusin.com/category/wordpress-development/)
# How to Offload WordPress Media to AWS S3
By[alexrusin](https://blog.alexrusin.com/author/alexrusin/)
September 11, 2024September 14, 2024
Managing media files can become a challenge as your [WordPress](https://wordpress.com/) website grows. Large media libraries can slow down your website, increase storage costs, and impact performance. One effective solution is to offload your media files to [Amazon S3](https://aws.amazon.com/s3/), a reliable and scalable cloud storage service. In this guide, we will walk you through the steps of using the WP Offload Media Lite plugin to offload your media files from your WordPress website to Amazon S3.
## Why Offload Media to Amazon S3?
Offloading media files to Amazon S3 offers several benefits:
* **Improved Website Performance**: Offloading media reduces the load on your web server, improving site speed and performance.
* **Scalability**: Amazon S3 provides scalable storage, so you never have to worry about running out of space as your website grows.
* **Enhanced Reliability**: S3’s infrastructure is highly durable and reliable, ensuring your media files are always available.
* **Cost Efficiency**: S3 allows you to pay only for the storage you use, making it a cost-effective solution for managing large media libraries.
## Prerequisites
Before we begin, make sure you have the following:
* A WordPress website up and running.
* An Amazon Web Services (AWS) account.
* S3 bucket created in your AWS account for storing media files.
## Step-by-Step Guide to Offload WordPress Media to S3 with WP Offload Media Lite
### Install WP Offload Media Lite Plugin
* Log in to your WordPress admin dashboard.
* Navigate to **Plugins > Add New**.
* Search for “WP Offload Media Lite.”
* Click **Install Now**, then **Activate**.
### Create an S3 Bucket
* Log in to your AWS Management Console.
* Navigate to **Services > S3**.
* Click on **Create bucket**.
* Enter a unique bucket name and select the appropriate region. (It’s best to choose a region closest to your server location for lower latency.)
* Leave the default settings for the rest of the options and click **Create bucket**.
### Update S3 Bucket Permissions
* Click on the bucket you just created.
* Select **Permissions** tab.
* Edit **Block public access (bucket settings)** to turn off **Block *all* public access**.
* Edit **Object Ownership** and select **ACLs enabled**. Acknowledge that ACLs will be restored and save changes.
### Set Up AWS IAM Role and Permissions
If you are hosting your WordPress site on AWS, please read along to learn how to create IAM Role and attach it an EC2 instance (or other resource) where your WordPress site is deployed. Otherwise, follow the instructions on how to set up IAM User and permissions in [this article](https://blog.alexrusin.com/how-to-backup-wordpress-website-to-aws-s3-using-updraftplus/).
#### Create Customer Managed Policy
* Go to **Services > IAM**.
* Click on **Policies** and **Create New**
* Select **JSON** view and paste the following:
```
{
"Version": "2012-10-17",
"Statement": [
{
"Effect": "Allow",
"Action": [
"s3:ListBucket",
"s3:ListAllMyBuckets",
"s3:GetBucketLocation"
],
"Resource": [
"*"
]
},
{
"Effect": "Allow",
"Action": [
"s3:*"
],
"Resource": [
"arn:aws:s3:::my-blog.alexrusin.com/*"
]
}
]
}
```
This is policy allows full access permissions to a specific S3 bucket.
* Give you policy a **name** and optionally a **description**.
* Click **Create policy.**
### Create a Role
* Click on **Roles** and then **Create role**.
* Select **AWS service** as ****Trusted entity type****.
* In **Use case** card select **EC2** from the dropdown and click **Next**.
* Select **Customer managed** policy from the dropdown and attach the created above policy to the role. Click **Next**.
* Give your role a name. Scroll down and click **Create role**.
### Attach Role to EC2
* Go to **Services > EC2**.
* Select EC2 instance you are running WordPress site on.
* In **Actions** > **Security** select **Modify IAM role**.
* Select the role you just created from the dropdown and click **Update IAM role.**
### Set Up WP Offload Media Lite
* Go to **WP Offload Media > Settings** in your WordPress dashboard.
* Under the **Storage Provider** section, select **Amazon S3**.
* Enter the **Access Key ID** and **Secret Access Key** you obtained from AWS if you created a user in the steps above. If you host your WordPress on AWS and create a role, select to use **IAM Roles** option.
* Choose the S3 bucket you created for your media files.
### Configure Plugin Settings
* Choose whether to keep the media files locally on your server after offloading to S3. It’s recommended to remove them from your server to save space.
* Configure URL settings. You can use the default Amazon S3 URL or set up a custom domain or CDN for serving media files.
* Save your settings.
### Test Your Setup
* Go to WordPress Media library.
* Click on **Add New Media File** and upload a file.
* After the file is uploaded, click on the file and verify **File URL** field has S3 bucket as the URL of the file.
## Best Practices
* **Regular Backups**: Always keep a backup of your WordPress website, including media files, before making significant changes.
* **Use a CDN**: For even better performance, consider using a Content Delivery Network (CDN) in conjunction with Amazon S3 to deliver media files faster to global visitors.
* **Monitor Costs**: Keep an eye on your S3 storage and data transfer costs, especially if your website has a high volume of media content.
## Conclusion
Offloading your WordPress media to Amazon S3 using WP Offload Media Lite is an effective way to optimize your website’s performance, save server space, and enhance reliability. With the steps outlined in this guide, you can easily set up and manage your media offloading to S3, allowing your website to scale smoothly as your content library grows.
By leveraging AWS S3, you can ensure that your media files are stored securely and served efficiently, improving the overall user experience on your WordPress site.
Share this article
Post Tags:
[#aws](https://blog.alexrusin.com/tag/aws/)
## Post navigation
[Previous
How to Backup WordPress Website to AWS S3 Using UpdraftPlus](https://blog.alexrusin.com/how-to-backup-wordpress-website-to-aws-s3-using-updraftplus/)
[Next
IAM Policies and Permissions in AWS](https://blog.alexrusin.com/iam-policies-and-permissions-in-aws/)
## Similar Posts
* [WordPress Development](https://blog.alexrusin.com/category/wordpress-development/)
### [Adding MailChimp Popup Signup Form to Your WordPress Website](https://blog.alexrusin.com/adding-mailchimp-popup-signup-form-to-your-wordpress-website/)
By[alexrusin](https://blog.alexrusin.com/author/alexrusin/)
June 27, 2016July 2, 2016
MailChimp email marketing service now gives you an option of creating popup signup form that can be embedded on any website. When all set and done, MailChimp provides you with the following code: You can just copy and paste this code into header.php or better yet in footer.php because you want the signup form to popup when the…
Share this article
[Read More Adding MailChimp Popup Signup Form to Your WordPress Website](https://blog.alexrusin.com/adding-mailchimp-popup-signup-form-to-your-wordpress-website/)
* [AWS](https://blog.alexrusin.com/category/aws/)
### [Instance Connect: Connect to EC2 without SSH Keys](https://blog.alexrusin.com/instance-connect-connect-to-ec2-without-ssh/)
By[alexrusin](https://blog.alexrusin.com/author/alexrusin/)
August 21, 2024August 23, 2024
Learn how to securely and efficiently connect to your Amazon EC2 instances using EC2 Instance Connect. This guide covers the benefits of avoiding manual SSH key management, the requirements for setup, and a step-by-step demo on configuring your EC2 instance and security groups. Ideal for anyone looking to streamline their cloud operations with AWS.
Share this article
[Read More Instance Connect: Connect to EC2 without SSH Keys](https://blog.alexrusin.com/instance-connect-connect-to-ec2-without-ssh/)
* [AWS](https://blog.alexrusin.com/category/aws/)
### [Understanding AWS Security Groups](https://blog.alexrusin.com/understanding-aws-security-groups/)
By[alexrusin](https://blog.alexrusin.com/author/alexrusin/)
August 15, 2024August 28, 2024
AWS security groups are essential virtual firewalls that manage the flow of inbound and outbound traffic for cloud resources like EC2 instances and databases. This guide delves into how security groups work, key protocols, common ports, and practical examples, ensuring you understand their critical role in maintaining a secure AWS environment.
Share this article
[Read More Understanding AWS Security Groups](https://blog.alexrusin.com/understanding-aws-security-groups/)
* [Javascript Development](https://blog.alexrusin.com/category/javascript/)
### [API Gateway Webhooks Lambda HMAC Validation](https://blog.alexrusin.com/api-gateway-webhooks-hmac-validation/)
By[alexrusin](https://blog.alexrusin.com/author/alexrusin/)
October 15, 2023February 16, 2025
In the previous post HMAC Validation we explored how to validate HMAC in an API project. The video Webhooks Processing: HTTP API Gateway + SQS +Lambda shows how to created a scalable solution for receiving webhooks and throttling them with SQS and Lambda. In this article we will look at how to use HMAC Validation…
Share this article
[Read More API Gateway Webhooks Lambda HMAC Validation](https://blog.alexrusin.com/api-gateway-webhooks-hmac-validation/)
* [AWS](https://blog.alexrusin.com/category/aws/)
### [Getting Started with Amazon CloudWatch Logs: Centralized Monitoring Made Easy](https://blog.alexrusin.com/getting-started-with-amazon-cloudwatch-logs-centralized-monitoring-made-easy/)
By[alexrusin](https://blog.alexrusin.com/author/alexrusin/)
September 24, 2024September 30, 2024
Amazon CloudWatch Logs is a powerful tool for monitoring and managing logs from your AWS resources in real time. In this guide, we explore the essential features of CloudWatch Logs, including log groups, streams, metric filters, and more.
Share this article
[Read More Getting Started with Amazon CloudWatch Logs: Centralized Monitoring Made Easy](https://blog.alexrusin.com/getting-started-with-amazon-cloudwatch-logs-centralized-monitoring-made-easy/)
* [AWS](https://blog.alexrusin.com/category/aws/) | [WordPress Development](https://blog.alexrusin.com/category/wordpress-development/)
### [How to Backup WordPress Website to AWS S3 Using UpdraftPlus](https://blog.alexrusin.com/how-to-backup-wordpress-website-to-aws-s3-using-updraftplus/)
By[alexrusin](https://blog.alexrusin.com/author/alexrusin/)
September 10, 2024September 14, 2024
Backing up your WordPress website is essential for safeguarding your data against unexpected issues. In this guide, we’ll walk you through the process of using UpdraftPlus to back up your WordPress site directly to AWS S3. You’ll learn how to set up UpdraftPlus, configure it to connect with S3, and schedule regular backups to keep your website secure.
Share this article
[Read More How to Backup WordPress Website to AWS S3 Using UpdraftPlus](https://blog.alexrusin.com/how-to-backup-wordpress-website-to-aws-s3-using-updraftplus/)
© 2026 Alex Rusin Blog - WordPress Theme by [Kadence WP](https://www.kadencewp.com/)
* BlogToggle child menu
+ [AWS](https://blog.alexrusin.com/category/aws/)
+ [Javascript Development](https://blog.alexrusin.com/category/javascript/)
+ [Remix](https://blog.alexrusin.com/category/remix/)
+ [PHP MySQL Development](https://blog.alexrusin.com/category/php-mysql-development/)
+ [Laravel](https://blog.alexrusin.com/category/laravel/)
+ [System Administration](https://blog.alexrusin.com/category/system-administration/)
+ [WordPress Development](https://blog.alexrusin.com/category/wordpress-development/)
+ [Magento](https://blog.alexrusin.com/category/magento/)
+ [Uncategorized](https://blog.alexrusin.com/category/uncategorized/)
* [My Projects](https://blog.alexrusin.com/category/my-projects/)
* Search for:Search Button
