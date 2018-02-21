# Configuring Custom Domains to Handle Open and Click Tracking<a name="configure-custom-open-click-domains"></a>

When you use event publishing to capture open and click events, Amazon SES makes minor changes to the emails you send\. To capture open events, Amazon SES adds a 1 pixel by 1 pixel transparent image to the bottom of each email\. This image has a unique file name for each email, and is hosted on a server operated by Amazon SES\. To capture link click events, Amazon SES replaces the links in your emails with links to a server operated by Amazon SES\. This immediately redirects the recipient to his or her intended destination\. Some Amazon SES customers may want to use their own domains, rather than domains owned and operated by Amazon SES, to create a more cohesive experience for their recipients\. 

You can configure multiple custom domains to handle open and click tracking events\. These custom domains are associated with configuration sets\. When you send an email using a configuration set, if that configuration set is configured to use a custom domain, then the open and click links in that email automatically use the custom domain specified in that configuration set\.

This section contains procedures for setting up a subdomain on a server you own to automatically redirect users to the open and click tracking servers operated by Amazon SES\. There are two steps involved in setting up these domains\. First, you configure the subdomain itself, and then you set up a configuration set to use the custom domain\. This topic contains procedures for completing both of these steps\.

## Part 1: Setting up a Domain for Handling Open and Click Link Redirects<a name="configure-custom-open-click-domain"></a>

The specific procedures for setting up a redirect domain vary depending on your web hosting provider \(and your Content Delivery Network, if you use an HTTPS server\)\. The procedures in the following sections provide general guidance rather than specific steps\.

### Option 1: Configuring an HTTP Domain<a name="configure-custom-open-click-domain-http"></a>

If the subdomain you will use for handling open and click links uses HTTP rather than HTTPS, the process for configuring the subdomain involves only a few steps\.

**Note**  
If you set up a custom domain that uses the HTTP protocol, and you send an email that contains links that use the HTTPS protocol, your customers may see a warning message when they click the links in your email\. If you plan to send emails that contain links that use the HTTPS protocol, you should use an HTTPS domain for handling open and click tracking events\.

If you plan to use an HTTPS subdomain, follow the procedures in [Option 2: Configuring an HTTPS Domain](#configure-custom-open-click-domain-https) instead\.

**To set up an HTTP subdomain for handling open and click links**

1. If you have not already done so, create a subdomain to use for open and click tracking links\. We recommend that you create a subdomain that is dedicated specifically to handling these links\. 

1. Verify the subdomain you created in step 1 for use with Amazon SES\. For more information, see [Verifying Domains in Amazon SES](verify-domains.md)\.

1. Modify the DNS record for the subdomain you created in step 1\. In the DNS record, add a new CNAME record to refer to the appropriate tracking domain for the AWS Region in which you are using Amazon SES\. The following table contains a list of tracking domains for the AWS Regions in which Amazon SES is available\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/configure-custom-open-click-domains.html)
**Note**  
Depending on your web hosting provider, it may take several minutes for the changes you make to the subdomain's DNS record to take effect\. Your web hosting provider or IT organization can provide additional information about these delays\.

### Option 2: Configuring an HTTPS Domain<a name="configure-custom-open-click-domain-https"></a>

If you plan to use an HTTPS subdomain for handling open and click links, you must perform some additional steps, beyond those required for setting up an HTTP domain\.

**To set up an HTTPS subdomain for handling open and click links**

1. Create a subdomain to use for open and click tracking links\. We recommend that you create a subdomain that is dedicated specifically to handling these links\. 

1. Verify the subdomain you created in step 1 for use with Amazon SES\. For more information, see [Verifying Domains in Amazon SES](verify-domains.md)\.

1. Create a new account with a Content Delivery Network \(CDN\), such as [Amazon CloudFront](https://aws.amazon.com/cloudfront)\.

1. Configure the CDN to redirect requests to the appropriate tracking domain for the AWS Region in which you are using Amazon SES\. The following table contains a list of tracking domains for the AWS Regions in which Amazon SES is available\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/configure-custom-open-click-domains.html)

1. If you use Amazon CloudFront as your CDN, complete the following procedures:

   1.  On the **CloudFront Distributations** page, choose the distribution that corresponds with your CDN\.

   1. On the **Behaviors** tab, choose the default behavior, and then choose **Edit**\.

   1. For **Cache Based on Selected Request Headers**, choose **All**\.

   1. For **Query String Forwarding and Caching**, choose **Forward all, cache based on all**\.

   If you use a CDN other than CloudFront, you may need to complete similar steps\. See your CDN's documentation for more information\.

1. Modify the DNS record for the subdomain you created in step 1\. In the DNS record, add a new CNAME record to refer to your address in the CDN you set up in step 3\.
**Note**  
Depending on your web hosting provider, it may take several minutes for the changes you make to the subdomain's DNS record to take effect\. Your web hosting provider or IT organization can provide additional information about these delays\.

1. Acquire an SSL certificate from a trusted Certificate Authority\. The certificate should cover your address at the CDN, as well as the subdomain you created in step 1\. Upload the certificate to the CDN\.

## Part 2: Setting up a Configuration Set to Refer to a Custom Open and Click Tracking Domain<a name="configure-custom-open-click-domain-config-set"></a>

After you configure your domain to handle open and click tracking redirects, you must set up an event destination in a configuration set to refer to your custom domain\. You can complete this step using the Amazon SES console or the `CreateConfigurationSetTrackingOptions` API operation\. This section contains procedures for completing these tasks using the Amazon SES console; for more information about using the API, see [CreateConfigurationSetTrackingOptions](http://docs.aws.amazon.com/ses/latest/APIReference/API_CreateConfigurationSetTrackingOptions.html) in the *[Amazon SES API Reference](http://docs.aws.amazon.com/ses/latest/APIReference/)*\.

**To create a new configuration set event destination that refers to a custom tracking domain**

1. Sign in to the AWS Management Console and open the Amazon SES console at [https://console\.aws\.amazon\.com/ses/](https://console.aws.amazon.com/ses/)\.

1. In the navigation bar on the left side of the screen, choose **Configuration Sets**\.

1. Choose **Create Configuration Set**\.

1. For **Configuration Set Name**, type a name for the configuration set, and then choose **Create Configuration Set**\.

1. In the list of configuration sets, select the box next to the configuration set you created in the previous step\. On the **Actions** menu, choose **Edit**\.

1. On the **Event Destinations** tab, for **Add Destination**, choose an event destination type\. For more information about the options in this menu, see [Add an Event Destination](event-publishing-add-event-destination.md)\.

1. For **Event types**, choose either **Click**, **Open**, or both, depending on the types of events you want to track\.

1. For **Domain**, choose **Use your own subdomain**\.

1. For **Select a verified domain**, choose the domain that you want to use for open and click event tracking\. In the text field to the left of the menu, you can optionally specify a subdomain of the parent domain\. 

1. Configure the remaining options as you normally would\. For more information about setting up event destinations, see [Add an Event Destination](event-publishing-add-event-destination.md)\.

1. Choose **Save**\.