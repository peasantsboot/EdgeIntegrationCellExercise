# Exercise 2 - Design and run an API on Edge Integration Cell

In the concluding section of the tutorial, we will be focusing on the creation of an API, which serves as a consolidated entity enabling developers to incorporate both policies and steps. For this exercise, we will generate an API that directs to the same SAP S/4HANA REST endpoint URL deployed in the prior exercise. Additionally, we will enhance its functionality by integrating traffic management features like Surge Protection and mediation capabilities such as the XML to JSON converter.

### Note

For all the subsequent steps in this exercise, please replace the **XX** in **userXX** with your respective id, e.g. user99.

### Configuration & Deployment

1. Navigate to the sub-navigation item **Integrations and APIs** under the **Design** tab on the left side pane and click on **Create** to create a package

![](/exercises/ex4/images/04_01_0010.png)

2. Create a package with name and short text **userXX** as shown in the image below and click on **Save**. Replace **XX** with the id assigned to you.

![](/exercises/ex4/images/00-02-SavePackage.png)

3. Switch to the **Artifacts** tab and add a new API by selecting **API** from the menu.

![](/exercises/ex4/images/04_03_0010.png)

4 Choose **Edge Integration Cell** as Runtime Profile and press **Next**.

![](/exercises/ex4/images/04_04_0009.png)

5. For the purpose of this exercise, we would be selecting the **URL** option.

![](/exercises/ex4/images/04_04_0010.png)

6. Fill in the details for the API as follows, then select **Create**.

Name: **API Artifact userXX** with **XX** the id assigned to you

URL:
```yaml 
https://proxyavrdev.hana.ondemand.com/Proxy/jenkslave55.cpi.c.eu-de-1.cloud.sap/9912/sap/bc/srt/scs_ext/sap/salesorderbulkrequest_in
```

API Base Path: **/apiuserXX** with **XX** the id assigned to you

API State (select from dropdown): **Active**

API Version: **1.0.0**

Runtime Profile (select from dropdown): **Edge Integration Cell**

![](/exercises/ex4/images/04_05_0010.png)

7. Post the successful creation of the API, navigate into the **Overview** tab to confirm if the details are correct.

![](/exercises/ex4/images/04_06_0010.png)

8. Click on **Edit** and navigate to the **Policies** tab and this is how the starter content should look like. As you can see the **Authentication** policy appears by default. 

![](/exercises/ex4/images/04_07_01_0010.png)

9. Double click on the **Authentication** policy and navigate to the property sheet at the bottom. By default **Basic** is not enabled and for this exercise, you should enable the **Basic** checkbox from the multi-select drop-down.

![](/exercises/ex4/images/04_07_02_0010.png)

10. Click on the **Authentication** policy and you should see a pop-up with a set of actions. Click on the **+ (plus)** icon to add a policy right after the **Authentication** policy. Select the **Surge Protection** policy from the drop-down to add the policy to your API.

![](/exercises/ex4/images/04_08_0010.png)

11. The policy protects the target endpoint from a sudden spike in incoming requests. Navigate to the highlighted section below in the property sheet of the policy and fill in the details as shown in the UI. We are effectively configuring the policy to allow only **5** calls in a span of **10** seconds.

![](/exercises/ex4/images/04_09_0010.png)

12. Next, we need to convert the response of the API call from XML to JSON. Click on the **Request Reply** and when the pop-up opens, click on the **+ (plus)** icon to add a new flow step right after the **Request Reply** flow step. Select **XML to JSON Converter** from the drop-down.

![](/exercises/ex4/images/04_10_01_0010.png)

13. Configure the step in accordance with the highlighted section shown in the UI.

JSON Prefix Separator: **Colon(:)**

JSON Output Encoding: **UTF-8**

Suppress JSON Root Element flag: **selected**

![](/exercises/ex4/images/04_10_02_0010.png)

14. Next, we need to define the content type of the response. Click on the **XML to JSON Converter** step and when the pop-up opens, click on the **+ (plus)** icon to add a new flow step right after the **XML to JSON Converter** flow step. Select **Content Modifier** from the drop-down.

