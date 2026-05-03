Static content offload - Best Practices for WordPress on AWS
[View a markdown version of this page](static-content-offload.md)
Static content offload - Best Practices for WordPress on AWS
[Documentation](/index.html)[AWS Whitepapers](https://aws.amazon.com/whitepapers/)[AWS Whitepaper](welcome.html)
This whitepaper is for historical reference only. Some content might be outdated and some links might not be available.
# Static content offload
This includes CSS, JavaScript, and image files – either those
that are part of your WordPress themes or those media files
uploaded by the content administrators. All these files can be
stored in Amazon Simple Storage Service (Amazon S3) using a
plugin such as W3 Total Cache and served to users in a scalable
and highly available manner.
[Amazon S3](https://aws.amazon.com/s3/)
offers a highly scalable, reliable, and low-latency data storage
infrastructure at low cost, which is accessible via REST APIs.
Amazon S3 redundantly stores your objects, not only on multiple
devices, but also across multiple facilities in an AWS Region,
thus providing exceptionally high levels of durability.
This has the positive side effect of offloading this workload
from your Lightsail instance and letting it focus on dynamic
content generation. This reduces the load on the server and is
an important step towards creating a stateless architecture (a
prerequisite before implementing automatic scaling).
You can subsequently configure Amazon S3 as an origin for
CloudFront to improve delivery of those static assets to users
around the world. Although WordPress isn’t integrated with
Amazon S3 and CloudFront out-of-the-box, a variety of plugins
add support for these services (for example, W3 Total Cache).
[Document Conventions](/general/latest/gr/docconventions.html)
Accelerating content delivery
Dynamic content
Did this page help you? - Yes
Thanks for letting us know we're doing a good job!
If you've got a moment, please tell us what we did right so we can do more of it.
Did this page help you? - No
Thanks for letting us know this page needs work. We're sorry we let you down.
If you've got a moment, please tell us how we can make the documentation better.
