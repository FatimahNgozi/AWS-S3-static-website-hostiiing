# AWS-S3-static-website-hosting

This repository documents my learning with static website hosting on Amazon S3.
I have my step-by-step guide on exactly how i did it
Technologies used includes: Amazon S3, HTML, CSS & JavaScript

## Getting Started
After all the necessary contents such as text, images and other media content has been gathered, we move straight to S3 bucket setup.
Amazon S3 Bucket Setup:
A new Amazon S3 bucket is created and configured specifically for hosting your static website(in my case MercyReads)
![image](https://github.com/user-attachments/assets/ab3a4ac2-1149-4705-b0d7-13fbd5831c5f)
Choose Properties and scroll down to the option Static website hosting and click on Edit button to enable the feature.
![image](https://github.com/user-attachments/assets/8904e5e2-592e-47ec-905a-1dc1bd8a1703)
![image](https://github.com/user-attachments/assets/4b020080-d94e-463c-ab1b-79af3df5778f)
![image](https://github.com/user-attachments/assets/3e5299fc-9ed6-48d4-9aea-c295a5733dd8)
![image](https://github.com/user-attachments/assets/419ff133-1e01-4b3c-bc1e-3a72a6987804)
In the index document part, put your index.html in there and put 404.html in the error document
Save changes to get a success edit message
Upon scrolling down to the Static website hosting section you will find the link of your bucket, click on it to open the link
You will find an error message
When you create any bucket, by default it is not publicly accessible. To actually make the bucket accessible, scroll back up, go to the Block public access (bucket settings), click on the edit button to untick/disable(off) the Block all public access button and save changes and enter confirm in the field provided to confirm the changes.
You will get a success message
On clicking on the bucket link again, you will get an error message still, so here is what to do next
The good part of S3 is that it gives multiple level of blocking to ensure your bucket is safe at bucket level and account level.
Now let's edit the bucket policy so that the bucket is accessible. Under Permissions, scroll down to the Bucket policy and click on the Edit button
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::Bucket-Name/*"
      ]
    }
  ]
}
Copy and paste the above json file in the section. Be sure to change the Bucket-Name with your own bucket name and save changes.
Now you will find that your S3 bucket is Publicly accessible
Now Click on Properties, scroll down then click on the S3 bucket link that is in Static website hosting section. It should display a 404 not found error and that is because the index.html file hasn't been uploaded (i.e no object yet in the bucket).
Now, add files to the S3 bucket created and also make and error file so that in case the user could not access the files, it would get a better error message instead of 404 not found.
Open the terminal and create a 404.html file
You will find that there is a 404.html created. Now copy this 404.html file to your S3 bucket using the command given below (aws s3 cp 404.html s3://bucket-name)
On refreshing the browser you will find the customized message instead of 404 Not found
In a better way to understand, this link displays error page because there is no other files available in the bucket for the user to access.
Now we will have to sync other files. You can use the (aws s3 sync . s3://bucket-name)
Now refresh your bucket on AWS management console and you will find other files there.
Now refresh your browser and you will see the Static website is ready
Consider Monitoring and Maintenance
Implementing monitoring tools is essential for tracking website performance, availability and identifying potential errors.
AWS provides various services and integrations that can help you achieve comprehensive monitoring. Amazon CloudFront and AWS WAF Logs can be of good help.
If you use Amazon CloudFront with AWS WAF, monitor CloudFront access logs and WAF logs to identify potential security threats and track the performance of your CDN.
Configure CloudFront by navigating to cloudFront from the AWS console and click on create distribution.
Under Origin Options, Origin domain should be given, therefore you should copy and paste the link of your S3 bucket from the browser to the Origin domain. So any requests that happens will be directed to the link in the browser
Click on Next and enable security protection at the Web Application Firewall (WAF)
Create Cache Policy for your distribution by clicking on create cache policy radio button.
Then create the distribution
Static Website Hosting is live!
