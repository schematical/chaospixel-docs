# ChaosPixel - Stable Diffusion As A Service:
[![](./thumb.png)](https://youtu.be/L-9wMDkQkxI)
Watch the video on YouTube for more information: https://youtu.be/L-9wMDkQkxI

## GQL Getting Started:
### Step 0: Signup:
In reality there is not automated signup process. Ping @schematical on Discord here: https://discord.gg/zUEacFT

### Step 1 - Create your Training Model:
A `TrainingModel` is what the final model you will be asking to do inference(In the case of Dreambooth generate pictures). It will train/fine tune on your custom training data and then be ready to create new images based on your prompts.
#### GQL:
```
mutation createTrainingModel($input: TrainingModelCreateInput!) {
  createTrainingModel(input: $input) {
    _id
    namespace
    name
    user {
      _id
      username
      __typename
    }
    __typename
  }
}

```

#### Variables:
```
{
  "input": {
    "name": "{your model name}",
    "namespace": "{your model namespace}"
  }
}
```
Replace `{your model name}` with a real model name and replace `{your model namespace}` with a real namespace. The namespace should have nothing but letters and numbers, no spaces or punctuation.

#### Response:
The most important thing from the response will be the TrainingModel's `_id` you will need to reference it later.
```
{
    _id: '650dad807c0c021e5b092ab7'
}
```


### Step 2 - Create your TrainingInstance:
A `TrainingInstance` is a collection of training data. In the case of Dreambooth and stable diffusion they are photographs.
#### GQL:
```
mutation createTrainingInstance($input: TrainingInstanceCreateInput!) {
  createTrainingInstance(input: $input) {
    _id
    namespace
    name
    user {
      _id
      username
      __typename
    }
    __typename
  }
}
```

#### Variables:
```
{
  "input": {
    "name": "youtubetest2",
    "namespace": "youtubetest2"
  }
}
```
Replace `{your model name}` with a real model name and replace `{your model namespace}` with a real namespace. The namespace should have nothing but letters and numbers, no spaces or punctuation.
#### Response:
The most important thing from the response will be the TrainingModel's `_id` you will need to reference it later.
```
{
    _id: '650dade3a16828c9ac45af33'
}
```



### Step 3: Get the TrainingUpload URLs:
You will then need to upload data to that TrainingInstance you created

#### GQL:
```
mutation getTrainingInstanceUploadUrls($input: GetTrainingInstanceFileUploadUrlInput!) {
  getTrainingInstanceUploadUrls(input: $input) {
    uploadUrls
    __typename
  }
}
```

#### Variables:

```
{
  "input": {
    "trainingInstanceId": "650dade3a16828c9ac45af33",
    "uploads": [
      {
        "extension": "jpg"
      },
      {
        "extension": "jpg"
      },
      {
        "extension": "jpg"
      },
      {
        "extension": "jpg"
      },
      {
        "extension": "jpg"
      },
      {
        "extension": "jpg"
      },
      {
        "extension": "jpg"
      },
      {
        "extension": "jpg"
      }
    ]
  }
}
```

The `trainingInstanceId` should be the `_id` you got back from step 2. Then for every photo you wish to upload pass in one entry in the `upload` param with the photo's extension(Ex: png, jpg).
#### Response:
```
{
  "getTrainingInstanceUploadUrls": {
    "uploadUrls": [
      "https://chaospixel-worker-v1-dev-us-east-1.s3.us-east-1.amazonaws.com/schematical/instances/650dade3a16828c9ac45af33/0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAVLUN3P2BXJVCGW56%2F20230922%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230922T155751Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLWVhc3QtMSJHMEUCIQDv5SXtqUYq3M%2BTLmD8Koubrd6iF2rIPiKqnLsmmzLeCAIgQz3JDJwTr7uIwQX8CPkgerpgrlWFCh8UvbDB7Z3N4QcqhAMIGRAAGgwzNjg1OTA5NDU5MjMiDMy3TgBFKWhSMhz3YyrhAjeJV4J0s%2BaRRarmn5XlB3oEivhU3IDUMtvxPnEv9FCh8Kk6xCLpLxe7g%2B79oWXaN97r225z8N9anXdG1epGNBmN7DLR36e6S%2F91kv7ulZZcfHE9PeJYMbV%2FOt0ZY8rigMVHwY4BQRzJg78C%2BGErFTeO162XZLM3WQfrjop8Gu3gtz2Bo3E14KP7uY4VXBNszsQO729AUDsM7V5o7g3qHKeghOYwqubH88lIwDJ2wOjGkCeifRy4Vphf7SA7yyjHALt6QsBSWdHiIUNNy5Oh27OBiioOoF57r0jJDURUOmrKjmF0N4QDJhbTVWetf%2FfB19tCj%2F075fsHa%2Bpp1aYKlIZZamjl8XwOQaS1yDmLWmn5Wq%2BneOMt7E%2FHtn1P735%2Fl9fEwjqplgXkdAjs3sfhIM%2F0A8seyyogNGBQsM%2BuEjq%2FLBYkcSecUXZB8Vv1yWDv6ZfN1R%2FVrdVsL%2BCnViF2MZcpMO7ytqgGOp4BqGe%2BEpVW8Y%2BaMWECzadR94qjs1ecInPoqC4JXhvujplNeNcwULyedc5hDCqX60TgMVXJ8X3p5kIJgmbkRsvMi9bz%2BwTbWRapgslclYzXqiFDA73BDJ2VgEwdnCjHX%2F%2BLdOJdig6DxVsRYnCU%2B55CUSRqVbaliwUEKBWFVEAClKmUivfis8DGf1LJYW%2BT77pjFew23sRpJLBDfgq9fWw%3D&X-Amz-Signature=38c55f54a6fea0b0a4f3ba163ead9cc08dbe1b7f163ef5b4e78cb8f9e47ddb6b&X-Amz-SignedHeaders=host&x-id=PutObject"
    ],
    "__typename": "GetTrainingInstanceFileUploadUrlResponse"
  }
}
```


### Step 3B - Upload your binary data: 
Extract the `uploadUrls` form step 3 and then for each image you intend to upload do a `PUT` request with your binary data as the body to the url. Only one image per url. 
The exact code will vary depending on your language of choice. Feel free to ping me in the [Discord](https://discord.gg/zUEacFT) if you have questions.


### Step 4 - Associate your TrainingInstance with your TrainingModel:
#### GQL:
```
mutation createTrainingModelInstance($input: TrainingModelInstanceCreateInput!) {
  createTrainingModelInstance(input: $input) {
    _id
    trainingModelId
    trainingInstanceId
    __typename
  }
}
```

#### Variables:
```
{
  "input": {
    "trainingModelId": "650dad807c0c021e5b092ab7",
    "trainingInstanceId": "650dade3a16828c9ac45af33"
  }
}
```
Be sure to replace the values with the respective input values from step 1 and 3.

### Step 5 - Create a TrainingModelJob:
Now to actually trigger the code that trains the model on the backend you will need to create a `TrainingModelJob`.

#### GQL:
```
mutation createTrainingModelJob($input: TrainingModelJobCreateInput!) {
  createTrainingModelJob(input: $input) {
    _id
    model {
      _id
      user {
        _id
        username
        __typename
      }
      __typename
    }
    __typename
  }
}
```

#### Variables:
```
{
  "input": {
    "settings": {
      "baseModel": "{base model name}",
      "steps": 50,
      "learningRate": "1e-7",
      "extraRunArgs": []
    },
    "trainingModelId": "650dad807c0c021e5b092ab7"
  }
}
```
But replace `{base model name}` with a real Dreambooth base model and replace the `trainingModelId` with the `_id` you go from step 1.

#### Response:
```
{
  "data": {
    "createTrainingModelJob": {
      "_id": "650db8d0302d6c0c02e0f861",
      "model": {
        "_id": "650dad807c0c021e5b092ab7",
        "user": {
          "_id": "64529df5da89824740c1dcd0",
          "username": "schematical",
          "__typename": "User"
        },
        "__typename": "TrainingModel"
      },
      "__typename": "TrainingModelJob"
    }
  }
}
```


### Step 6 - Create an InferenceRequest:
Once your TrainingModel is done training(Between 5 and 30 minutes depending on how many image you are uploading) you can now create your InferenceRequests.
NOTE at the time of writing this you can create InferenceRequests even before the TrainingModel is done training and they will run as soon as the TrainingModel is done training(or at least get put in line to run).

#### GQL:
```
mutation createInferenceRequest($input: InferenceRequestCreateInput!) {
  createInferenceRequest(input: $input) {
    _id
    trainingModelId
    trainingModelVersionId
    prompt
    userId
    awsBatchJobId
    submittedAt
    completedAt
    user {
      _id
      username
      __typename
    }
    model {
      _id
      name
      namespace
      userId
      user {
        _id
        username
        __typename
      }
      __typename
    }
    __typename
  }
}
```

#### Variables:
```
{
  "input": {
    "prompt": {
      "prompt": "{ your prompt here }",
      "imageCount": 4,
     
    },
    "trainingModelId": "6463a9bdd445f8bff68a9a26"
  }
}
```
But replace `{ your prompt here }` with your actual prompt. Be sure to reference your instance by its namespace. 
Also replace the `trainingModelId` wil the `_id` you got from step 1.


### Step 7 - View your InferenceRequests:
#### GQL:
Finally, you want to view the results of your InferenceRequest. This will return urls you can use to view your resulting images.
```
query listInferenceRequest($input: InferenceRequestFilterInput) {
listInferenceRequest(input: $input) {    _id
    trainingModelId
    trainingModelVersionId
    prompt
    userId
    awsBatchJobId
    submittedAt
    completedAt
    user {
      _id
      username
      __typename
    }
    results(showArchived: false) {
      _id
      url
      __typename
    }
    __typename
  }
}
```
#### Variables:
```
{
  "input": {
    "pagination": {
      "limit": 8,
      "skip": 0,
      "orderBy": "createdAt",
      "direction": -1
    }
  }
}
```
#### Response:
```
{
  "data": {
    "listInferenceRequest": [
      {
        "_id": "650c95a42e039cb479826b9e",
        "trainingModelId": "6463a9bdd445f8bff68a9a26",
        "trainingModelVersionId": null,
        "prompt": {
          "prompt": "a portrait of (rvrctepppy) dog as  batman,  diffuse lighting, fantasy, intricate, elegant, highly detailed, lifelike, photorealistic, digital painting, illustration, concept art, smooth, sharp focus",
        },
        "userId": "64529df5da89824740c1dcd0",
        "awsBatchJobId": "964d3e96-1030-475b-ac7c-63e9feb2c665",
        "submittedAt": "2023-09-21T19:12:40.921Z",
        "completedAt": "2023-09-21T19:21:30.499Z",
        "user": {
          "_id": "64529df5da89824740c1dcd0",
          "username": "schematical",
          "__typename": "User"
        },
        "results": [
          {
            "_id": "650c97ba2bb642c66f32765f",
            "url": "https://schematical.com/schematical/models/646abcddd445f8bff68a9a26/inferenceRequests/650c95a42e039cb479826b9e/2023-09-21_19-21-24_out-0-wm.png?Expires=1695790800",
            "__typename": "InferenceResult"
          }
        ]
      }
    ]
  }
}
     
```
Notice the `url` field in the `results`. That URL will work for a short amount of time so you can download your results. 


## Coming soon:

### Private InferenceRequests:
Premium users will have the ability to create private IntereceRequests. 

### Webhooks:
I built the whole system using Event Driven Architecture, so it will be super simple for me to set up some type of Webhook system to tell you when training jobs and inferences are done so you don't have to poll the API.

Ping me on [Discord](https://discord.gg/zUEacFT) if you have questions.


### Support:
Interested in supporting me as I maintain these free scripts? Click the link below:

<a href="https://www.buymeacoffee.com/schematical" target="_blank">
    <img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" />
</a>






### Need Help:

#### Jump On The Discord:
This stuff can be a bit complex. Luckily we have a small community of people that like to help.
So head on over to the [Discord](https://discord.gg/F6cErPe6VJ) and feel free to ask any questions you might have.

#### Need more help:
I do consult on this so feel free to hop on over to [Schematical.com](https://schematical.com?utm_source=github_sc-terraform-cicd) and signup for a consultation.

