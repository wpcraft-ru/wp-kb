Amazon S3 bucket creation - Best Practices for WordPress on AWS
[View a markdown version of this page](amazon-s3-bucket-creation.md)
Amazon S3 bucket creation - Best Practices for WordPress on AWS
[Documentation](/index.html)[AWS Whitepapers](https://aws.amazon.com/whitepapers/)[AWS Whitepaper](welcome.html)
This whitepaper is for historical reference only. Some content might be outdated and some links might not be available.
# Amazon S3 bucket creation
1. First, create an Amazon S3 bucket in the AWS Region of your choice. For steps, refer
to [Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html). Enable static website hosting for the bucket by following
the [Tutorial: Configuring a static website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/HowDoIWebsiteConfiguration.html).
2. Create a policy to provide the user created previously access to the specified S3
bucket, and attach the policy to the user. For steps to create the following policy, refer
to [Managing Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage.html).
JSON
```
{
"Version":"2012-10-17",
"Statement": [
{
"Sid": "Stmt1389783689000",
"Effect": "Allow",
"Principal": "*",
"Action": [
"s3:DeleteObject",
"s3:GetObject",
"s3:GetObjectAcl",
"s3:ListBucket",
"s3:PutObject",
"s3:PutObjectAcl"
],
"Resource": [
"arn:aws:s3:::wp-demo",
"arn:aws:s3:::wp-demo/*"
]
}
]
}
```
3. Install and activate the W3TC plugin from the WordPress admin panel.
4. Browse to the General Settings section of the
plugin’s configuration, and ensure that both Browser
Cache and CDN are enabled.
5. From the drop-down list in the CDN configuration, choose Origin Push: Amazon CloudFront (this option has Amazon S3 as its origin).
6. Browse to the Browser Cache section of the plugin’s configuration and enable the
expires, cache
control, and entity tag (ETag) headers.
7. Also activate the Prevent caching of objects after settings
change option so that a new query string is generated and appended to
objects whenever any settings are changed.
8. Browse to the CDN section of the plugin’s configuration and enter the security
credentials of the user you created earlier, as well as the name of the S3 bucket.
9. If you are serving your website via the CloudFront URL, enter the distribution domain name
in the relevant box. Otherwise, enter one or more CNAMEs for your custom domain name(s).
10. Finally, export the media library and upload the wp-includes,
theme files, and custom files to Amazon S3 using the W3TC
plugin. These upload functions are available in the
General section of the
CDN configuration page.
[Document Conventions](/general/latest/gr/docconventions.html)
User creation
Static origin creation
Did this page help you? - Yes
Thanks for letting us know we're doing a good job!
If you've got a moment, please tell us what we did right so we can do more of it.
Did this page help you? - No
Thanks for letting us know this page needs work. We're sorry we let you down.
If you've got a moment, please tell us how we can make the documentation better.
