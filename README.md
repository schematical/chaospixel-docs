

Insert ERD

## GQL Getting Started:
### Step 0: Signup:
In reality there is not automated signup process. Ping @schematical on Discord here: https://discord.gg/zUEacFT

### Step 1: Create your Training Model
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


### Step 2: Create your TrainingInstance:
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



### Step 4: Associate your TrainingInstance with your TrainingModel:
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

### Step 5: Create a TrainingModelJob:
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