![](/exercises/ex4/images/04_10_03_0010.png)

14. Double click on the **Content Modifier** step and navigate to the **Message Header** tab at the bottom. Add a new header **content-type** of source type **Constant** and source value **application/json**. Once done click on **Save**.

![](/exercises/ex4/images/04_10_04_0010.png)

15. Now we are ready to deploy the API on Edge. Click on "..." icon on the top right corner and from the dropdown select **Deploy**. During creation we had already configured the runtime profile and the hence this would deploy the API to the configured edge runtime profile.

![](/exercises/ex4/images/04_11_0010.png)

16. Post a successful deployment, the status field for the API should change from **Not deployed** to the **Deployed** state. In the UI, the highlighted section shows that the API is successfully deployed and is ready for execution.

![](/exercises/ex4/images/04_12_0010.png)

17. Navigate out of the edit view by selecting **Cancel** from the dropdown as shown.

![](/exercises/ex4/images/04_13_0010.png)

18. Now we would navigate to the monitoring shell navigation item on the left and select **Integrations and APIs**. Post the selection, we would select the edge runtime profile from the dropdown.

![](/exercises/ex4/images/04_14_0010.png)

19. Under the **Manage Integration Content** section, click on the highlighted tile to navigate to the list view of all the deployed artifacts.

![](/exercises/ex4/images/04_15_0010.png)

20. Select the artifact **API Artifact userXX** and you should see the endpoint which would be used to access the API. Click on the highlighted icon and copy the deployed url to the clipboard.

<br>![](/exercises/ex4/images/04_16_0010.png)

## Testing

Now that we have successfully deployed the API, it is time to test the API.

**Note**: You can use the Bruno API client application for which we have provided a collection with pre-configured sample requests. As a prerequisite, you should have gone through [Setup Bruno API client](../prep/). If not, do the setup, then come back and proceed with the steps below. Otherwise, if using a different API test tool, you need to create the request from scratch.

1. Open the Bruno application on your laptop, expand the **Edge Integration Cell Exercises** collection and select the GET request **Exercise 2 - Request Sales Orders**. Paste the copied end point from the clipboard into the URL field or simply replace the **XX** in the URL with the id provided to you. Ensure that the **eu03** environment has been selected. Then trigger a message by selecting the **Send Request** button on the upper right.

Note: if you like to create the request on your own, simply create a new GET request without any body, paste the copied end point from the clipboard into the URL field and maintain the credentials as follows:

User name:
```yaml 
sb-88e72008-6ed5-4ef9-9532-34dd3c81a538!b44358|it-rt-cpisuite-europe-03!b18631
```

Password:
```yaml 
49678bdf-17e9-49e3-acfb-566fd1ffe207$fQyWDkCysu-zx38Ho0qZeuCm8RXp5elSp0tp8Vlko5E=
```

![](/exercises/ex4/images/04_19_0010.png)

2. Post a successful request, we would try to simulate the scenario, where we violate the criteria for surge protection policy. Execute the request by clicking on the **Send** button repeatedly such that we end up executing more than 5 requests in a span of 10 seconds. Ideally, on making the 6th request, the surge protection policy should get triggered and you should see a response as shown below. 

![](/exercises/ex4/images/04_20_0010.png)

3. Now, we would navigate back to the monitoring UI to visualize the execution flow. Click on **Monitor Message Processing** link as shown in the UI for the artifact **API Artifact userXX** and this should show all the executions of the API with the latest execution appearing right at the top.

![](/exercises/ex4/images/04_21_0010.png)

4. Select the latest execution as shown below. We can see that a Surge Protection limit exceeded for maximum number of calls.

![](/exercises/ex4/images/04_22_0010.png)


## Summary

You've now completed the following:

1. Modelled an API
2. Deployed it on the Edge Integration Cell
3. Executed the API and violated the surge protection policy successfully which prevents the backend from traffic surges
4. Monitored the execution flow of the API 

Navigate back to - [Main page](/README.md)
