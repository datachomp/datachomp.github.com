---
title: Upload file to S3 with ASP.NET
author: Rob
layout: post
permalink: /archives/upload-file-to-s3-with-asp-net/
dsq_thread_id:
  - 888404907
categories:
  - AppDev
  - ASP.NET
---
# 

I suppose the subject pulls no punches with what we're going to do. We are going to have simple form with a file upload and a button. Once we hit the button, whatever file was selected is going to ride the magic internet carpet and appear in an Amazon S3 bucket.  
WebForm:

    1.  &nbsp;
    
    2.  
    
    3.  

Code behind:

    1.  &nbsp;
    
    2.  protected void Button1_Click&#40;object sender, EventArgs e&#41;
    
    3.  		&#123;
    
    4.  			string result = Program.GetServiceOutput&#40;&#41;;
    
    5.  			Label1.Text = result;
    
    6.  		&#125;
    
    7.  &nbsp;

Class file for AWSCalls;

    1.  &nbsp;
    
    2.  using Amazon;   // these are part of the AWSSDK.dll that you add to the project
    
    3.  using Amazon.S3;
    
    4.  using Amazon.S3.Model;
    
    5.  ....
    
    6.  public static void SendFileToS3&#40;string filename, Stream ImgStream&#41;
    
    7.  		&#123;
    
    8.  			string accessKey = "Access Key here!";
    
    9.  			string secretAccessKey = "Secret Key goes here!";
    
    10. 			string bucketName = "Bucket Name goes Here!";
    
    11. 			string keyName = filename; 
    
    12. &nbsp;
    
    13. 			AmazonS3 client = Amazon.AWSClientFactory.CreateAmazonS3Client&#40;accessKey, secretAccessKey&#41;;
    
    14. 			PutObjectRequest request = [new][1] PutObjectRequest&#40;&#41;;
    
    15. &nbsp;
    
    16. 			request.WithInputStream&#40;ImgStream&#41;;
    
    17. &nbsp;
    
    18. 			request.WithBucketName&#40;bucketName&#41;;
    
    19. 			request.WithKey&#40;keyName&#41;;
    
    20. 			request.StorageClass = S3StorageClass.ReducedRedundancy; //set storage to reduced redundancy
    
    21. 			client.PutObject&#40;request&#41;;
    
    22. 		&#125;

 [1]: http://www.google.com/search?q=new msdn.microsoft.com

So there you have it! We take a file, throw it into a page.... and that page does a heave-ho to Amazon S3. It also goes the extra step to set it to ReducedRedundancy to save you a pretty penny.