# Kubernetes Multicontainer Challenge

> In this chapter you will create a multi-container application in Kubernetes. This is more close to real-life but makes administration a little more challenging. In reality you might want to be able to specify that mutliple containers are able to talk to each other in a defined way. You might want to make make sure certain parts of your application run in multiple instances to cover load. You might want to be able to monitor performance of your application. You might want to make sure that your system is self-healing so that faulty components are replaced automatically. For updates you might want to make sure to have zero downtime of your application. We are going to configure all of this in this section.

>## Here's what you'll learn:
>- How to write YAML files to specify a desired state of a Kubernetes object
>- How to use the Azure Portal to view Application performance
>- How to store secrets in Kubernetes
>- How to configure your Kubernetes instance to ensure a certain number of pods is always running
>- How to define rolling upgrades to avoid downtime during an application update
>- How to put all what you've learned into an end to end DevOps pipeline via Azure DevOps


## 1. Kubernetes multi container app deployment 
> Need help? Check hints [here :blue_book:](hints/k8sMulti.md)!
- Get the sample code for a multi container application. (Multi-Calc)
- Build the container images for frontend and backend services locally.
- Push the images to your ACR 
- Apply the container images in your Kubernetes cluster using the *.yml files provided 
    - backend-pod.yml
    - backend-svc.yml
    - frontend-pod.yml
    - frontend-svc.yml
- Configure your application to be accessible from the internet and call the page. Use the calculation.

## 2. Application Insights
> Need help? Check hints [here :blue_book:](hints/applicationinsights.md)!

In this chapter you will create an application insights resource for monitoring your application performance and health status.
- Create application insights in azure
- Configure your apps to inject the application insights key via environment variables
- Use a kubernetes secret to ensure consistency
- Set up an availability test for your endpoint
- Observe and query your application performance during deployments and rolling upgrades

# Fully automated Azure DevOps YAML deployment
In this chapter you will leverage self-healing capabilites of K8s and extend your Azure DevOps pipeline to trigger a deployment to your K8s cluster. Your application will have no downtime during a rolling upgrade.

## 1. Create a yaml
- Create a deployment file to decribe the desired state of your application including replicas of your backend service.
- Modify the deployment file manually so that 
    - the backend service can be found
    - the backend service is available internally only
    - the correct image is being used. 
- Apply the deployment file manually.
> You will find a sample for that in the *hints/yaml* folder (full_deploy.yml). But first, try for yourself ;)

## 2. Fake a failed pod
- Check the number of backend pods. K8s will take care to keep the number of available pods as specified.
- Give it a try and kill some pods. They will be recreated.
```
kubectl get pods
kubectl delete pods/PODNAME
```

## 3. Automate deployment via Azure DevOps
> This is about automatically releasing your app via yaml to you cluster. [here :blue_book:](hints/azuredevops_yaml_kubernetes.md)!
- Check in your yaml file into your code repository
- Make sure that your yaml file is available in the drop
- Make sure to authenticate to your AKS cluster
- Use the kubernetes apply task in your release to deploy your app continously
- Activate the build and release trigger to deploy on every code change
> You can use the full_deploy.yml *hints/yaml* folder. But again, try for yourself ;)

# Bonus Challenge - Technology Shootout
Let's say a co-worker of you recommends writing the backend app with in "Go" for performance reasons. How could you try the Go-Backend and run it without downtime? Where could you find performance data? 
Implement the solution and upgrade your application to the Go-backend without downtime. (The Go backend app can be found in /apps/go-calc-backend .)
- Build the Go backend image 
- Publish the image in your registry
- Modify your backend-service Yaml to target the new image
- Deploy
- Check monitoring data for performance impact
